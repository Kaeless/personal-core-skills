# 来源记录与提炼边界

参考：李博杰《深入理解 AI Agent：设计原理与工程实践》。

- [用户指定的阅读站点](https://bojieli.github.io/ai-agent-book/astro/)
- [配套源码仓库](https://github.com/bojieli/ai-agent-book)
- 本次核对日期：2026-09-23。
- 源码快照：`22fd9c5041a378ff0019d91fe22fed9482f8b128`。
- 阅读方式：读取网站入口，浏览中文章节开头与目录，重点抽样第 1、2、3、7 章相关段落及对应实验 README、实现和测试；没有逐字审阅全书，也没有运行上游全部实验。后续补读同一快照的网页 README、首页/阅读器组件、全局/阅读样式和阅读位置实现；浏览器无可用连接，网页规则基于源码核对，未做原站视觉或交互实测。

下列链接固定到已读取的版本，便于重新核对。这里描述的是样本中的做法，不推断全仓库处处一致。

## 正文与实验中观察到的做法

| 来源 | 已观察内容 | 对应提炼 |
| --- | --- | --- |
| [第 1 章：最小循环与轨迹](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/book/chapter1.md#L162-L232) | 先给核心对象与边界，再给明确标注的 Python 风格伪代码，以及具体消息轨迹 | 最小机制骨架与一次运行互相解释；区分伪代码和可执行实现 |
| [第 2 章开头](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/book/chapter2.md#L1-L30) | 从前章概念转入上下文问题，用工程师缺少产品背景的场景解释信息需求 | 章间承接已有概念；类比落回具体对象 |
| [第 3 章开头](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/book/chapter3.md#L1-L34) | 将单次会话管理推进到跨会话记忆，并提供具体对话 | 章节按能力与限制推进；复用读者已知对象 |
| [第 7 章：运行轨迹分析](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/book/chapter7.md#L85-L127) | 结合具体消息分析成功与失败，讨论终态通过时仍有过程问题 | 沿证据定位原因；验证器判定与过程质量分开 |
| [第 1 章实验导航](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter1/README.md) | 从离线轨迹进入单任务与消融，再读入口和核心实现；区分本地项目、外部复现与设计文档 | 为初学者提供最短阅读与运行路径，标明实现状态 |
| [搜索实验 README](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter1/web-search-agent/README.md#L1-L95) | 解释工具定义、服务端执行、结果回传，再给离线命令与轨迹观察方法 | 命令附近说明观察点；离线与联网职责明确 |
| [搜索入口](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter1/web-search-agent/main.py#L1-L145)与[核心模块](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter1/web-search-agent/agent.py#L1-L130) | 参数与使用模式位于入口，轨迹分类、工具适配与失败判断可定位；JSON 保存问答和运行信息 | 入口、核心与输出分工；结构化结果支持复盘 |
| [上下文压缩实验 README](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter2/context-compression/README.md#L1-L165)及[策略实现](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter2/context-compression/compression_strategies.py#L1-L140) | 先介绍问题与策略差异，区分交互、对照与日志脚本；策略枚举、结果 dataclass 与分派可直接阅读 | 基线与处理条件显式表示；按用途拆分，按需要采用类型化结果 |
| [记忆检索离线演示](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter3/agentic-rag-for-user-memory/offline_demo.py#L1-L150) | 本地检索、规则驱动的多轮查询、证据覆盖与逐步轨迹分开呈现 | 交代离线替代了哪个环节，避免将规则演示解释为模型能力 |
| [评估纯逻辑](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter7/android-world/experiment_core.py#L1-L165)与[测试样本](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter7/android-world/test_experiment.py#L1-L100) | 报告逻辑与模拟器导入分离；记录运行错误、有效配对、成本与证据不足；测试脱敏和特定异常边界 | 可独立验证的计算与外部环境分离；失败和证据不足成为结果的一部分 |
| [第 1 章实验台账](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/chapter1/EXPERIMENT_LEDGER.md) | 记录具体运行条件、服务受限情况、消融实现修正及不被实验支持的旧判断 | 预期服从证据；历史结果附带适用条件 |

## 网页风格的补充来源

以下同样固定在上述快照。指南以源码中的实际配置为起点，包含明确标注的适用范围与新增验收建议。

| 来源 | 核对内容 | 对应规则 |
| --- | --- | --- |
| [网页项目说明](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/web-astro/README.md) | 功能、存储与备份范围、渐进增强、单一正文来源、部署、图形适配和已知限制 | 阅读交互、来源一致性、部署与降级要求；不将未实现搜索列为原站能力 |
| [首页组件](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/web-astro/src/components/Home.astro) | 书籍首屏、前置知识、阅读入口、章节卡片与实验入口 | 首页信息顺序与内容导航 |
| [阅读器组件](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/web-astro/src/components/Reader.astro) | 目录、工具栏、页间导航、代码复制和折行、滚动进度 | 页面区域与增强交互 |
| [全局样式](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/web-astro/src/styles/global.css) | 深浅色变量、字体栈、焦点与基础布局 | 语义色板与字体起点 |
| [阅读器样式](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/web-astro/src/styles/reader.css) | 三列宽度、正文排版、引用块、代码、多个断点与底部安全区域 | 响应式排版与不遮挡正文的移动控制 |
| [阅读位置数据](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/web-astro/src/lib/reading-position.ts) | 章节、小节、相对偏移、版本校验及存储失败处理 | 恢复状态的数据约束与降级 |

## 补缺核对

| 维度 | 修订后的归属 | 边界 |
| --- | --- | --- |
| 正文讲解与章节结构 | `writing.md` | 已有内容保留 |
| 实验实现、运行与证据 | `labs.md` | 已有内容保留，未运行上游全部实验 |
| 首页、三栏阅读器、主题与排版 | `web-reading.md` | 新增，具体值可调整 |
| 阅读状态、笔记、移动交互与可访问性 | `web-reading.md` | 新增，按所需页面范围实现 |
| 代码、图表、公式、脚注与复制 | `writing.md` + `web-reading.md` | 补充网页呈现与交互要求 |
| 单一内容来源、图形数据保真与部署路径 | `web-reading.md` | 新增，避免网页改版损坏内容 |
| 来源证据与验收边界 | 本文件 + `web-reading.md` | 来源检查不等于浏览器实测 |

未打包原站 CSS、组件或现成网站模板；当前交付是可迁移的 Skill 指南。多语言、全文搜索、云端同步和自动部署不是通用默认功能。交付具体网站时，仍需按项目需求实现并验证。

## 本 Skill 新增的综合建议

本 Skill 将上述做法整理为适用于其他技术领域的工作流程；以下内容是综合后的质量要求，不能据此声称原书所有代码都满足：

- 根据实验规模决定单文件或拆分，优先遵循目标仓库语言和规范。
- 新建在线实验不静默回退为 mock；若显式回退，记录真实模式与原因。
- 统计公开分母、缺失项与失败成本，避免仅比较成功样本。
- HTML 结构检查、交互验收、代码验证与真实环境运行分别报告。
- 只记录接口实际提供的可观察轨迹，不虚构模型内部思维过程。

原代码样本存在中英文混合注释、不同缩进方式、终端装饰和宽泛异常处理。Skill 提炼的是教学结构、职责划分及证据意识，没有将这些表面细节规定为统一代码风格。

原书中的模型名称、服务参数和性能数字没有被固化为本 Skill 的技术前提。使用者写具体主题时，应自行核对当前实现和官方资料。

## 归属

[上游许可证](https://github.com/bojieli/ai-agent-book/blob/22fd9c5041a378ff0019d91fe22fed9482f8b128/LICENSE)标注 Apache License 2.0、Copyright 2025 Bojie Li。本 Skill 提供独立编写的规则、评论与原创缓存示例，保留来源链接；未分发原书正文、插图或实验实现，也不声称获得原作者认可。
