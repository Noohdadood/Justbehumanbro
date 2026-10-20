---
name: "ai-prose-line-edit"
description: "Use when line-editing any draft (fiction, essays, articles, emails) to remove sentence- and word-level AI-writing artifacts, based on LAMP, Reinhart et al., and Rallapalli et al."
---

# AI Prose Line Edit

A sentence- and word-level editing pass for any genre. For fiction, run `human-narrative-craft` first, because structure matters more than wording. For articles and essays, run `human-nonfiction-voice` first.

## Sources

- **LAMP:** Chakrabarty, Laban and Wu, "Can AI writing be salvaged?" (arXiv:2409.14509). Professional writers made 8,035 edits to 1,057 paragraphs written by GPT-4o, Claude 3.5 Sonnet and Llama 3.1 70B, across fiction, travel, food, personal essays and advice.
- **Reinhart et al.:** "Do LLMs write like humans?" (Carnegie Mellon and NJIT, arXiv:2410.16107). Compared 66 grammatical features across about 8,000 parallel human and LLM continuations in six registers.
- **Rallapalli et al.:** "Interpretable Stylistic Variation in Human and LLM Writing" (Carnegie Mellon SEI, arXiv:2604.14111, preprint). Covered 11 models, 8 genres and 4 decoding strategies, using 67 Biber features.

## Principle

AI prose is uniform. It leans on the same few words and grammatical constructions, and it under-uses rare ones that humans reach for. Swapping individual words does not fix this, because the grammatical tilt remains. Professional writers fixed AI text mostly by replacing material (74% of edits) or deleting it (18%), and rarely by inserting new material (8%).

## Pass 1: LAMP's seven categories, most common first

1. **Awkward word choice and phrasing (28%):** imprecise words, unclear pronouns, needless passive voice, "seemed to." Fix: name the exact thing or action.
2. **Poor sentence structure (20%):** run-ons, convoluted multi-clause sentences, stapled transitions. Fix: split long sentences and vary length, putting short sentences next to long ones.
3. **Unnecessary or redundant exposition (18%):** restating the obvious, explaining what was already shown. Fix: delete it and trust the reader.
4. **Cliché (17%):** stock images and phrases such as "a beacon of hope." Fix: write the specific observation the cliché stands in for.
5. **Purple prose:** ornate language that draws attention to itself. Fix: say it plainly.
6. **Lack of specificity:** generalizations with no picture. Fix: add a concrete detail, number, name or interior thought. This is the one category where edits lengthen the text.
7. **Tense inconsistency.** Fix: make tense consistent.

Also watch for repeated part-of-speech templates. LAMP found that 15 of the 50 most common templates in LLM text rarely appear in human text, for example "a [noun] of [noun] and [noun]."

## Pass 2: grammar density (Reinhart; Rallapalli)

Count these per paragraph. Do not ban them; aim for human-like rates.

Overused by instruction-tuned models (ratios are GPT-4o vs human, from Reinhart):

- Present participial clauses ("..., revealing a deeper truth"): 5.3×, and 2–5× across instruction-tuned models.
- "That" clauses as subject ("That she left was clear"): 2.6×.
- Nominalizations ("the implementation of" instead of "implementing"): 2.1×.
- Phrasal coordination ("X and Y" pairs such as "resilience and determination"): 1.9×.
- Postnominal past-participle clauses ("the data collected by") and sentence relatives (", which made everything harder"), from Rallapalli.

Underused by LLMs. Humans already use these rarely, about 0.2–1.2 per 1,000 words, and LLMs suppress them further (Rallapalli):

- Concessive "though"
- Synthetic negation ("no one came", "neither") instead of "did not"
- Wh-object clauses ("I know what you did")
- Pied-piping ("the town in which")
- Discourse particles ("well", "anyway", "now") in voiced writing
- Agentless passive ("the bill was passed"): GPT-4o uses it at about 0.5× the human rate

Use these where they fit naturally. Never force them.

## Pass 3: flagged vocabulary

These lists are model-specific and date-stamped. Treat them as flags, not bans. Humans use these words too, and annotators who over-relied on vocabulary wrongly flagged human text.

- **GPT-4o**, as a rate relative to human (Reinhart, 2024 models): camaraderie 162, tapestry 155, intricate 119, underscore 107, unspoken 102, amidst 100, palpable 95, solace 95, fleeting 84, unravel 83. "Tapestry" appeared in 23% of GPT-4o texts and "amidst" in 27%.
- **GPT-4o mini:** grapple 131, ignite 122, vibrant 92, cacophony 89.
- **Llama 3 Instruct:** unease, palpable, pang, waft, rut.
- **Also flagged by Russell et al. (ACL 2025):** delve, crucial, testament, showcase, vibrant, significantly, "not only … but also", "it's not just X, it's Y", and reflexive groups of three.

If a flagged word is the exact right word, keep it. If it is decoration, replace the whole phrase, not just the word.

## Workflow

1. **Pass 1.** Output a table with one row per sentence that has an issue: the quoted sentence, its category (1–7), and the fix. Do not summarize or group rows.
2. **Pass 2.** Report per-paragraph counts of the four most-overused constructions. Rewrite any paragraph with more than two present participial clauses or a stack of nominalizations.
3. **Pass 3.** List each flagged word or pattern with its sentence, and say whether you kept or replaced it and why.
4. **Reread aloud.** Check that sentence lengths vary and the voice matches the author's.

## Constraints

- Preserve meaning. LAMP judged about 70% of its writers' non-deletion edits to be meaning-preserving.
- Do not insert typos, slang or contractions as "human noise." Rallapalli found LLMs already overuse contractions, so they are not a human signal.
- When editing someone's own draft, keep their voice and report every change.
- Do not use this skill to disguise AI authorship where disclosure is required.

## Limits

- The word lists come from 2024–2025 models and will drift as models change.
- The studies disagree on downtoners ("barely", "nearly"), so they are left out.
- Rallapalli is a preprint, and its ratios appear only in figures.
- Line edits do not change narrative or argument structure, so detectors that use structure will still flag it.