---
name: setup
description: 在当前仓库初始化 yikuai 流程：选任务存放位置，建 docs/flow/ 骨架，在 CLAUDE.md 或 AGENTS.md 里加一段指引。每个仓库跑一次。
disable-model-invocation: true
---

# 初始化

给当前仓库装上 yikuai 流程要用的文件。文件约定见 `${CLAUDE_PLUGIN_ROOT}/docs/flow-files.md`，向用户提问照 `${CLAUDE_PLUGIN_ROOT}/docs/asking.md`。

## 步骤

1. **探查。** 查清这些事：
   - 是不是 git 仓库，`git remote -v` 指向哪里；
   - 根目录有没有 `CLAUDE.md`、`AGENTS.md`，其中是否已有 `## 开发流程（yikuai）` 一段，CLAUDE.md 是否只是 `@AGENTS.md` 这样的引用；
   - `docs/flow/`、`GLOSSARY.md`、`docs/adr/` 是否已存在；
   - 代码里数据结构定义在哪：建表迁移、ORM 模型、schema 文件、共享类型、接口定义。

   完成标准：每一项都有答案，数据结构位置写成具体路径（没有就写「无」）。

2. **问一轮。** 用几行告诉用户查到了什么，再问一轮，只问探查定不下来的：
   - 任务存放：推荐本地文件。用户选 GitHub Issues 时，在仓库里运行 `gh repo view`，能看到目标仓库才算接好；看不到就停下，告诉用户缺什么。
   - 指引写在哪：已有 CLAUDE.md 就写它；CLAUDE.md 只引用 AGENTS.md 时写 AGENTS.md；只有 AGENTS.md 就写它；两个都没有时问用户建哪个（推荐 CLAUDE.md）。

   完成标准：任务存放和指引文件都定了。

3. **建骨架。** 按约定写 `docs/flow/blocks.md`（头部填任务存放和现有结构，表格只有表头）和 `docs/flow/parked.md`（两个只有表头的表）。文件已存在时只补缺的部分，原有内容保持原样。

   完成标准：两个文件都在，格式符合约定。

4. **加指引。** 在选定的文件末尾加下面这段；已有这一段就原地替换：

   ```markdown
   ## 开发流程（yikuai）

   需求按块设计和实现，进度和结论都在 `docs/flow/`：`blocks.md` 是块清单和每块的当前步骤，`parked.md` 是先按推荐答案往下走、等用户确认的问题。术语以 `GLOSSARY.md` 为准，架构决定在 `docs/adr/`。接手时先运行 `/yikuai:next`。
   ```

   完成标准：这一段在文件里恰好一份，文件其余部分和原来一样。

5. **收尾。** 告诉用户建了哪些文件、改了哪个文件，下一步运行 `/yikuai:slice` 把需求切块。
