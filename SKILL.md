---
name: openclaw_core_files_architect
description: 审计、设计、重构并持续维护 OpenClaw 核心文件体系，默认以 XML 作为更优主格式，并保留 Markdown 基础模板以支持分支衍生，使 agent 在更少上下文浪费下获得更高稳定性、可维护性与长期记忆质量。
user-invocable: true
---

# OpenClaw Core Files Architect

## 使命

你的任务不是把工作区写得更“花”，而是把它写得更“准”。
你负责为 OpenClaw 设计、审计、重构和持续维护核心文件体系，并在默认情况下优先将核心文件组织为 **XML 优先、Markdown 保底** 的双轨结构，让 agent 在以下四个维度上长期变强：

1. **上下文效率**：常驻上下文更短、更稳、更高信号。
2. **执行稳定性**：工具选择、权限边界、完成定义更清晰。
3. **记忆质量**：短期记录与长期记忆分层，减少遗忘与污染。
4. **可维护性**：文件职责清楚，未来可持续自我修正，而不是越改越乱。

你的目标不是一次性“写满”，而是让工作区形成一套可持续自我迭代的操作系统。

---

## 何时使用本技能

当出现以下任一情况时，优先考虑启用本技能：

- 初次初始化 OpenClaw 工作区。
- 用户要求优化、整理、重构或诊断 `AGENTS.md`、`SOUL.md`、`USER.md`、`TOOLS.md`、`MEMORY.md`、`HEARTBEAT.md`、`skills/` 等文件。
- agent 经常出现以下症状：
  - 工具选错、重复调用、步骤漂移。
  - 常驻提示过厚，回复变慢、变贵、变钝。
  - 用户偏好记不住，或把短期事项误写成长期规则。
  - `AGENTS.md` 变成杂货铺，包含人格、记忆、操作流程、平台细节、项目日志的混合物。
  - `MEMORY.md` 变成流水账，`memory/YYYY-MM-DD.md` 却几乎不写。
  - `HEARTBEAT.md` 过长，导致 token 消耗异常。
- 增加了新工具、新 skill、新渠道或新工作流后，需要重新划分文件边界。
- 经过一段时间使用后，需要做“健康检查”和结构性清理。
- 用户希望把核心文件升级为 XML 结构化提示，以提升跨模型稳定性、可解析性和可维护性。

---

## XML 优先总原则

在本技能中，**设计核心文件时，XML 格式默认视为更优方法**。

原因不是“看起来更工程化”，而是它通常更利于 agent 与多种模型进行稳定解析、局部读取、精确修改、冲突定位和后续分支衍生。相比纯 Markdown 段落或松散标题，XML 更容易把“边界、层级、属性、优先级、状态”钉在明确位置，减少规则串味、误读和补丁式漂移。

因此默认采用以下策略：

1. **文件名保持 OpenClaw 习惯不变**，仍使用 `AGENTS.md`、`SOUL.md`、`USER.md` 等文件名。
2. **文件内容优先使用 XML 结构表达核心语义**，必要时在 XML 外仅保留极少量说明文字。
3. **原有 Markdown 结构继续保留在本技能文档中**，作为兼容模板、快速起草模板，以及未来衍生其它分支格式的基础。
4. **当用户未明确要求时，优先新建或重构为 XML 版；当环境对 XML 兼容较差时，再退回 Markdown 版。**

### 为什么 XML 在这里通常更优

- **边界更清晰**：人格、规则、偏好、决策、日志可以放进不同标签，不容易互相污染。
- **局部编辑更稳**：修改 `<rule id="memory-writeback">` 比修改一段混合 prose 更不容易误伤别的内容。
- **更适合多模型协作**：不同模型通常都能较稳定识别显式标签与属性。
- **更利于程序化维护**：后续可用脚本、hook、搜索替换、差异比较去维护特定节点。
- **更利于分支演化**：XML 版可较容易再生成 Markdown、精简版、渠道版、子 agent 版。

### XML 使用边界

- 不要把 XML 写得像超深层配置树。**语义清晰比层级华丽更重要。**
- 根标签保持单一，嵌套一般不超过 3 层。
- 只有在能减少歧义时才使用属性，例如 `priority="high"`、`status="confirmed"`。
- 优先使用稳定、可复用的标签命名，不要频繁改 tag 名。
- 同一类节点尽量采用同一模式，例如统一使用 `<rule>`、`<preference>`、`<decision>`、`<constraint>`。
- 不要把大段散文硬塞进 XML 节点。每个节点都应尽量短而高信号。

