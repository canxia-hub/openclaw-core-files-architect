# OpenClaw Core Files Architect

<p align="center">
  <img src="https://img.shields.io/badge/version-2.0-blue.svg" alt="version">
  <img src="https://img.shields.io/badge/format-XML%20First-green.svg" alt="format">
  <img src="https://img.shields.io/badge/license-MIT-yellow.svg" alt="license">
</p>

Audit, design, refactor, and continuously maintain the OpenClaw core files system, using XML as the primary format by default, with Markdown fallback templates for branch derivatives, enabling agents to achieve higher stability, maintainability, and long-term memory quality with less context waste.

审计、设计、重构并持续维护 OpenClaw 核心文件体系，默认以 XML 作为更优主格式，并保留 Markdown 基础模板以支持分支衍生，使 agent 在更少上下文浪费下获得更高稳定性、可维护性与长期记忆质量。

## Why XML-First? | 为什么 XML 优先？

| Advantage | Description |
|-----------|-------------|
| **Clearer Boundaries** | Personality, rules, preferences can be placed in different tags, preventing cross-contamination |
| **More Stable Local Edits** | Modifying `<rule id="memory-writeback">` is less likely to accidentally affect other content than modifying mixed paragraphs |
| **Multi-Model Collaboration** | Different models can usually identify explicit tags and attributes relatively stably |
| **Programmatic Maintenance** | Can be maintained with scripts, hooks, search-replace, diff comparison for specific nodes |
| **Branch Evolution** | Can easily generate Markdown versions, simplified versions, channel versions, sub-agent versions |

| 优势 | 说明 |
|------|------|
| **边界更清晰** | 人格、规则、偏好可放进不同标签，不容易互相污染 |
| **局部编辑更稳** | 修改特定节点比修改混合段落更不容易误伤 |
| **多模型协作** | 不同模型通常都能较稳定识别显式标签与属性 |
| **程序化维护** | 可用脚本、hook、搜索替换、差异比较去维护特定节点 |
| **分支演化** | 可轻松再生成 Markdown 版、精简版、渠道版、子 agent 版 |

## Features | 功能特性

- 📐 **Core File Architecture Design** - Design and audit AGENTS.md, SOUL.md, USER.md, TOOLS.md, MEMORY.md, HEARTBEAT.md
- 🔄 **XML-First Format** - Use XML structured format by default for better parseability
- 🧩 **Markdown Fallback** - Support Markdown templates for compatibility
- ✅ **Quality Validation** - Validate file structure, length limits, and responsibility boundaries
- 🔍 **Self-Diagnosis** - Diagnose common issues like bloated context, rule conflicts, memory pollution

## Installation | 安装

### Via ClawHub (Recommended)

```bash
clawhub install openclaw-core-files-architect
```

### Manual Installation

```bash
# Clone to your OpenClaw skills directory
git clone https://github.com/canxia-hub/openclaw-core-files-architect.git
cd openclaw-core-files-architect
# Copy to your OpenClaw skills directory
cp -r . ~/.openclaw/skills/openclaw-core-files-architect/
```

## Usage | 使用方法

```bash
# Invoke the skill
/openclaw-core-files-architect

# Or in your AGENTS.md, reference it:
# references:
#   - name: openclaw-core-files-architect
#     path: ~/.openclaw/skills/openclaw-core-files-architect/SKILL.md
```

## XML Root Tags | XML 专用根标签

| File | XML Root Tag |
|------|-------------|
| AGENTS.md | `<agent_contract>` |
| SOUL.md | `<soul_profile>` |
| USER.md | `<user_profile>` |
| TOOLS.md | `<tool_notes>` |
| MEMORY.md | `<long_term_memory>` |
| HEARTBEAT.md | `<heartbeat>` |
| IDENTITY.md | `<identity_profile>` |
| BOOTSTRAP.md | `<bootstrap>` |

## Files | 文件说明

- `SKILL.md` - Main skill definition and usage guide
- `INITIAL_UNIVERSAL_XML_TEMPLATE.md` - Universal XML template for initial setup

## Related Skills | 相关技能

- [agent-creator](https://github.com/canxia-hub/agent-creator) - Agent creation wizard using this skill's principles

## License | 许可证

MIT License - See [LICENSE](LICENSE) for details.

## Author | 作者

小千 (Xiao Qian) - OpenClaw Agent

---

<p align="center">
  <a href="https://openclaw.ai">OpenClaw</a> • 
  <a href="https://clawhub.com">ClawHub</a>
</p>
