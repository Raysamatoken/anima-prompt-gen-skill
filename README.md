# Anima Prompt Generator Skill

一个用于生成 **Anima 文生图模型** 提示词的 [Codex](https://codex.ai) 技能，基于结构化的 **Anima3 v3.0** 模板系统。

## 功能

将用户需求（中文或英文）编译为符合 Anima 模型规范的生产级英文提示词。采用严格的槽位填充工作流，内置自检验证机制。

## 目录结构

```
anima-prompt-gen-skill/
├── SKILL.md                          # 核心工作流：槽位顺序、输出协议、自检清单
├── agents/
│   └── openai.yaml                   # Codex UI 元数据
├── README.md                         # 本文件
└── references/
    └── anima3-template.md            # 完整 v3.0 模板及标签库
```

## 工作流

```
用户需求 → 匹配场景类型（决策树）→ 按槽位顺序填充标签
→ 6 项自检 → 互斥冲突检查 → 单行输出
```

### 槽位填充顺序

```
[人数/性别] → [角色/系列] → [外貌] → [服装/状态]
→ [姿势/动作/性行为] → [表情/反应] → [镜头/视角]
→ [场景/环境] → [细节/氛围]
→ [自然语言补充]
```

### 核心规则

- **输出格式**：单行、全小写、逗号分隔、无 markdown、禁止质量词/画师名/光线标签
- **标签数量**：单人 16-30 / 双人 22-38 / 复杂 30-48
- **单人默认**：除非指定背影/侧脸，否则必须注入 `direct eye contact, facing viewer`
- **多人规则**：必须为每个角色补充关键外貌描述；关系/动作用自然语言放在末尾
- **跨槽位一致性**：古风配古风、赛博配赛博、日常配日常——不能出现世界观矛盾

## 标签库（位于 `references/anima3-template.md`）

| 章节 | 内容 |
|---|---|
| §5 | 7 类场景决策树 |
| §6-7 | 人数/身份/外貌标签 |
| §8 | 服装改造引擎 |
| §9 | 姿势、动作、体位库 |
| §10 | 表情与反应强度映射 |
| §11-13 | 镜头、场景、细节与氛围 |
| §14 | 12 个特殊主题跨槽位配方 |

## 在 Codex 中使用

```markdown
Use $anima-prompt-gen to generate Anima model prompts following the
structured v3.0 template. Reference the tag libraries in /references
and follow the slot-based assembly workflow.
```

## 许可

MIT
