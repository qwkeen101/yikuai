---
name: spec
description: 把 yikuai 当前块的数据结构、效果图和决定综合成 spec.md，再拆成能独立验证的任务。
disable-model-invocation: true
---

# 规格

设计已经问完，这一步只综合：把这一块已有的材料写成规格，再拆成任务。

先读 `${CLAUDE_PLUGIN_ROOT}/docs/flow-files.md`；spec.md 格式见同目录 [SPEC-FORMAT.md](SPEC-FORMAT.md)。

## 步骤

1. **收材料。** 读 blocks.md，当前块应在「规格」；不在时告诉用户还差哪一步，然后结束。读本块 data.md、`docs/design/<NN-slug>/`、decisions.md、parked.md 里本块的条目、GLOSSARY.md、相关 ADR，再看代码里本块要动的模块和可参照的已有测试。

   完成标准：知道每条决定、每个搁置条目、要动的模块和可参照的测试。

2. **写 spec.md。** 按 SPEC-FORMAT.md 写。综合时发现还有没定的事，按推荐答案写进规格，同时记进 parked.md，在规格「假设」里列出。

   完成标准：data.md 每个实体、decisions.md 每条「已定」、效果图每个页面都在规格里有着落；本块在 parked.md「待确认」里的每一条都列在「假设」。

3. **拆任务。** 每个任务是一条**纵向切片**：从数据、接口到界面打通一个用户能感知的行为，做完能单独验证。要先做的重构或数据迁移单独成任务，排在最前。每个任务写验收标准和依赖。

   完成标准：spec.md 每条用户故事都有任务覆盖；依赖无环；每个任务的验收标准都能用测试或手动操作判定。

4. **确认清单。** 用编号列表给用户看：标题、依赖、交付什么。用户说好，或调整粒度和依赖。

   完成标准：用户确认清单。

5. **写入。** 按 blocks.md 头部的任务存放方式写任务：本地文件写 `tickets/NN-<slug>.md`；GitHub Issues 按依赖顺序建 issue，链接列进 spec.md「任务」。状态改为「实现中」，告诉用户下一步运行 `/yikuai:build`。

   完成标准：每个任务都有文件或 issue，状态是「实现中」。
