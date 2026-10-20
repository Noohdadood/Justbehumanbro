---
name: "human-nonfiction-voice"
description: "Use when drafting or revising nonfiction articles, essays, or blog posts so they avoid the AI tells expert readers notice, based on Russell, Karpinska and Iyyer (ACL 2025)."
---

# Human Nonfiction Voice

This skill is based on "People who frequently use ChatGPT for writing tasks are accurate and robust detectors of AI-generated text" by Jenna Russell, Marzena Karpinska and Mohit Iyyer (ACL 2025, arXiv:2501.15654).

In that study, five expert readers judged 300 nonfiction articles, each under 1,000 words. Every article was paired with an AI version written from the same title and subtitle by GPT-4o, Claude 3.5 Sonnet or o1-Pro, sometimes paraphrased or "humanized." The experts' majority vote misclassified only 1 of the 300. Non-experts performed near chance.

This skill covers article-level choices. Run `ai-prose-line-edit` afterward for sentence-level fixes.

## What the experts cited

The percentages are the share of expert explanations that mentioned each clue.

- **Vocabulary (53.1%):** stock words such as delve, crucial, testament, vibrant and showcase. Experts who relied on vocabulary alone made the most errors.
- **Sentence structure (35.9%):** "not only … but also", "it's not just X, it's Y", reflexive groups of three, and uniformly complex sentences. Human writing mixes in short, choppy sentences.
- **Grammar and punctuation (24.8%):** text that is too clean and too formal, with few dashes, brackets or ellipses.
- **Originality (23.7%):** a safe, obvious angle with no surprise or humor. Humans write things like "catnip for a certain type of Reddit user."
- **Quotes (22.3%):** formal quotes that sound like the surrounding prose and land at the end of paragraphs. Real quotes are short, conversational fragments woven into sentences.
- **Clarity (19.5%):** over-explaining, irrelevant detail, telling instead of showing.

The experts' guide also covered these points:

- **Introductions:** scene-setting about a time or place.
- **Conclusions:** a neat, upbeat summary, often with a values list such as "inclusivity, equity, representation."
- **Formatting:** lists with bolded headers and colons.
- **Tone and content:** flowery, positive tone, and avoidance of controversy or mild profanity.
- **Attribution verbs:** humans repeat "says", while AI varies it with "notes" and "explains".
- **Names:** placeholder names such as Emily or Sarah (63% of GPT-4o articles and 70% of Claude articles), and stacks of "Dr." and "Prof." titles.
- **Specifics:** human nonfiction contains many verifiable specifics, such as dates, figures, places and named sources.

## Workflow

1. **Angle.** Before drafting, state the non-obvious claim or detail the piece hinges on. If there isn't one, ask the user for it rather than inventing one.
2. **Draft.** Open on a fact, person or tension, not scene-setting. End on the strongest specific point, not a recap or a moral.
3. **Audit.** Output a table with one row per clue above. For each row, quote the draft where the clue appears, or write "absent". Do not merge or skip rows.
4. **Revise** each flagged item:
   - weave quotes into sentences
   - use "says" for attribution
   - replace placeholder names
   - cut the summary conclusion
5. **Specifics check.** List every factual claim with its source. Mark any claim without a source [NEEDS SOURCE].

## Hard rules

- Never invent quotes, people, statistics or sources to seem human. Use only what the user supplied or what you verified. Otherwise leave a placeholder such as [QUOTE NEEDED].
- Do not add deliberate errors, slang or contractions as noise.
- When revising the author's draft, keep their voice and report each change.
- Do not use this skill to submit AI-written work as fully human-written where disclosure is required, such as coursework, journalism standards or publication policies. In the study, the commercial detector Pangram still caught 99.3% of "humanized" articles. Aim for good writing, not evasion.

## Limits

The evidence comes from American English articles under 1,000 words drawn from eight outlets, including AP, the New York Times, the Wall Street Journal and National Geographic, judged by five expert annotators. Clues such as names and vocabulary are specific to particular models and change as models update.