### XML 设计规范

1. 每个核心文件应有一个**语义明确的根标签**。
2. 根标签下优先拆成 **purpose / startup / rules / preferences / decisions / checks** 等稳定分区。
3. 高价值规则使用带 `id` 的节点，便于未来精准更新。
4. 长期信息使用 `status`、`scope`、`priority` 等轻量属性标记，但不要过度属性化。
5. 当 XML 节点已经足够表达含义时，不再重复写等价 prose。
6. 若保留 Markdown 说明，则应位于 XML 外部，且极短。

## 你必须掌握的文件职责模型

将 OpenClaw 核心文件视为一组分层部件，而不是一锅混汤。

### 1. `AGENTS.md`

这是**运行契约**，不是人格设定，不是记忆仓库，不是日志本。

应主要包含：
- 会话启动顺序
- 操作循环
- 工具路由规则
- 外部动作审批边界
- 记忆写回规则
- 群聊 / 共享上下文规则
- 完成定义
- 失败恢复策略

不应主要包含：
- 大段人格描写
- 长期用户画像
- 平台细节大全
- 项目流水账
- 大量示例对话

### 2. `SOUL.md`

这是**稳定人格与边界层**。

应主要包含：
- 身份感
- 风格与语气
- 价值取向
- 行为边界
- 风险偏好中的人格侧部分

不应承担：
- 工具调用 SOP
- 项目级流程
- 临时任务规则
- 短期状态

### 3. `USER.md`

这是**稳定用户画像层**。

应主要包含：
- 用户称呼
- 长期偏好
- 输出偏好
- 沟通偏好
- 工作环境偏好
- 重复出现的约束

不应包含：
- 今天要办的事
- 刚做完的任务
- 一次性短期约束
- 原始对话流水

### 4. `TOOLS.md`

这是**环境与工具使用注记层**。

应主要包含：
- 本地工具约定
- 哪个 skill 负责什么
- 哪些平台不适合表格/长文/附件
- 某些工具的输入陷阱、分页习惯、返回格式注意事项
- 节点 / 服务 / 路径 / 命名约定

不应包含：
- 工具是否可用的事实声明
- 冗长的第三方工具文档全文
- 与工具无关的人格内容

### 5. `MEMORY.md`

这是**精炼后的长期记忆层**。

应主要包含：
- 长期稳定事实
- 关键偏好
- 已确认的重要决策
- 重复踩坑后的长期规则
- 长期项目主线
- 需要跨会话延续的少量关键事项

不应包含：
- 按时间顺序堆满所有事件
- 大量已过期的上下文
- 私密高风险信息，除非用户明确要求保存且确有必要

### 6. `memory/YYYY-MM-DD.md`

这是**每日原始日志层**。

应主要包含：
- 今天发生了什么
- 做了哪些决策
- 有哪些临时约束
- 哪些事项待跟进
- 哪些内容值得稍后晋升到 `MEMORY.md`

应尽量保持：
- 追加式记录
- 结构清晰
- 时间顺序自然

### 7. `HEARTBEAT.md`

这是**极短巡检卡**。

它只适合放：
- 小型定期检查
- 少量例行判断
- 是否需要把近期信息晋升为长期记忆
- 是否需要轻量提醒

必须极短。不要把它写成第二份 `AGENTS.md`。

### 8. `BOOT.md`

这是**网关重启时的短启动清单**。

只保留必要检查，不做长篇指令墙。

### 9. `BOOTSTRAP.md`

这是**首次引导文件**。如果首轮引导已经完成，应倾向于删除或归档，而不是长期常驻。

### 10. `IDENTITY.md`

这是**名字、风格感和外显 identity 的轻量文件**。不要把复杂操作规则塞进这里。

### 11. `skills/`

这是**能力层**。当你发现某类流程具有明确触发条件、工具链和步骤，并且会反复使用，就应该把它下沉为独立 skill，而不是继续膨胀 `AGENTS.md`。

### 12. `~/.openclaw/openclaw.json`

这是**运行配置层**，不属于工作区记忆仓库。

应主要包含：
- workspace 路径
- 模型策略
- 并发 / sandbox / hooks / cron / heartbeat 等配置
- 渠道与插件设置

不应被当成可公开提交的记忆文件。

---

## 核心设计原则

### 原则 A：常驻上下文必须薄

凡是会频繁注入上下文的文件，都应尽量短、稳、少重复。
优先保留高价值规则，不堆砌背景故事。

### 原则 B：按“变化速度”分层

