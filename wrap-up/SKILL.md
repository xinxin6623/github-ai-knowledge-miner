---
name: wrap-up
description: 对话收尾沉淀技能。用户说"标准收尾"、"规则收尾"、"技能收尾"或 /wrap-up 时触发。智能判断本轮对话有什么值得保留，分别写入三个不同位置：完整存档（Archives）、任务接力单（项目下 relay-task.md）、用户画像与环境偏好（primitive）。同时联动更新 auto memory。强调 token 经济：摘要 not 原文，过程日志和搜索结果不留。
---

# 对话收尾 / Wrap-up

收到"标准收尾 / 规则收尾 / 技能收尾"时执行。目标：把这一轮对话里值得跨会话保留的信息，按用途分流到三个仓位，让下一轮（无论是否同一个 agent）能无缝接上。

## 三个仓位 — 各有定位，不重复

| 仓位 | 路径 | 写法 | 内容 |
|---|---|---|---|
| **Archives** 完整存档 | `/Users/macmini/baidu/Archives/YYYY-MM-DD-{slug}.md` | 每次新建文件 | 这轮对话的来龙去脉，决策、产出、踩坑、启示 |
| **Relay Task** 接力单 | `<cwd>/relay-task.md` | 整文件覆盖 | 下一个 agent 接手所需的最小完整信息（带指针） |
| **Primitive** 画像 | `/Users/macmini/baidu/primitive/{user,environment,preferences,workflow}.md` | 读后合并更新 | 跨项目通用、长期稳定的用户/环境/偏好事实 |

> 跟 auto memory（`~/.claude/projects/-Users-macmini-Documents-Codex/memory/`）的关系：Archives 长篇沉淀只在 Archives；Primitive 里的稳定事实**同时**同步一份到 auto memory（按 user/feedback/preference 分类）。relay-task 不进 auto memory（它是项目内交接，不是跨会话记忆）。

## 执行流程

1. **扫描对话**：识别四类信号
   - 任务进度信号 → relay-task
   - 决策 / 产出 / 踩坑 → Archives
   - 用户身份、机器环境、偏好表态 → primitive
   - 任意其中能归入 feedback/user/project/reference → auto memory

2. **智能取舍 — 默认跳过**：每个仓位独立判断，没有新增就不写。明确告诉用户"X 跳过，因为 ..."。

3. **Token 纪律**：
   - **不留**：搜索结果原文、grep/find 输出、完整文件 dump、tool 调用记录、失败的中间尝试
   - **留**：结论、决策、最终产出的关键片段、对未来有用的踩坑教训
   - 引用代码用 `path:line` 指针，不复制大段代码体

4. **写入前**：Primitive 必须先 Read 现有文件再合并；Archives 直接新建；relay-task 覆盖。

5. **同步 auto memory**：写完 primitive，把对应条目按 auto memory 规则也写一份过去（参考系统提示词里的 user/feedback/project/reference 分类）。

6. **回执**：用紧凑表格告诉用户每个仓位的动作和原因。

## 文件模板

### Archives 模板

文件名：`YYYY-MM-DD-{topic-slug}.md`（slug 用英文短横线或中文均可，简短可识别）

```markdown
---
date: YYYY-MM-DD
topic: <一句话主题>
project: <项目目录名 或 "无项目">
duration: <可选：本轮对话大致跨度>
---

# {topic}

## 背景
为什么做这件事，触发点是什么。

## 目标
本轮想达成什么。

## 关键决策
- 决策 1（为什么这么选 / 否决了什么备选）
- 决策 2

## 实施摘要
按步骤讲清楚做了什么。代码用指针：`<path>:<line>` 或贴最关键的 5-15 行。

## 产出物
- 文件：<path>（新建/修改）
- commit / PR：<hash 或 url>
- 配置变更：<path>

## 踩坑与启示
对未来类似任务有用的经验。失败路径如果有教育意义就留，否则不留。

## 未尽事项
本轮没做完但应该接着做的（如果有，同时写进 relay-task）。
```

