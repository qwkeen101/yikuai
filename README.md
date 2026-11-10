# yikuai（一块）

一次只做一块的 Claude Code 开发流程技能包。

把大需求切成几块，每块依次走完「定结构 → 看界面 → 追问 → 规格 → 实现」，再做下一块。每个结论当场写进仓库里的文件，随时能停，换个会话也能接着做。

## 为什么

全栈开发在设计阶段有几样常见的负担：

- 数据结构没先定，后面不断加表、加相似字段，表结构越来越乱。
- 界面没先给产品看，做完才发现不对，只能返工。
- 「一直追问到所有问题问完」的设计访谈很耗脑力，几个需求同时设计更累。
- 大需求拆出的小需求，有的能并行做，有的互相依赖，顺序不理清就会互相卡住。

## 六条原则

1. **一次只设计一块。** 设计不并行；并行只用在实现阶段互不依赖的任务上。
2. **数据结构先行。** 每一块先定实体、字段、关系、状态，再谈别的。
3. **界面先看再做。** 有界面的块先出效果图，用户点头再往下。
4. **追问有上限。** 每轮最多 5 题，每题带推荐答案，回「好」就是接受推荐；能从代码、文档、环境里查到的事实自己查，不问用户。
5. **可以搁置。** 不想现在答的题按推荐答案先走，记进待确认清单，之后统一回看，不阻塞进度。
6. **随时能停。** 每个结论当场写进文件；新会话靠文件接着做，不靠对话记忆。

## 流程

```text
/yikuai:setup            每个仓库一次
      │
/yikuai:slice            大需求 → 若干块，标依赖，排设计顺序
      │
      ▼  每块依次走，同一时刻只有一块在设计
/yikuai:schema   定结构   → data.md
/yikuai:mock     看界面   → docs/design/<块>/    （无界面的块跳过）
/yikuai:ask      追问     → decisions.md、parked.md
/yikuai:spec     规格     → spec.md、tickets/
/yikuai:build    实现     → 代码、测试、提交    （互不依赖的任务可并行）
      │
      ▼  下一块

随时可用：/yikuai:next 看进度、接着做；/yikuai:review-parked 回看待确认
任何一题都可以回「讲讲」：先由 /yikuai:explain 讲明白，再接着问
```

| 步骤 | 技能 | 做什么 | 做完的标志 | 块状态变为 |
|---|---|---|---|---|
| 0 | `setup` | 建 `docs/flow/` 骨架，在 CLAUDE.md 或 AGENTS.md 加指引 | 骨架和指引都在 | |
| 1 | `slice` | 切块、标依赖、排顺序 | 用户确认块清单 | 待设计 |
| 2 | `schema` | 定实体、字段、关系、状态，优先复用 | 用户确认 data.md | 看界面 / 追问 |
| 3 | `mock` | 出四种状态的效果图，核对数据 | 用户确认效果图 | 追问 |
| 4 | `ask` | 每轮最多 5 题，可搁置 | 未决问题清空 | 规格 |
| 5 | `spec` | 写规格，拆纵向切片任务 | 用户确认任务清单 | 实现中 |
| 6 | `build` | 先写测试，按依赖实现、提交 | 全部任务完成，测试通过 | 完成 |

## 安装

### 插件方式

在 Claude Code 里运行：

```text
/plugin marketplace add qwkeen101/yikuai
/plugin install yikuai@yikuai
```

### 本地目录方式

```bash
git clone https://github.com/qwkeen101/yikuai.git ~/src/yikuai

# 只在这次会话里加载
claude --plugin-dir ~/src/yikuai
```

也可以把本地目录当作插件市场长期安装：在 Claude Code 里运行 `/plugin marketplace add ~/src/yikuai`，再 `/plugin install yikuai@yikuai`。

技能会引用包里的 `docs/` 目录，请按插件整体加载，不要只拷贝 `skills/`。

## 快速开始

1. 在项目仓库里运行 `/yikuai:setup`。
2. 运行 `/yikuai:slice`，后面跟上需求描述或需求文档路径。
3. 按提示依次运行 `/yikuai:schema`、`/yikuai:mock`、`/yikuai:ask`、`/yikuai:spec`、`/yikuai:build`。
4. 新会话开头运行 `/yikuai:next`，或直接说「接着做」。

## 技能一览

| 技能 | 一句话 | 触发 |
|---|---|---|
| `setup` | 在仓库里初始化流程：选任务存放位置，建 `docs/flow/` 骨架，加指引 | 用户 |
| `slice` | 把大需求切成不超过 7 块，标依赖、排设计顺序 | 用户 |
| `schema` | 给当前块定数据结构，先盘点已有实体，新增要说明为什么不能复用 | 用户或自动 |
| `mock` | 给当前块出正常、空、加载、出错四种状态的效果图，界面暴露的数据问题回写 data.md | 用户或自动 |
| `ask` | 对当前块短追问，每轮最多 5 题、带推荐答案、可搁置 | 用户或自动 |
| `spec` | 不再提问，把数据结构、效果图和决定综合成规格，拆成能独立验证的任务 | 用户 |
| `build` | 按依赖顺序实现任务，先写测试，做完一个提交一个 | 用户 |
| `next` | 几行说明在哪一块哪一步、下一步运行什么、有几条待确认 | 用户或自动 |
| `review-parked` | 回看待确认清单，一次最多 5 条，改了答案就指出影响 | 用户 |
| `explain` | 把没看懂的题、术语或方案用大白话讲明白，只讲做这个决定要懂的，讲过的记下来不重讲 | 用户或自动 |

「自动」指 Claude 会在合适的时候自己调用，例如说「接着做」时由 `next` 接上 `schema`、`mock` 或 `ask`，回「讲讲」时由 `explain` 接上；「用户」指只有你输入命令才会运行。

## 在项目里生成的文件

```text
docs/flow/
  blocks.md                 块清单：编号、名称、范围、依赖、界面、状态、当前步骤
  parked.md                 待确认清单
  learned.md                讲解记录
  <NN-slug>/
    data.md                 数据结构
    decisions.md            未决问题和已定决定
    spec.md                 规格
    tickets/NN-<slug>.md    任务
docs/design/<NN-slug>/      效果图
GLOSSARY.md                 术语表
docs/adr/NNNN-*.md          架构决定记录
```

各文件的格式和块的状态机见 [docs/flow-files.md](docs/flow-files.md)，提问规矩见 [docs/asking.md](docs/asking.md)。

## 致谢

流程思路受 [mattpocock/skills](https://github.com/mattpocock/skills) 启发（MIT 许可）；讲解步骤（`explain`）的思路参考了其中的 teach 技能。

## 许可

[MIT](LICENSE)