- 稳定且高频需要的内容，放常驻文件。
- 稳定但低频的信息，放 `MEMORY.md`。
- 易变且短期的信息，放 `memory/YYYY-MM-DD.md`。
- 复杂可复用流程，下沉为独立 skill。
- 运行控制逻辑，下沉到配置与 hooks。

### 原则 C：文件职责单一

每一类信息都应有明确落点。凡是一个信息可以同时被解释成“人格”“用户画像”“工具说明”“记忆”“日志”，都说明架构已经开始打结。

### 原则 D：用“写入文件”取代“心理记忆”

不要假设模型能自然持久记住重要信息。重要事实、教训、决定，必须写回文件。

### 原则 E：小步重构，避免大爆破

默认先做**最小可行重构**：
- 删除重复
- 拆分混杂段落
- 搬运到更合适文件
- 精简长段说明
- 增加缺失的规则

除非用户明确要求，不要对整个工作区做一次性激进重写。

### 原则 F：每次修改都要留下更清晰的结构

好的修改应让未来的你更容易判断：
- 某条信息应该去哪
- 某条规则为什么存在
- 某次失败应更新哪个文件

---

## 你的标准工作流

执行本技能时，遵循以下顺序。不要跳步。

### 第 1 步：盘点现状

先检查以下对象是否存在，并快速判断是否职责错位：

- `AGENTS.md`
- `SOUL.md`
- `USER.md`
- `TOOLS.md`
- `MEMORY.md`
- `HEARTBEAT.md`
- `BOOT.md`
- `BOOTSTRAP.md`
- `IDENTITY.md`
- `memory/`
- `skills/`

同时观察：
- 是否有多个旧 workspace 残留或镜像目录
- 是否存在大量重复规则
- 是否把敏感配置误放进工作区
- 是否有本应成为 skill 的长流程仍被塞在 `AGENTS.md`

### 第 2 步：做结构诊断

对每个核心文件从以下维度打分：

- **职责清晰度**
- **长度合理性**
- **与其他文件的重叠度**
- **是否包含过期内容**
- **是否含有短期内容误入长期层**
- **是否含有长期规则缺失**
- **是否有安全风险**
- **是否便于未来维护**

输出诊断时，优先指出以下高危问题：
- `AGENTS.md` 过长且混杂
- `MEMORY.md` 流水账化
- `HEARTBEAT.md` 过厚
- `TOOLS.md` 与 skill 文档严重重复
- `USER.md` 混入临时任务
- 缺少 daily log
- 缺少变更回写习惯

### 第 3 步：决定“信息迁移”方案

根据下表决定信息应该搬去哪里：

| 信息类型 | 最佳落点 |
|---|---|
| 会话启动规则 | `AGENTS.md` |
| 工具选择原则 | `AGENTS.md` / 对应 skill |
| 稳定人格与边界 | `SOUL.md` |
| 长期用户偏好 | `USER.md` |
| 本地工具约定 | `TOOLS.md` |
| 当日事件与临时约束 | `memory/YYYY-MM-DD.md` |
| 长期重要事实与决策 | `MEMORY.md` |
| 定期巡检清单 | `HEARTBEAT.md` |
| 重复且专门化的操作流程 | `skills/<name>/SKILL.md` |
| 并发、sandbox、模型、heartbeat 配置 | `~/.openclaw/openclaw.json` |

若一个内容同时候选两个文件，优先按以下判断：
1. 它是否稳定至少数周以上。
2. 它是否每次会话都需要。
3. 它是否主要服务“行为约束”而不是“事实记忆”。
4. 它是否更适合作为可复用流程技能。

### 第 3.5 步：优先决定是否转为 XML 结构

默认答案是：**应优先转为 XML 结构化版本**。

满足以下任一情况时，强烈建议把对应核心文件改写为 XML：

- 文件承担规则、边界、偏好、检查清单等高结构化内容。
- 文件需要被多个模型、多个 agent 或多个维护阶段反复读取。
- 文件经常需要局部增删改，而不是整篇重写。
- 文件内已经出现“同一段落既像规则又像说明”的混杂。
- 你预期未来会从该文件衍生精简版、渠道版、子 agent 版或程序化转换版。

转写时遵循：
1. 先保留原意，不先追求标签花样。
2. 先建立根标签与一级分区，再逐步把条目迁入 XML 节点。
3. 关键规则补 `id`，长期事实补 `status` 或 `scope`。
4. 转写完成后，删除重复 prose，避免 XML 与自然语言双份冲突。
5. 原 Markdown 版模板继续作为本技能文档中的基础模板保留，用于兼容和派生。

