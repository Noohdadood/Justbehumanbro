---
name: human-narrative-craft
description: Use when drafting or revising short fiction that should read as human-authored, by auditing and revising AI-leaning narrative defaults identified in the StoryScope study.
---

# Human Narrative Craft

Based on StoryScope by Jenna Russell, Rishanth Rajendhran, Chau Minh Pham, Mohit Iyyer (University of Maryland) and John Wieting (Google DeepMind), COLM 2026, arXiv:2604.03136. The study compared 10,272 human short stories with 51,336 stories that five LLMs (Claude Sonnet 4.6, GPT-5.4, Gemini 3 Flash, DeepSeek V3.2, Kimi K2.5) wrote from the same prompts. Each story was scored on 304 discourse-level features.

## Core principle

Narrative choices carry the signal. With every style feature removed, narrative features alone still separated human from AI stories at 93.2% macro-F1. Editing AI stories to remove clichés and purple prose barely changed that (95.5% before, 93.9% after). AI stories cluster tightly together in narrative space. Human stories are rarer: in 57.8% of prompts, the human version was the most unusual of the six. Revise the choices, not just the wording.

The defaults travel together. No single narrative dimension is sufficient on its own, and none is necessary: characters alone reach 80.2%, and dropping any one dimension costs at most 1.2 points. Fixing one default won't change the overall profile, so revise characters, plot, time, and references together. Start with characters, the strongest single lever.

The figures below are population averages, not rules for any one story.

## Diagnostic: AI-leaning defaults

| Default | AI | Human | Revise toward |
|---|---|---|---|
| Narrator comments on the theme | 77% | 52% | Let images and actions carry the meaning. Cut the closing lesson. |
| Thematic explicitness (1–5) | 3.94 | 3.28 | Leave part of the moral unresolved. |
| Dialogue as philosophical debate | 59% | 34% | Concrete talk: errands, grievances, deflection, jokes. |
| No subplots | 79% | 57% | A thematically parallel subplot (human 42%, AI 21%). |
| Resolution driven by protagonist's choice | 69% | 46% | Let chance, other people, or outside forces shape the ending. |
| Resolved by internal understanding | 47% | 27% | Resolve through external action, or leave it unresolved. |
| Emotion shown through the body | 81% | 38% | Sometimes name the feeling directly (human 29% explicit labels, AI 8%). |
| Olfactory imagery | 82% | 57% | Use the sense this particular character would notice. |
| Implicit, vague allusion | 72% | 50% | Name real books, songs, places, brands (human 47%, AI 24%). |
| Character introduced by external description | 52% | 30% | Introduce characters in dialogue or in action. |
| Morally ambivalent protagonist | 38% | 59% | Give the protagonist choices the story doesn't excuse. |

Other AI-leaning defaults, as mean 1–5 scores (AI vs human):

- Setting mirrors a character's mood (4.07 vs 3.58).
- Heavy sensory density, and deep access to characters' inner thoughts.
- Prominent nature or ecology.
- A single unbroken causal chain (4.20 vs 3.92).
- An opening that grounds the setting heavily and in fine-grained spatial detail.
- A long build of character investment before the main threat arrives (2.99 vs 2.76).

Human-leaning choices:

- Chronological discontinuity, flashbacks, and flash-forwards.
- Time jumps that delay a revelation.
- Surprises that force a rereading of earlier scenes.
- More dialogue relative to narration, and more distinct locations.
- Fourth-wall breaks and direct reader address (ordinal means 0.67 vs 0.39 and 0.28 vs 0.07).

Human fingerprints include characters introduced in dialogue, a single focal character, back-loaded revelation, crossover-genre ambition, and visible withholding (the reader can tell something is being kept back).

## Per-source fingerprints (self-check)

- **Claude:** flattest event escalation of any source, low variety of event types, uniform voice, epilogue or flash-forward endings, no dreams or visions, uncanny or haunted moods, and reverent, continuist handling of convention (62% vs 39–56%). It also tends to overshoot requested length.
- **GPT:** gossip or rumor drives the plot (64% vs 44–55%), a retrospective view from decades later, no habitual ("we used to…") narration, and partial or ambiguous reconciliations. It subverts reader expectations more often than other AIs (41% vs 27–36%).
- **Gemini:** tidiest endings, extended denouements, bleak settings (88%), siege or ordeal plots, and mostly direct speech.
- **DeepSeek:** front-loads crucial context, has a visible narrator, shows emotion through behavioral cues, and interleaves backstory evenly.
- **Kimi:** in medias res openings, characters introduced in action, no explicit trait labels, and few distinctive traits overall.

If you are one of these models, check the draft against your own fingerprint first.

## Workflow

1. **Draft** from the premise without the checklist, so the story has its own logic.
2. **Outline.** Summarize the draft along the paper's dimensions before judging it: agents (with emotion and motivation trajectories), social network, events and causality, plot (themes, stated moral, arc), setting, time (order, jumps), revelation (what is withheld, when surprises land), and perspective. Use the outline to judge structure rather than prose.
3. **Audit.** For each default, quote the line or scene where it appears, or write "absent". Never flag a default without evidence.
4. **Revise.** Replace each flagged default with a specific alternative that fits these characters and this premise. Make structural changes where needed, such as reordering scenes, adding a subplot, or rewriting the ending. Cover several dimensions, starting with characters.
5. **Check for overcorrection.** Remove any change that only adds variety: a gratuitous flashback, a decorative subplot, unearned ambiguity, or a name-drop the character wouldn't make. Every choice must serve the story.
6. **Reread for specificity.** Look for at least one fitting real-world reference, one line only this character would say, and one turn the reader won't predict.

## Constraints

- When revising someone's draft, keep their voice, plot, and point of view. List each structural change and the default it addresses.
- Do not add mechanical "human" noise such as typos, filler, or fake hesitation. Human texture comes from choices.
- Do not pad. Length follows the story, not a target.
- Do not use this skill to pass off AI-generated fiction as fully human-written where disclosure is expected (coursework, contests, publishers, or platforms with AI-use rules). The paper was motivated by readers being misled about authorship. Use the skill for craft, and disclose AI assistance where it is required.

## Limits

- Surface style is still the stronger signal: text classifiers detected AI stories at 99.7% or higher. This skill covers narrative choices only.
- The evidence comes from stories of about 5,000 words by five specific models. Features were defined by GPT-5.1 and annotated by Gemini 3 Flash (κ = 0.84 vs human annotators; human–human κ = 0.74).
- Results held across length-matched subsets and genres, but may not transfer to poetry, flash fiction, nonfiction, or newer models.
