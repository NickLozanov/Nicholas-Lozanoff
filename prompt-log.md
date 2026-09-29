# Prompt Log

A running record of AI sessions that mattered for this repo.

## 2026-09-08
- Asked an AI to generate the portfolio repo folder skeleton (capabilities/, docs/briefs/, docs/decisions/, data/, analysis/figures/, .claude/skills/) based on the course spec.
- Verified: no folder named after the course/term, every folder has a file, no generic AI filler left in place of real content.

## 2026-09-18
- Asked an AI to apply professor feedback from the first review stage: fix a `.gitignore` that matched nothing (patterns were on one line instead of five), move the bio out of `BIO.md` into `README.md` and start an engagement index there, reformat `RESUME.md` with Markdown headings/bullets and drop a stray `<img>` tag, add one-line `README.md` files to `analysis/`, `capabilities/`, and `docs/`, and replace the placeholder text in `capabilities/marginal-analysis/README.md`.
- Declined the AI's default recommendation to redact the Remote Pilot, OSHA-30, and ESCP coordinator ID numbers from `RESUME.md`; kept all credential IDs as-is.
- Did not write `docs/briefs/` or fill in `capabilities/marginal-analysis/spec.md` — per the professor's note, the brief and the analysis reasoning are mine to write first, before the model, not the AI's.
- Verified: `.gitignore` now matches the five intended patterns, no fabricated analysis content was added anywhere spec/model work is still outstanding.

## 2026-09-27
- Asked an AI (Claude Code) to format `docs/briefs/perfect-competition-brief.md` to match the resume styling: headings, a bed-cap table, bulleted hypothesis points, and the supporting figure centered. Wording of the brief was not changed.
- Second request in the same session: removed the bed-cap table and the supporting figure from the brief.
- Verified: the problem statement and hypothesis text were my own words, unchanged by either edit.

## 2026-09-29 — Stage 1 critique of the brief
Run in a Claude Code session after the first review of the brief (grade 80, B-).

**Prompt:** "Critique my Stage 1 brief (problem statement and hypothesis) against the case data below. Check whether the problem statement contains every constraint the model will need, whether the hypothesis is falsifiable, whether the mechanism behind my predicted mix matches how the case actually defines labor, and whether the predicted mix is feasible. Do not solve for the optimal mix." — followed by the brief and the case data table (crop parameters, 36-week season, $20,000 fixed costs, 64 beds, 720 own hours, up to 4 temps at 1,440 hrs each, Labor(q) = q × hrs/wk/bed × 36 × (1 + rate)^q).

**What came back, and what I did about each point:**

| # | Critique point | What I did |
|---|---|---|
| 1 | No falsification criterion — the brief never says what model result would prove the hypothesis wrong. | Pending: I will add a "How I would know I was wrong" section with a tomato-bed threshold N that I choose. |
| 2 | The mechanism is on the wrong variable. The brief argues in calendar time (tomatoes early, mesclun "damping" tomatoes late in the season), but the labor formula compounds on q, the number of beds of that crop. The 10th tomato bed costs more labor than the 9th all season long; nothing gets worse in October. Tomato marginal labor: bed 1 ≈ 99 hrs, bed 10 ≈ 424 hrs, bed 15 ≈ 854 hrs, bed 20 ≈ 1,651 hrs. Each crop compounds on its own bed count, so mesclun cannot damp tomatoes. | Pending: I will rewrite the mechanism in bed count. |
| 3 | The predicted mix (15 T / 30 M / 19 C) breaks the labor ceiling. It needs ≈ 8,510 hrs (tomatoes 5,639; mesclun 1,960; carrots 911) against a ceiling of 6,480 hrs (720 + 4 × 1,440). | Pending: I need to decide whether to keep these numbers or revise them. |
| 4 | The objective is vague — "several accounting terms such as profit margin and total revenue." Revenue and margin can point to different mixes; the model needs one objective. | Addressed: the problem statement now says the objective is profit for the season. |
| 5 | The problem statement leaves out constraints the model needs: the $20,000 fixed cost, the labor formula, own hours, temp workers, and the labor-hour ceiling. "Roughly" hedges the prediction. | Addressed: added the crop table, the fixed cost, the labor formula, and the 6,480-hr ceiling; dropped "roughly." |
| 6 | The fixed $20,000 does not change which mix is best (it is paid whatever I plant); it only changes whether the season's profit is positive. | Noted; no change to the brief. |
| 7 | The brief is written from outside ("It is acknowledged that...", "an optimization exercise must be completed"). | Addressed in the problem statement (rewritten in first person). The hypothesis will be rewritten with point 2. |
| 8 | "Carrots … consistent revenue throughout the season" — the case gives revenue per bed per season, with no week-by-week timing, so there is nothing for a model to measure this against. | Pending, with the mechanism rewrite. |

- The AI wrote the new problem-statement text from my original wording plus the case data. I reviewed it before committing.
- Not done by AI: the hypothesis numbers, the mechanism, and the threshold N. Those are mine to write.