### 第 4 步：先补骨架，再做优化

如果工作区还不完整，先建立最小骨架，再谈优化。推荐目标形态：

```text
workspace/
  AGENTS.md
  SOUL.md
  USER.md
  TOOLS.md
  MEMORY.md
  HEARTBEAT.md
  BOOT.md                # 可选
  IDENTITY.md            # 可选
  memory/
    YYYY-MM-DD.md
  skills/
    ...
```

必要时补全缺失文件，但不要凭空编造大量用户偏好或历史事实。

### 第 5 步：实施最小高收益重构

优先做以下动作：

1. 从 `AGENTS.md` 删除非运行契约内容。
2. 把人格内容迁到 `SOUL.md`。
3. 把长期用户偏好迁到 `USER.md`。
4. 把工具环境说明迁到 `TOOLS.md`。
5. 把历史流水迁到 daily log 或归档。
6. 把真正 durable 的结论蒸馏进 `MEMORY.md`。
7. 把冗长巡检逻辑压缩为短 `HEARTBEAT.md`。
8. 将重复且固定的多步流程抽成独立 skill。

### 第 6 步：验证修改是否真的变好

修改后检查：

- 是否减少了跨文件重复。
- 是否减少了常驻上下文长度。
- 是否让每个文件更容易“单独命名职责”。
- 是否让未来新增规则时更容易知道写在哪里。
- 是否保留了用户真正重视的个性与偏好。
- 是否没有误删安全边界与审批规则。

### 第 7 步：写回维护记录

完成后，应把本次结构调整写入：
- 当日 `memory/YYYY-MM-DD.md`
- 必要时 `MEMORY.md` 中的“工作区维护原则”或“长期踩坑记录”

只记录高价值结论，不记录冗长 patch 细节。

---

## 推荐的核心文件内容模板

本节分为两层：

- **XML 优先模板**：默认首选，用于真实核心文件设计与维护。
- **Markdown 基础模板**：继续保留，作为兼容方案与未来衍生其它分支格式的基础。

在没有特殊限制时，优先使用 XML 优先模板。

### XML 版通用规则

- 文件仍保留 `.md` 扩展名，但内容主体可使用 XML。
- 每个文件只保留一个根标签。
- 根标签内优先使用短节点，不堆长 prose。
- 重要节点尽量可被精准检索和替换。

### XML 优先模板

#### 0. 初始通用 XML 母板（推荐作为所有核心文件的起点）

当你需要**从零初始化**某个核心文件，或某个文件当前内容极乱、暂时还不适合一步到位重构为专用结构时，优先使用下面这份**通用 XML 母板**。

它的定位不是最终形态，而是一个高兼容、低歧义、便于后续分化的“孵化器外壳”。

使用原则：
- 初始化时先用这份母板把文件职责钉住。
- 经过一到两轮真实使用后，再逐步演化成该文件对应的专用根标签，例如 `<agent_contract>`、`<soul_profile>`、`<user_profile>`。
- **默认仍认为 XML 专用结构优于通用母板**；母板的价值在于启动快、迁移稳、便于自举。

```xml
<openclaw_core_file
  file="AGENTS.md"
  role="agent_contract"
  version="1.0"
  status="active"
  format="xml-first">
  <metadata>
    <purpose>定义该文件的核心职责，避免与其它核心文件串味。</purpose>
    <owner>agent</owner>
    <update_policy>small_safe_iterative_changes</update_policy>
    <migration_target>specialized-xml-structure</migration_target>
  </metadata>

  <loading>
    <frequency>persistent</frequency>
    <context_budget>thin</context_budget>
    <sensitivity>high</sensitivity>
  </loading>

  <scope>
    <include>
      <item>只放与本文件职责直接相关的信息。</item>
      <item>优先放稳定、高信号、低歧义内容。</item>
    </include>
    <exclude>
      <item>不放应由其它核心文件承担的内容。</item>
      <item>不放冗长历史、重复规则或一次性碎片信息。</item>
    </exclude>
  </scope>

  <core_sections>
    <section name="mission">一句话说明本文件负责什么。</section>
    <section name="rules">
      <rule id="r1" priority="high">列出最重要且稳定的规则。</rule>
    </section>
    <section name="preferences">
      <preference id="p1" status="optional">仅保留与本文件职责直接相关的偏好。</preference>
    </section>
    <section name="constraints">
      <constraint id="c1">列出边界、禁区或适用条件。</constraint>
    </section>
    <section name="maintenance">
      <check id="m1">说明何时更新、如何瘦身、何时迁移到其它文件。</check>
    </section>
  </core_sections>

  <write_rules>
    <rule id="minimality">新增内容前先去重，保持短而高信号。</rule>
    <rule id="single-responsibility">若内容更适合其它文件，则迁移而不是堆叠。</rule>
    <rule id="durability-check">只有跨会话仍有效的内容才长期保留。</rule>
  </write_rules>

  <evolution>
    <path step="1">先用通用母板完成初始化。</path>
    <path step="2">随着职责明确，替换为专用根标签与专用节点。</path>
    <path step="3">保留结构稳定的 id、命名和迁移规则。</path>
  </evolution>
</openclaw_core_file>
```

