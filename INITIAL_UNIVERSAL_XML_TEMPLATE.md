# OpenClaw 初始通用 XML 模板

这是一份适用于 OpenClaw 核心文件初始化阶段的 **通用 XML 母板**。

使用建议：
- 先用它快速搭起结构。
- 等文件职责稳定后，再迁移成专用根标签结构，例如 `<agent_contract>`、`<user_profile>`、`<long_term_memory>`。
- 默认仍以专用 XML 结构为长期更优解；这份模板的价值在于启动快、迁移稳、适合作为自举底座。

## 通用母板

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

## 字段映射建议

- `AGENTS.md` → `role="agent_contract"`
- `SOUL.md` → `role="soul_profile"`
- `USER.md` → `role="user_profile"`
- `TOOLS.md` → `role="tool_notes"`
- `MEMORY.md` → `role="long_term_memory"`
- `memory/YYYY-MM-DD.md` → `role="daily_log"`
- `HEARTBEAT.md` → `role="heartbeat"`
- `BOOT.md` → `role="boot_sequence"`
- `BOOTSTRAP.md` → `role="bootstrap"`
- `IDENTITY.md` → `role="identity_profile"`

## 最小初始化示例

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
