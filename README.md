# dsh-memory-preset ｜ 记忆优先预设

一个 agent 预设：上下文里只留最近一轮原文，更早的历史交给记忆库检索。适合长期聊天和陪练，不适合要完整上下文的编码会话。

## 做什么

- 覆盖出厂预设中的一行（`preset-memory`），工具集只做减法
- 每轮把更早的轮次折叠掉（靠 `dsh-context-window`）
- persona 里写清「旧原文不在上下文是常态」，要求先 `companion_memory_search` 再回答
- 保留：文件读写、文件搜索、技能、web 搜索 / 抓取、问用户、交付

## 不做什么

- 不提供 shell、jobs、schedule、goal、plan-mode、compaction、subagent / workflow、todo
- 不替你检索：记忆库和注入要另外装 `dsh-memory-store`

## 要求

| 项 | 要求 |
| --- | --- |
| DSH | 0.2.2-rc.1 |
| 依赖 | `dsh-memory-store`（记忆工具）、`dsh-context-window`（折叠） |
| 安装 | 三者都作为 bundle 挂上，重启后在预设列表里选「记忆优先」 |

## 安装

1. 先把 `dsh-memory-store` 与 `dsh-context-window` 装好
2. profile 的 `package.json` 加 `"dsh-memory-preset": "link:D:/plugins/dsh-memory-preset"`，并加进 `dsh.profile.bundles`
3. 重启 DSH，在会话设置里把预设换成「记忆优先」

## 注意

- 折叠是模型侧的：事件日志和客户端聊天记录都还在，只是模型这一轮看不到旧原文
- 记忆库为空时它会正常聊天，不会解释机制
- 要长期记住的事得显式写进记忆库，它不会自动总结全部历史


## 风险

- 只留最近一轮原文，忘事、答错的概率比完整上下文高
- 记忆检索结果来自本机库，库里的东西会进模型请求
- 不适合长文档、长代码这类要完整上下文的活
- 记忆写错会一直错下去，得人工纠正

---

作者：温凌微（Vera），网名暗海拾星。个人自用后整理开源，不是官方项目。
