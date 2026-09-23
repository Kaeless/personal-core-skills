# 原创示例：缓存少读了几次，也可能读到旧值

本例用于展示本 Skill 的讲解与代码粒度，未复制参考书正文或实验代码。它是确定性的本地实验，不涉及模型服务，也不测量生产性能。

## 从一次旧值开始

配置源最初保存 `mode=A`。程序第一次读取后把它缓存三秒，第二秒配置源改成 `B`。这时再次查询会返回什么？

判断取决于缓存有效期。这里约定缓存项保存 `(value, expires_at)`：只有 `now < expires_at` 时使用旧条目，到期时重新读取。配置源的变化不会主动通知缓存。

下面是完整实验。将代码保存为 `demo.py`，使用 Python 3.10 或更新版本执行 `python3 demo.py`。它只依赖标准库，逻辑时间由实验输入提供，无需真的等待三秒。生产代码的超时判断通常需要单调时钟，本例省略了真实时钟、并发访问和多键管理。

```python
"""比较固定 TTL 缓存与直接读取，观察读取次数和旧值。"""

import json
from dataclasses import dataclass


@dataclass
class Source:
    value: str = "A"
    reads: int = 0

    def read(self) -> str:
        self.reads += 1
        return self.value


class TtlCache:
    def __init__(self, source: Source, ttl: float):
        if ttl <= 0:
            raise ValueError("ttl 必须大于 0")
        self.source = source
        self.ttl = ttl
        self.entry: tuple[str, float] | None = None

    def get(self, now: float) -> tuple[str, str]:
        # 到期边界必须重新读取，否则旧值会多保留一次请求。
        if self.entry is not None and now < self.entry[1]:
            return self.entry[0], "hit"
        value = self.source.read()
        self.entry = (value, now + self.ttl)
        return value, "miss"


def run(use_cache: bool) -> dict:
    # 每个条件使用独立数据源，避免前一轮污染后一轮。
    source = Source()
    cache = TtlCache(source, ttl=3)
    trace = []
    for now in range(4):
        if now == 2:
            source.value = "B"
        if use_cache:
            value, event = cache.get(now)
        else:
            value, event = source.read(), "direct"
        trace.append({"time": now, "event": event, "value": value,
                      "source_value": source.value,
                      "stale": value != source.value})
    return {"strategy": "ttl" if use_cache else "direct",
            "source_reads": source.reads, "trace": trace}


if __name__ == "__main__":
    print(json.dumps([run(False), run(True)], ensure_ascii=False, indent=2))
```

`get()` 的第一次调用没有缓存项，所以读取数据源并保存截止时间 `3`。在第二秒，即便数据源已经改为 `B`，判断 `2 < 3` 仍成立，函数直接返回缓存中的 `A`。第三秒判断不成立，才会读取并保存 `B`。

两次 `run()` 各自新建数据源，收到同样的时间序列和更新事件。唯一的对照变量是是否通过缓存读取。

## 怎样读结果

运行后先比较 `source_reads`，再逐行查看 `trace`，特别是时间 `2` 与 `3`。以下是由代码推导的预期，不是某次真实服务的测量记录：

| 时间 | 数据源 | 直接读取 | TTL 缓存 | 缓存事件 |
| --- | --- | --- | --- | --- |
| 0 | A | A | A | miss |
| 1 | A | A | A | hit |
| 2 | B | B | A | hit |
| 3 | B | B | B | miss |

直接读取预期访问数据源四次；缓存访问两次，但第二秒返回旧值。这说明减少读取和保持新鲜度需要一起观察。实验没有网络、锁竞争或序列化开销，因此不能由读取次数减半推导出延迟减半。

## 改一个条件

将 `ttl=3` 改为 `ttl=2` 后重新运行。第二秒恰好过期，更新可以被读到。这个结果依赖更新与请求的时间关系：换成其他更新时刻，仍可能返回旧值。

检查到期边界时，可以故意将 `<` 改成 `<=`，观察第三秒是否仍返回旧值；确认后恢复。这个变更让读者验证一个实际的语义差异。

继续思考：若要求更新后下一次请求必然读到新值，单纯缩短 TTL 是否足够？需要怎样的失效通知或一致性机制？后续章节可由这个未满足的要求引出新的设计。