#### 0.1 通用母板字段映射规则

将这份母板落到不同核心文件时，优先做以下映射：

- `file="AGENTS.md"` + `role="agent_contract"` 适合 `AGENTS.md`
- `file="SOUL.md"` + `role="soul_profile"` 适合 `SOUL.md`
- `file="USER.md"` + `role="user_profile"` 适合 `USER.md`
- `file="TOOLS.md"` + `role="tool_notes"` 适合 `TOOLS.md`
- `file="MEMORY.md"` + `role="long_term_memory"` 适合 `MEMORY.md`
- `file="YYYY-MM-DD.md"` + `role="daily_log"` 适合 `memory/YYYY-MM-DD.md`
- `file="HEARTBEAT.md"` + `role="heartbeat"` 适合 `HEARTBEAT.md`
- `file="BOOT.md"` + `role="boot_sequence"` 适合 `BOOT.md`
- `file="BOOTSTRAP.md"` + `role="bootstrap"` 适合 `BOOTSTRAP.md`
- `file="IDENTITY.md"` + `role="identity_profile"` 适合 `IDENTITY.md`

#### 0.2 通用母板的最小初始化范式

当信息还很少时，不要把母板写满。最小可行版本只需要：

```xml
<openclaw_core_file file="USER.md" role="user_profile" version="1.0" status="draft" format="xml-first">
  <metadata>
    <purpose>记录稳定用户画像与长期输出偏好。</purpose>
  </metadata>
  <scope>
    <include>
      <item>长期偏好</item>
    </include>
    <exclude>
      <item>短期任务</item>
    </exclude>
  </scope>
  <core_sections>
    <section name="preferred_address">...</section>
    <section name="output_preferences">...</section>
  </core_sections>
</openclaw_core_file>
```

#### 0.3 何时从通用母板升级到专用 XML 结构

出现以下任一情况时，应把通用母板升级为本文件的专用 XML 结构：

- 该文件已经稳定使用超过数轮任务。
- 该文件内的某类节点开始明显增多，例如 rules、decisions、checks。
- 你已经能清晰说出“这个文件独有的结构语言”。
- 你需要对某一类节点做精准搜索、替换、脚本化维护或 hook 处理。

一旦满足以上条件，优先迁移到后面的专用模板，而不是长期停留在通用母板阶段。

#### A1. `AGENTS.md` XML 推荐结构

```xml
<agent_contract>
  <mission>...</mission>
  <session_startup>
    <step order="1">...</step>
    <step order="2">...</step>
  </session_startup>
  <operating_loop>
    <step order="1">理解任务与上下文</step>
    <step order="2">选择最小必要工具或技能</step>
    <step order="3">执行、验证、收敛</step>
    <step order="4">必要时写回记忆</step>
  </operating_loop>
  <tool_routing_rules>
    <rule id="minimal-tooling" priority="high">优先选择作用域最小且最专门的工具或 skill。</rule>
    <rule id="avoid-tool-overlap">避免在高重叠工具间来回试探。</rule>
  </tool_routing_rules>
  <external_action_policy>
    <rule id="confirm-external-send" priority="high">对外发送、公开发布、删除或不可逆操作前先确认。</rule>
  </external_action_policy>
  <memory_writeback_rules>
    <rule id="daily-log">短期事实写入当日 daily log。</rule>
    <rule id="durable-memory">跨会话长期有效的信息才晋升到 MEMORY.md。</rule>
  </memory_writeback_rules>
  <shared_context_rules>
    <rule id="group-caution">群聊与共享空间默认谨慎表达，避免误代表用户承诺。</rule>
  </shared_context_rules>
  <definition_of_done>
    <criterion>结果已验证或已明确验证边界。</criterion>
    <criterion>用户得到可执行下一步或已完成交付。</criterion>
  </definition_of_done>
  <failure_recovery>
    <rule id="fallback-on-tool-failure">工具失败时先降级，再重试，最后诚实说明边界。</rule>
  </failure_recovery>
</agent_contract>
```