### Relay Task 模板

固定路径：当前工作目录下 `relay-task.md`，整文件覆盖。

```markdown
# Relay Task

_updated: YYYY-MM-DD HH:MM_
_project: <repo / cwd>_
_branch: <git branch 如适用>_

## 任务
<2-3 句：在做什么、为什么>

## 当前进度
- [x] 已完成的步骤
- [~] 进行中（写明卡在哪）
- [ ] 未开始的步骤

## 下一步（具体到能直接动手）
1. 打开 `<file>:<line>`，做 X
2. 运行 `<command>`
3. 验证 Y

## 关键文件 / 路径
- 主代码：`<path>`
- 配置：`<path>`
- 测试：`<path>`
- 本轮存档（详细背景去这里翻）：`/Users/macmini/baidu/Archives/<file>.md`
- 相关 auto memory 条目：`<filename.md>`

## 阻塞 / 风险 / 待用户决策
- 列出还没解决的依赖、需要用户拍板的选项

## 上下文要点（接手前必读）
- 3-5 条让下一个 agent 立刻进入状态的关键事实
```

### Primitive 模板

四个维度，按需更新（`/Users/macmini/baidu/primitive/` 下）：

- `user.md` — 角色、所在领域、技术栈深度、当前关注的方向
- `environment.md` — OS / 机型 / 关键目录约定 / 常用工具与版本 / 路径白名单
- `preferences.md` — 沟通风格、输出长度偏好、代码风格、命名习惯、回复语言
- `workflow.md` — 项目组织习惯、常用 skill / 命令、典型协作流程

每个文件结构：

```markdown
# <Dimension>

_last updated: YYYY-MM-DD_

## 稳定事实
- 条目（首次确认 YYYY-MM-DD）
- 条目（首次确认 YYYY-MM-DD，YYYY-MM-DD 修正）

## 备注
偶尔有需要解释的上下文放这里。
```

更新规则：
- Read 现文件 → 比较 → 仅追加/修正变化的条目，保留原日期
- 矛盾就更新条目并加新日期注释，不删除历史日期
- 没有任何新事实就跳过这个维度

## 与 auto memory 联动

写完 Primitive 后，把对应条目按 auto memory 类型规则同步：
- `user.md` 内容 → auto memory `user` type
- `preferences.md` / `workflow.md` 偏好类 → `feedback` type
- 项目相关、阶段性的事实（即便从对话里抓到）→ `project` type，但**不要**写到 primitive
- 外部资源指针 → `reference` type

参考路径：`~/.claude/projects/-Users-macmini-Documents-Codex/memory/`，遵循该目录的 `MEMORY.md` 索引规范。

## 回执格式

执行完后给用户简短报告：

```
收尾完成 ✓

Archives  → /Users/macmini/baidu/Archives/2026-05-05-xxx.md  (新建, ~约 N 字)
Relay     → <cwd>/relay-task.md  (覆盖)
Primitive → preferences.md 新增 1 条；user.md 跳过（无新信息）；environment.md 跳过；workflow.md 跳过
Memory    → 同步 2 条：feedback_xxx.md, user_role.md

未保留：本轮的 grep 结果、build 日志、3 次失败的命令尝试 — 对未来无价值。
```

## 边界情况

- **没有 cwd 项目**（在家目录或临时目录调用）：跳过 relay-task，告诉用户"不在项目目录，未生成 relay"
- **对话很短或纯闲聊**：可能三个仓位都跳过，明确说"本轮无需沉淀"
- **用户在对话中已经显式让你保存过某事**：不要重复保存，但可以在回执里指出"已在对话中保存"
- **存档主题难以一句话概括**：拆成两个 Archives 文件，分别命名

## 不要做的事

- 不要把整段 tool output 复制进 Archives
- 不要为了凑内容而保留无信息量的"完成了 X"
- 不要在 Primitive 里写项目特定信息（那是 project memory 的事）
- 不要静默跳过 — 每个跳过都要在回执里说明理由
