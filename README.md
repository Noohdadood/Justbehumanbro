# Human Narrative Craft

A Claude skill for drafting and revising short fiction so that its narrative choices do not follow the defaults that a 2026 study found in AI-written stories. The skill has Claude draft from a premise, outline the draft along the study's narrative dimensions, and audit it against 11 named AI-leaning defaults and a list of human-leaning choices. Each flagged default needs a quoted line or scene as evidence, or it is not flagged. Revisions change structure (who drives the plot, what is withheld, how time is ordered, which characters appear in dialogue) rather than swapping words. The skill then checks for overcorrection, meaning changes that add variety without serving the story. It targets narrative choices only, not sentence-level style, because text classifiers still catch AI prose at 99.7% or higher.

![Animated workflow: draft, outline, audit, revise, check overcorrection, reread](assets/workflow.gif)

*The skill's six-step workflow.*

## Credit

This skill is based entirely on the research of the StoryScope authors. This repository is **not affiliated with, endorsed by, or maintained by** the paper's authors or their institutions.

- **Paper:** "StoryScope: Investigating Idiosyncrasies in AI Fiction," Jenna Russell, Rishanth Rajendhran, Chau Minh Pham, Mohit Iyyer (University of Maryland, College Park) and John Wieting (Google DeepMind). COLM 2026.
- **arXiv:** [2604.03136](https://arxiv.org/abs/2604.03136)
- **Code and data:** [github.com/jenna-russell/storyscope](https://github.com/jenna-russell/storyscope)

```bibtex
@misc{russell2026storyscope,
  title         = {StoryScope: Investigating Idiosyncrasies in AI Fiction},
  author        = {Russell, Jenna and Rajendhran, Rishanth and Pham, Chau Minh and Iyyer, Mohit and Wieting, John},
  year          = {2026},
  eprint        = {2604.03136},
  archivePrefix = {arXiv},
  url           = {https://arxiv.org/abs/2604.03136},
  note          = {COLM 2026}
}
```

## The paper in detail

**Research question.** Can AI-generated stories be told apart from human ones without relying on surface style, using discourse-level narrative choices such as character agency and chronological discontinuity? Can originality be measured as statistical rarity in that narrative feature space?

**Data.** The authors extracted 10,272 human-written short stories from Books3. They reverse-engineered a writing prompt from each story with Gemini 2.5 Flash, then generated mirrored stories from five LLMs: Gemini 3 Flash, Kimi K2.5, DeepSeek V3.2, Claude Sonnet 4.6, and GPT-5.4. That gives 51,336 AI stories and 61,608 stories in total, averaging 4,753 words. Models refused to generate 24 stories. Generation cost roughly $2,800 and feature extraction roughly $1,600.

**Pipeline (StoryScope).**
1. The authors adopted 10 of the 12 aspects of the NarraBench taxonomy: Agent, Social Network, Event, Plot, Structure, Setting, Time, Revelation, Perspective, and Style. They excluded Paratext and Motivation.
2. GPT-5.1 converted each story into a structured JSON template along those dimensions.
3. For each prompt, GPT-5.1 compared the six templates. A discovery pool of 600 stories over 100 prompts was used for this stage.
4. GPT-5.1 proposed features within each dimension. The process ran three times, producing 408 candidates. Embedding clustering at cosine threshold 0.85 merged 65 clusters and left 304 features (a 25.5% reduction). The features split into categorical (124), ordinal (59), scale (45), binary (44), and multi-select (32).
5. Gemini 3 Flash annotated all 304 features for every story. Its repeatability across five runs was Krippendorff's α = 0.90. On 240 story-feature items from 12 stories, the mean human-model Cohen's κ was 0.84, against a human-human κ of 0.74.

**Classifier.** The authors used XGBoost because it outperformed linear models and random forests on validation and integrates with SHAP. SHAP importance, assessed with bootstrap resampling (B=50, prompt-level), sorted features into 30 core features and 75 fingerprint features. Binary models used a 5:1 positive class weight to match the AI-to-human ratio.

**Main results: binary detection (human vs. AI), macro-F1 / AUPRC (Table 2).**

| Method | Size | F1 | AP |
|---|---|---|---|
| Narrative (ours) | 257 | 93.2 | .959 |
| Core only | 30 | 84.8 | .828 |
| Core + fingerprint | 101 | 91.1 | .934 |
| Narrative + Style | 304 | 96.0 | .982 |
| Style only | 39 | 85.8 | .867 |
| ModernBERT | — | 99.9 | 1.00 |
| Stylometric + XGB | 144 | 99.8 | .999 |
| TF-IDF + XGB | 5,000 | 99.7 | .999 |
| Binoculars (zero-shot) | — | 55.9 | .404 |
| LAMP-edited stories | — | 93.9 | .988 |

**Main results: six-way attribution (Table 3).**

| Method | Size | Macro-F1 | Accuracy |
|---|---|---|---|
| Narrative (ours) | 257 | 68.4 | 68.4 |
| Core only | 30 | 46.5 | 46.8 |
| Core + fingerprint | 101 | 63.4 | 63.6 |
| Narrative + Style | 304 | 77.3 | 77.3 |
| Style only | 39 | 60.4 | 60.5 |
| ModernBERT | — | 99.8 | 99.8 |
| Stylometric + XGB | 144 | 99.6 | 99.6 |
| TF-IDF + XGB | 5,000 | 99.5 | 99.5 |

Narrative features alone retain over 97% of the Narrative + Style performance on binary detection (93.2 vs. 96.0).

**Core findings (Table 16, human vs. AI).** Each row gives human value, AI value.
- Thematic explicitness (1–5 scale): 3.28 vs. 3.94. Narrator states the theme: 52% vs. 77%.
- Dialogue as philosophical debate: 34% vs. 59%. Implicit-echo references: 50% vs. 72%.
- Emotion conveyed through the body: 38% vs. 81%. Explicit emotion labels: 29% vs. 8%.
- Olfactory sensory modality: 57% vs. 82%. Setting as psychological mirror: 3.58 vs. 4.07.
- Causal chain continuity: 3.92 vs. 4.20. Resolution driven by protagonist choice: 46% vs. 69%.
- No subplots: 57% vs. 79%. Thematically parallel subplots: 42% vs. 21%.
- Resolution through internal understanding: 27% vs. 47%. Character introduced by external description: 30% vs. 52%.
- Explicit named references: 47% vs. 24%. Balanced mix of explicit and implicit references: 37% vs. 16%.
- Fourth-wall permeability (ordinal mean): 0.67 vs. 0.39. Direct reader address (ordinal mean): 0.28 vs. 0.07.
- Chronological discontinuity (1–5): 2.40 vs. 2.12. Anachrony intensity (1–5): 2.58 vs. 2.31.
- Morally ambivalent protagonist: 59% vs. 38%.

**Per-model fingerprints.** Fingerprint counts were Human 32, Claude 26, GPT 11, Gemini 11, DeepSeek 7, and Kimi 3. Per-class six-way F1 with style was Human 93.0, Claude 89.3, GPT 82.1, and DeepSeek, Gemini, and Kimi between 65.8 and 66.8. Without style, the narrative model gave Human 88.5, Claude 77.1, GPT 73.0, and the bottom three 55.2 to 59.6. Claude favors epilogues and avoids dreams. GPT uses gossip as a plot mechanism in 64% of stories, against 44% to 55% for the other sources.

**Clustering and rarity.** The five AI sources overlap in a shared region, and human stories sit apart (Figure 2). The six most-confused source pairs are all AI-AI. Mean human-AI centroid distance is 6.6, against 4.3 for AI-AI. Human stories have a mean rarity percentile of 0.71, against 0.49 for AI (Cohen's d = 0.83). Of all human stories, 24.7% fall in the top 10% rarest, against 7.1% of AI stories. In 57.8% of prompts the human story is the rarest of the six versions (16.7% by chance).

**Robustness checks.**
- **LAMP editing.** Span-level removal of AI artifacts (cliché, purple prose and similar) was applied to 278 Gemini stories, with Gemini as the rewriter. Macro-F1 went from 95.5 to 93.9.
- **Length matching.** On a 2,754-story subset (1,377 human, 1,377 AI) matched by length deciles, Narrative stayed at 93.2. Style Only moved from 85.8 to 86.8. The paper also reports Narrative at 91.6, 94.3, and 93.7 across length bands.
- **Topic.** Binary macro-F1 ranges from 90.0 (mystery/detective) to 96.2 (historical). The Kruskal-Wallis test found no significant topic differences (H = 4.69, p = 0.46).
- **Memorization.** The screen flagged 0.70% of generated-story comparisons as high risk. Dropping high-risk prompts moved Narrative from 93.18 to 93.23 binary and from 68.42 to 68.68 six-way.
- **Dimension ablations (Table 8).** Trained alone, Agents scored best at 80.2 binary macro-F1. Removing Agents cost 1.2 points, the largest single drop. No dimension was sufficient alone or necessary alone.

### Figures

Redrawn from the paper's published numbers (not copied from the PDF).

**Pipeline**

![StoryScope pipeline](assets/pipeline.png)

**Where AI and human fiction differ most** (Table 16)

![AI vs human feature prevalence](assets/ai-vs-human-features.png)

**Detection results** (Table 2): narrative features alone reach 93.2% macro-F1.

![Detection results](assets/detection-results.png)

**Authorship attribution** (Figure 3): mistakes cluster among the AI models.

![Confusion matrix](assets/confusion-matrix.png)


## How the paper was applied to the skill

| Paper finding | Where it appears in SKILL.md |
|---|---|
| Narrative features alone reach 93.2%; LAMP editing moves 95.5% to 93.9% | "Core principle": revise choices, not wording. "Constraints" and "Limits": the skill covers narrative choices only. |
| Dimensions are redundant; removing any one costs at most 1.2 points | "The defaults co-occur" paragraph. Workflow step 3 requires revising several dimensions together. |
| Agents is the best single dimension (80.2%) | "Start with characters" in the core principle. Workflow step 3 says to start with characters. |
| Template stage (templates per story, Figure 8 prompt) | Workflow step 2, the Outline. It summarizes the draft along the paper's dimensions (agents, social network, events, plot, setting, time, revelation, perspective) before judging prose. |
| Closed-form, quotable features (Table 14–15) | The diagnostic table lists each default as a checkable choice. Workflow step 2 requires a quoted line or "absent" for every default. |
| Population averages (e.g., 77% vs. 52%) | "The figures below are population averages" paragraph. Workflow step 4 (overcorrection check). |
| Per-model fingerprints (Table 17) | "Per-source fingerprints (self-check)" section, which asks a model to check its draft against its own profile. |
| Style is still the stronger signal (99.7%+ for text classifiers) | "Limits" and the first Constraint. The skill does not claim to make prose undetectable. |
| Motivation: readers misled about authorship (Shy Girl, Section 1) | The last Constraint: no use to pass off AI fiction as human-written where disclosure is required. |
| Rarity as a proxy for originality (Section 4.1, Appendix H) | Not operationalized. The skill has no rarity metric, and step 5 asks for specificity instead. |

**Design decisions.**
- **Narrative choices, not wording.** The 93.2% narrative-only result and the LAMP result (93.9% after edits) both show that rewording alone leaves the signal in place. The skill therefore asks for structural changes.
- **Revise several dimensions together.** The ablations show redundancy across dimensions, so fixing one default does not change the profile. The workflow requires covering several dimensions.
- **Start with characters.** Agents is the strongest single dimension, so the revision begins there.
- **Outline before judging.** This mirrors the paper's template stage, so structure is judged from a summary rather than from prose.
- **Quoted evidence.** The paper's features are closed-form questions with discrete answers. The skill mirrors this by requiring a quote or "absent" for each default.
- **Overcorrection check.** Because the figures are averages, a story can be pushed toward human-leaning averages without being better. Step 4 removes changes that only add variety.
- **Self-checks.** Per-model fingerprints let a model check its own habits.
- **Disclosure constraint.** The paper was motivated by readers being misled about authorship, so the skill is for craft and must not be used to hide AI assistance where disclosure is expected.

**Correction.** In the diagnostic and in the list of human-leaning choices, the fourth-wall and reader-address values (0.67 vs. 0.39 and 0.28 vs. 0.07) are ordinal means on the paper's integer codes. They are not percentages.

## Installation

Option 1: Claude Code. Copy the skill folder into your user skills directory:

```bash
cp -r skills/human-narrative-craft ~/.claude/skills/
```

Option 2: Claude.ai. Zip the `skills/human-narrative-craft` folder and upload it as a skill in Claude.ai settings.

## Usage examples

**Audit a draft.**
> Use the human-narrative-craft skill to audit this 4,000-word story. Quote the line or scene for each default, or write "absent." Then revise the characters first and list each structural change against the default it addresses.

**Draft from a premise.**
> Using the human-narrative-craft skill, draft a 3,000-word story about a ferry dispatcher who finds a sealed letter addressed to someone who died in 1987. Keep the draft's own logic, then run the outline and audit steps and report which defaults you changed.

## Limitations

- The study's evidence comes from stories averaging about 5,000 words, written by five specific models (Claude Sonnet 4.6, GPT-5.4, Gemini 3 Flash, DeepSeek V3.2, Kimi K2.5). Findings may not transfer to poetry, flash fiction, nonfiction, or newer models.
- The features were defined by GPT-5.1 and annotated by Gemini 3 Flash. The reported human-model agreement (κ = 0.84) comes from 12 stories.
- All figures are population averages. A given story can legitimately use any of the defaults.
- The paper's test-set sizes are reported inconsistently (8,262 and 8,301 stories). This README uses the paper's stated numbers without reconciling them.
- The skill does not change sentence-level style, which remains the stronger detection signal.

## Responsible use

Use this skill to improve the craft of fiction you wrote or are developing. Do not use it to make AI-generated fiction pass as fully human-written where disclosure is required, such as coursework, contests, publishers, or platforms with AI-use rules. The paper's own motivation is that readers are being misled about who wrote the books they buy. Disclose AI assistance where it is required.
