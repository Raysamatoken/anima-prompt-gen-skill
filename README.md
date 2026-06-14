# Anima Prompt Generator Skill

A [Codex](https://codex.ai) skill for generating Anima text-to-image model prompts using the structured **Anima3 v3.0** template system.

## What It Does

Compiles user requirements (in Chinese or English) into production-ready English prompts for Anima models. Follows a rigorous slot-based assembly workflow with built-in self-check validation.

## Structure

```
anima-prompt-gen-skill/
├── SKILL.md                          # Core workflow: slot order, output protocol, self-check
├── agents/
│   └── openai.yaml                   # Codex UI metadata
└── references/
    └── anima3-template.md            # Full v3.0 template with tag libraries
```

## Workflow

```
User request → Match scene type (Decision Tree) → Fill slots in order
→ Self-check (6-point validation) → Conflict check → Single-line output
```

### Slot Order

```
[count/gender] → [character/series] → [appearance] → [clothing/state]
→ [pose/action/sex] → [expression/reaction] → [camera/shot]
→ [scene/environment] → [detail/mood]
→ [natural language supplement]
```

### Key Rules

- **Output**: 1 line, all lowercase, comma-separated, no markdown, no quality/artist/lighting tags
- **Tag count**: Solo 16-30 / Duo 22-38 / Complex 30-48
- **Solo default**: `direct eye contact, facing viewer` unless back/profile specified
- **Multi-person**: Must supplement appearance for each character; natural language at end for relationships
- **Cross-slot consistency**: Ancient ↔ ancient, cyber ↔ cyber, daily ↔ daily — no world-breaking mixes

## Tag Libraries (in `references/anima3-template.md`)

| Section | Content |
|---|---|
| §5 | 7 scene-type decision trees |
| §6-7 | Count, identity, appearance tags |
| §8 | Clothing modification engine |
| §9 | Pose, action, sex positions |
| §10 | Expression & reaction mapping |
| §11-13 | Camera, scene, detail & mood |
| §14 | 12 special theme recipes |

## Usage in Codex

```markdown
Use $anima-prompt-gen to generate Anima model prompts following the
structured v3.0 template. Reference the tag libraries in /references
and follow the slot-based assembly workflow.
```

## License

MIT
