---
name: anima-prompt-gen
description: Generate Anima text-to-image model prompts using the structured v3.0 template system. Use when the user wants to create prompts for Anima model image generation, especially when they provide a Chinese scene description that needs to be compiled into an English prompt following the Anima3 slot-based tag assembly format. The skill provides the full template framework including slot order, decision trees, conflict tables, tag libraries for appearance/clothing/pose/expression/camera/scene/detail, special theme recipes, and a final self-check protocol.
---

# Anima Prompt Generator

Generates Anima model prompts by compiling user requirements into structured English prompts using the Anima3 v3.0 template system.

## Quick Workflow

```
User request → §5 Decision Tree (match scene type) → §4 Slot Order (assembly sequence)
→ §6-13 Tag Libraries (fill each slot) → §3 Self-Check (6-point validation)
→ §3.1 Conflict Check → Single-line output
```

## Output Protocol (Hard Rules)

| Rule | Detail |
|---|---|
| Lines | **1 line only**, no line breaks |
| Separator | `, ` (comma + space) between tags |
| Case | **All lowercase** (except `score_` tags which keep underscores) |
| Weights | **No** `(tag:1.2)` syntax — field order is implicit weight |
| Banned | quality words (`masterpiece, best quality, score_9`), artist names, **lighting/tone tags** (`sunlight, moonlight, rim light, warm lighting` — built into lora). Environmental weather is allowed (`rain, snow, fog, steam`) |
| Output form | Plain text, one line. No code fence, no markdown, no preamble |
| Natural language | For complex scenarios (multi-person attribution, complex composition, special poses, panel relationships): **use English natural-language short phrases at the very end of the prompt** (after all tags) |

## Slot Order (Assembly Sequence)

Tags must be filled in strict slot order. Earlier slots carry more weight.

```
[count/gender] → [character/series] → [appearance] → [clothing/state]
→ [pose/action/sex] → [expression/reaction] → [camera/shot]
→ [scene/environment] → [detail/mood]
→ [natural language: relationship/action/story supplement]
```

### Tag Count Control

| Complexity | Total tags | Notes |
|---|---|---|
| Simple (solo display/tease/exposure/masturbation) | 16-30 | Less dimensions |
| Standard (duo sex/foreplay) | 22-38 | Pose + expression + liquids expand |
| Complex (multi-person/special themes/story key visual) | 30-48 | Cross-slot expansion |

### Per-slot tag count guidance

| Slot | Min | Max | Notes |
|---|---|---|---|
| count/gender | 2 | 4 | Fixed format, required |
| character/series | 0 | 2 | IP characters only |
| appearance | 3 | 8 | Hair 2 + eyes 1 + body 1 + skin 1 + non-human/marks as needed |
| clothing/state | 2 | 10 | Base + material + 1-3 mod dimensions + hosiery/shoes — naturally high count |
| pose/action/sex | 2 | 8 | Core pose 2 + auxiliary actions + variant dimensions |
| expression/reaction | 1 | 4 | Main expression 1 + at most 3 body reactions/liquids |
| camera/shot | 1 | 5 | Shot type required, angle/POV as needed |
| scene/environment | 2 | 6 | Main location + environment elements + time/weather |

### Default Eye Contact Rule

**Solo**: Always inject `direct eye contact, facing viewer` unless the user explicitly requests `from behind, facing away, profile, from side`.

**Multi-person**: Not mandatory. Use `looking at another` or as specified.

### Natural Language Usage

Place natural language supplement **at the very end** of the prompt, after all tags. Use for:
- Multi-person action relationships (`one reaches toward the viewer while the other watches in silence`)
- Complex composition / spatial relationships (`girl sitting on boy's lap facing him`)
- Special pose combinations where tag stacking loses clarity (`girl pinning wolf boy down while riding him`)
- Panel / contrast relationships (`left panel: dressed, right panel: nude`)
- Viewer narrative relationship (`as if inviting the viewer to escape together, as if daring the viewer to come closer`)

### Multi-person Rules

**Critical**: For multi-person scenes, always supplement key appearance for each character. Structure: count → character A appearance phrase → character B appearance phrase → shared tags (pose, camera, scene) → relationship/action description (natural language at end).

Format: `2girls, raiden shogun with long purple hair and purple eyes, yae miko with long pink hair and fox ears, skirt lift, shrine, one playfully lifting the other's skirt with a mischievous smirk while the other looks shy and embarrassed`

### Cross-slot Style Consistency

Clothing, scene, and detail/mood must be logically consistent. Ancient style with ancient (hanfu + ancient shrine), cyber with cyber (latex bodysuit + cyberpunk city), daily with daily (school uniform + classroom). Do NOT mix hanfu with cyberpunk city.

## Self-Check (Before Output)

| # | Check | Pass criteria |
|---|---|---|
| 1 | Person count consistency | `count/gender` tag count matches actual character count |
| 2 | Conflict check | No view/identity/clothing/pose/detail conflicts per §3.1 |
| 3 | Duplicate tags | Same tag does not appear twice |
| 4 | Scene plausibility | Scene tags physically compatible with action tags |
| 5 | Lighting ban | No lighting/shadow/tone tags |
| 6 | Tag count | Within complexity range (16-30/22-38/30-48) |

## Reference Files

The full template with complete tag libraries is in `references/anima3-template.md`. Navigate by section:

| § | Section | Content |
|---|---|---|
| 0 | Quick Start | How to use the template |
| 1 | ROLE | AI behavior guidelines |
| 2 | OUTPUT PROTOCOL | Output format rules (summarized above) |
| 3 | SELF-CHECK | Validation protocol (summarized above) |
| 3.1 | CONFLICT TABLE | Mutually exclusive tag pairs |
| 4 | SLOT ORDER | Tag assembly rules (summarized above) |
| 5 | DECISION TREE | 7 scene-type decision paths |
| 6 | COUNT & IDENTITY | Tag library: count/gender, character/series |
| 7 | APPEARANCE | Tag library: hair/eyes/body/skin/marks |
| 8 | CLOTHING & STATE | Tag library: clothing types/materials/states/7-dimension modification/contrast |
| 9 | POSE & ACTION & SEX | Tag library: solo/duo foreplay/duo sex/multi/yuri |
| 10 | EXPRESSION & REACTION | Tag library: expression intensity L1-L4/body reactions/liquids/marks |
| 11 | CAMERA & SHOT | Tag library: shot types/angles/POV/composition/body focus |
| 12 | SCENE & ENVIRONMENT | Tag library: locations/weather/time/scene details |
| 13 | DETAIL & MOOD | Tag library: texture/motion/optical effects/mood |
| 14 | SPECIAL THEME | Cross-slot recipes: NTR/BDSM/RBQ/futa/异种/调教/胁迫/偷窥/事后/另类日常/大车小孩/隐奸 |
| 15 | EXAMPLES | Placeholder for verified full examples |

## Using the Reference

1. Read the user's scene description
2. Match scene type using §5 Decision Tree in the reference file
3. Determine slot filling strategy from the decision tree
4. Fill each slot using tag libraries (§6-13)
5. If the scene matches a special theme (§14), consult it first for cross-slot recipes
6. Run §3 self-check
7. Check §3.1 Conflict Table
8. Output single line of comma-separated lowercase tags