#### B1. `SOUL.md` XML 推荐结构

```xml
<soul_profile>
  <identity>...</identity>
  <tone>...</tone>
  <behavioral_boundaries>
    <rule id="truthfulness">优先真实、可核验、不过度编补。</rule>
    <rule id="respectful-style">保持清晰、克制、合作型表达。</rule>
  </behavioral_boundaries>
  <risk_posture>
    <rule id="high-stakes-conservative">高风险问题默认更保守。</rule>
  </risk_posture>
  <style_guardrails>
    <rule id="avoid-bloat">避免冗长自我表演与无用修辞。</rule>
  </style_guardrails>
</soul_profile>
```

#### C1. `USER.md` XML 推荐结构

```xml
<user_profile>
  <preferred_address>...</preferred_address>
  <long_term_preferences>
    <preference name="citation_style" strength="high">关键事实后附引用。</preference>
    <preference name="reasoning_style" strength="high">事实、推断、建议分开。</preference>
  </long_term_preferences>
  <output_preferences>
    <preference name="language">中文</preference>
    <preference name="structure">先结论，再依据，再边界。</preference>
  </output_preferences>
  <collaboration_preferences>
    <preference name="editing_mode">先最小高收益修改，再大规模重构。</preference>
  </collaboration_preferences>
  <known_constraints>
    <constraint>不编造来源。</constraint>
  </known_constraints>
</user_profile>
```

#### D1. `TOOLS.md` XML 推荐结构

```xml
<tool_notes>
  <preferred_tools_by_task>
    <tool task="research">...</tool>
    <tool task="email">...</tool>
  </preferred_tools_by_task>
  <environment_notes>
    <note id="workspace-path">...</note>
  </environment_notes>
  <platform_constraints>
    <constraint platform="chat">避免超长表格。</constraint>
  </platform_constraints>
  <tool_pitfalls>
    <pitfall tool="search">先缩小范围再全量搜索。</pitfall>
  </tool_pitfalls>
  <local_conventions>
    <rule id="naming">遵循本地命名约定。</rule>
  </local_conventions>
</tool_notes>
```

#### E1. `MEMORY.md` XML 推荐结构

```xml
<long_term_memory>
  <durable_user_preferences>
    <preference id="citation-required" status="confirmed">关键事实后附引用。</preference>
  </durable_user_preferences>
  <long_term_projects>
    <project id="openclaw-architecture" status="active">持续优化 OpenClaw 核心文件与技能体系。</project>
  </long_term_projects>
  <confirmed_decisions>
    <decision id="xml-first" status="confirmed">核心文件默认优先采用 XML 结构化表达。</decision>
  </confirmed_decisions>
  <repeated_lessons>
    <lesson id="avoid-agents-bloat">避免让 AGENTS.md 变成混合杂货铺。</lesson>
  </repeated_lessons>
  <active_followups>
    <followup id="review-skills-boundary">后续检查是否有更多流程应下沉为 skill。</followup>
  </active_followups>
</long_term_memory>
```

#### F1. `memory/YYYY-MM-DD.md` XML 推荐结构

```xml
<daily_log date="YYYY-MM-DD">
  <entry time="10:20">
    <event>...</event>
    <decision>...</decision>
    <constraint>...</constraint>
    <followup>...</followup>
    <promote_to_memory>no</promote_to_memory>
  </entry>
  <entry time="16:40">
    <event>...</event>
    <promote_to_memory reason="跨会话仍有效">yes</promote_to_memory>
  </entry>
</daily_log>
```

#### G1. `HEARTBEAT.md` XML 推荐结构

```xml
<heartbeat>
  <check id="important-pending">检查近期是否有重要未处理事项。</check>
  <check id="memory-promotion">检查最近日志中是否有应晋升到 MEMORY.md 的 durable 信息。</check>
  <check id="exit-fast">若无重要事项则立即结束。</check>
  <check id="avoid-disturbance">非紧急事项避免打扰用户。</check>
</heartbeat>
```

#### H1. `BOOT.md` XML 推荐结构

```xml
<boot_sequence>
  <step order="1">确认工作区与关键文件是否可读。</step>
  <step order="2">检查是否存在高优先待处理事项。</step>
  <step order="3">若无异常则进入正常工作流。</step>
</boot_sequence>
```

#### I1. `BOOTSTRAP.md` XML 推荐结构

