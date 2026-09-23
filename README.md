# Personal Core Skills

可独立安装的个人 Codex Skills。

| Skill | 用途 |
| --- | --- |
| [ai-book](skills/ai-book/SKILL.md) | 编写机制驱动的中文技术书、可运行配套实验与在线阅读页面，参考《深入理解 AI Agent》的教学组织方式 |

## 安装 ai-book

将 `skills/ai-book` 整个目录放入 Codex 的用户 Skills 目录，默认是 `~/.codex/skills/ai-book`。如果设置了 `CODEX_HOME`，使用它下面的 `skills` 目录。已有同名 Skill 时先比较内容，避免直接覆盖。

例如，在尚未克隆本仓库、目标目录不存在时：

```bash
git clone https://github.com/Kaeless/personal-core-skills.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R personal-core-skills/skills/ai-book "${CODEX_HOME:-$HOME/.codex}/skills/ai-book"
```

让客户端重新加载 Skills，随后在支持 Skill 调用的会话中使用：

```text
使用 $ai-book，给有 Linux 基础的读者写一章 XDP 入门，
沿收包路径解释代码，并提供一个可以运行、观察和修改的实验。
```

生成在线书籍时，可明确要求完整阅读体验：

```text
使用 $ai-book 将指定书稿制作成在线书籍，参考阅读站点的首页、三栏布局、
深浅主题、字号调整、代码复制、图片查看和移动端阅读体验，保留单一书稿来源。
```

网页规范及按交付范围选择的阅读交互见 [在线阅读指南](skills/ai-book/references/web-reading.md)。

也可用于改写已有章节：

```text
使用 $ai-book 改写指定教程。保留实际代码和命令，
补充状态变化、失败分支与实验结果的解释，不增加未实现的功能。
```

Skill 不依赖专用插件或在线 API。生成的实验是否需要网络、模型或特权，取决于所选主题。

## 来源与验证范围

`ai-book` 的来源快照、正文样本、实验代码样本及提炼边界见 [来源记录](skills/ai-book/references/sources.md)。规则和缓存示例为重新编写的内容，不包含原书全文、图片或实验源码，不代表原作者背书。

发布前检查 Skill 格式、相对文件链接，并执行原创缓存示例，验证缓存命中、到期更新和对照隔离。格式检查与示例通过不代表生成的所有章节或外部实验均已验证。

网页指南已对照参考站点的组件、样式和阅读状态源码补充；本次没有可连接的浏览器，未进行原站的视觉或交互实测。后续生成网站时须单独完成相应验收。