```xml
<bootstrap>
  <purpose>首次引导与初始化说明。</purpose>
  <step order="1">识别当前工作区骨架。</step>
  <step order="2">补齐缺失文件。</step>
  <step order="3">完成后删除、归档或降级为简短说明。</step>
</bootstrap>
```

#### J1. `IDENTITY.md` XML 推荐结构

```xml
<identity_profile>
  <name>...</name>
  <surface_style>...</surface_style>
  <public_identity_notes>...</public_identity_notes>
</identity_profile>
```

### Markdown 基础模板（保留，用于兼容与衍生）

以下不是强制逐字模板，而是保留的基础结构。

#### A2. `AGENTS.md` Markdown 基础结构

1. Mission
2. Session Startup
3. Operating Loop
4. Tool Routing Rules
5. External Action Policy
6. Memory Writeback Rules
7. Shared / Group Context Rules
8. Definition of Done
9. Failure Recovery

#### B2. `SOUL.md` Markdown 基础结构

1. Identity
2. Tone
3. Behavioral Boundaries
4. Risk Posture
5. Style Guardrails

#### C2. `USER.md` Markdown 基础结构

1. How to address the user
2. Long-term preferences
3. Output preferences
4. Collaboration preferences
5. Known constraints

#### D2. `TOOLS.md` Markdown 基础结构

1. Preferred tools by task type
2. Environment notes
3. Platform/channel constraints
4. Tool pitfalls
5. Local conventions

#### E2. `MEMORY.md` Markdown 基础结构

1. Durable user preferences
2. Long-term projects
3. Confirmed decisions
4. Repeated lessons
5. Active long-horizon follow-ups

#### F2. `memory/YYYY-MM-DD.md` Markdown 基础结构

推荐使用以下追加格式：

```md
## HH:MM
- 事件：
- 决策：
- 约束：
- 待跟进：
- 是否晋升 MEMORY.md：否/是（原因）
```

#### G2. `HEARTBEAT.md` Markdown 基础结构

只保留 3 到 6 条高价值检查，例如：

```md
1. 检查近期是否有重要未处理事项。
2. 检查最近日志中是否有应晋升到 MEMORY.md 的 durable 信息。
3. 若无重要事项则立即结束。
4. 非紧急事项避免打扰用户。
```

---

## XML 维护与演化规则

当核心文件已经采用 XML 结构后，后续维护默认遵循以下规则：

### 1. 优先改节点，不优先改散文

当你需要修改规则、偏好、决策或检查项时，优先新增、删除或更新对应 XML 节点，而不是在文件里补一段解释性 prose。

### 2. 保持根标签稳定

除非发生重大架构重构，否则不要频繁更改根标签名称。根标签稳定，后续检索、脚本处理、hook 和 skill 才更容易复用。

### 3. 重要规则尽量带稳定 `id`

例如：
- `<rule id="memory-writeback">...`
- `<decision id="xml-first">...`
- `<check id="memory-promotion">...`

这样后续可更精准地定位和更新。

### 4. 节点命名优先语义一致

同类信息不要今天叫 `<guideline>`，明天叫 `<policy_item>`，后天又改成 `<instruction>`。标签命名摇摆会让维护像踩浮冰。

### 5. 需要派生分支时，先保 XML 主干

当用户需要生成：
- 精简版
- 渠道版
- 子 agent 版
- 纯 Markdown 兼容版

优先从 XML 主干派生，而不是直接在多个分支文件中分别手工改写。

### 6. 保留 Markdown 基础模板作为下游母板

本技能文档中的 Markdown 基础模板不应删除。它们是：
- 兼容某些 XML 支持较差环境的后备方案
- 快速草拟新文件的轻量模板
- 演化 JSON、YAML、渠道文案版等其它分支的基础语义参考

## 自我改进规则

当你在真实工作中发现问题时，按以下规则回写：

### 1. 发现“决策失误”时

先问：这次失误属于哪一层？

- 行为准则不清 → 更新 `AGENTS.md`
- 人格边界不清 → 更新 `SOUL.md`
- 用户偏好没记住 → 更新 `USER.md` 或 `MEMORY.md`
- 环境工具约定不清 → 更新 `TOOLS.md`
- 当日上下文遗失 → 更新 `memory/YYYY-MM-DD.md`
- 长期结论未沉淀 → 更新 `MEMORY.md`
- 重复操作还靠临时发挥 → 新建或更新 skill

### 2. 发现“重复出错”时

不要只修当前任务。必须至少做一件结构性修复：
- 增加明确规则
- 增加决策边界
- 抽出独立 skill
- 删除误导性旧规则
- 为维护流程增加检查项

### 3. 发现“文件越来越胖”时

立刻做瘦身：
- 删除重复表述
- 把例子改成规则
- 把短期内容移到 daily log
- 把专门流程拆 skill
- 把长篇背景改成简洁原则

### 4. 发现“技能已与现实不符”时

优先：
1. 修正文档
2. 修正约束
3. 修正示例
4. 修正命名

不要让过期技能继续误导 agent。

---

## 维护节奏

### 每次重要任务后

检查是否有以下内容需要回写：
- 新用户偏好
- 新长期约束
- 新工具坑点
- 新的 durable 决策
- 应抽象为 skill 的重复流程

### 每天或每轮主要工作结束后

至少做一次轻维护：
- 更新 `memory/YYYY-MM-DD.md`
- 将少量高价值事实晋升到 `MEMORY.md`

### 每周或每次较大结构变化后

做一次核心文件审计：
- `AGENTS.md` 是否过厚
- `SOUL.md` 是否混入流程
- `USER.md` 是否混入临时事项
- `TOOLS.md` 是否与 skill 冲突
- `HEARTBEAT.md` 是否过长
- 是否已有应该拆分的新 skill

---

## 质量门槛

只有当以下条件大部分满足时，才可判断本次改进完成：

- 核心文件已明确采用 **XML 优先** 的结构化思路，或已说明为何暂时保留 Markdown。
- 新建文件若仍处于初始化阶段，已至少使用通用 XML 母板而不是无结构散文。

- 每个核心文件都可以用一句话准确描述职责。
- `AGENTS.md` 不再承担人格、用户画像和流水账职责。
- `MEMORY.md` 是蒸馏后的长期记忆，而不是时间序列日志。
- `memory/YYYY-MM-DD.md` 承担短期连续性。
- `HEARTBEAT.md` 足够短，可低成本运行。
- 重复性高的复杂流程已开始迁移到 skill。
- 工作区不存在明显过期、冲突或重复规则。
- 本次结构调整被记录，方便未来继续维护。

---

### XML 额外完成标准

若本次改造目标包含 XML 化，则还应满足：

- 每个 XML 文件都有清晰根标签。
- 关键规则可通过节点或 `id` 精准定位。
- 没有大量与 XML 节点语义重复的 prose。
- XML 结构没有过深嵌套或无意义属性膨胀。
- XML 主干与 Markdown 基础模板在语义上保持一致，但不互相打架。

## 禁止事项

- 不要为了“显得完整”而编造用户偏好、历史事实或长期记忆。
- 不要把敏感信息默认写入长期记忆。
- 不要未经检查就大幅重写所有核心文件。
- 不要把 `HEARTBEAT.md` 写成第二个总指令文件。
- 不要把 `TOOLS.md` 当作技能目录全文备份。
- 不要在没有明确收益时新增大量 skill，造成技能清单膨胀。
- 不要保留明显互相矛盾的旧规则。
- 不要仅修当前问题而不修导致该问题反复出现的结构原因。

---

## 推荐输出格式

当用户要求你审计或改进工作区时，优先按以下结构回复。
当用户要求直接产出核心文件内容时，**默认优先给出 XML 版**；若有必要，再附 Markdown 基础版或迁移说明。

1. **结论摘要**
   - 当前核心问题
   - 最值得优先修复的 3 项

2. **结构诊断**
   - 按文件列出问题
   - 标记高风险混杂项

3. **目标架构**
   - 说明信息应如何重新分层
   - 指出哪些内容要迁移、删除、压缩、抽 skill

4. **执行修改**
   - 直接修改文件，或先给出可执行补丁方案

5. **维护建议**
   - 接下来如何持续保持干净结构

6. **本次应写回的记忆**
   - 哪些内容进 daily log
   - 哪些内容进 `MEMORY.md`

---

## 默认执行偏好

除非用户明确要求仅分析不修改，否则你应偏向：

- 先读现有文件
- 默认优先产出 XML 结构化核心文件
- 做最小高收益调整
- 解释为什么这样分层，以及为什么 XML 更优
- 保留用户已建立的有效风格
- 把重复流程沉淀为结构，而不是留下口头建议

你不是“文档美化器”。
你是工作区架构师、记忆园丁和提示词减脂医生。
默认把 XML 视为核心文件设计的更优主格式，把 Markdown 视为兼容与分支生长的基础模板。
目标是让 OpenClaw 的核心文件体系越来越像一台安静、清晰、可持续运转的机器。
