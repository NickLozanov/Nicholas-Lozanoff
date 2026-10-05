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

## 2026-09-29 — Brief replaced with my own rewrite; second Stage 1 critique
- I replaced the problem statement and hypothesis with my own rewrite. The AI's problem-statement draft from the earlier entry (data table, labor formula, first-person wording) was removed completely. The AI pasted my text into the brief word for word; the only change was Markdown headings and bullets.
- The "Addressed" rows in the first critique above (points 4, 5, 7) referred to the AI's draft and no longer apply to the brief as it stands.
- Between the two critiques I asked the AI which mixes fit under the labor ceiling and for options on bed-count reasoning. It gave feasible mixes and three sample explanations. I did not use its sample text. My chosen mix (14 / 30 / 20) and reasoning are my own.

**Prompt:** "Critique my Stage 1 brief (problem statement and hypothesis) against the case data. Check whether the problem statement contains every constraint the model will need, whether the hypothesis is falsifiable, whether the mechanism matches how the case defines labor (Labor(q) = q × hrs/wk/bed × 36 × (1 + rate)^q), and whether the predicted mix is feasible. Do not solve for the optimal mix." — followed by the brief and the case data.

**What came back, and what I did about each point:**

| # | Critique point | What I did |
|---|---|---|
| 1 | **The predicted mix breaks the labor ceiling.** 14 T / 30 M / 20 C needs ≈ 7,727 hrs (tomatoes 4,785; mesclun 1,960; carrots 983) against 6,480 hrs available (720 + 4 × 1,440). Because carrots cap at 20 and mesclun at 30, any mix that uses all 64 beds needs at least 14 tomato beds, and every such mix is over the ceiling. So the "all 100% of beds are used" assumption can't hold; labor, not beds, is the binding limit. | Pending — my decision. |
| 2 | **Hours are written as dollars.** "$6,480 ($720 staff workers + $5,760 of temporary workers)" are labor *hours*. In dollars, temps cost 5,760 × $17.36 ≈ $99,994, and my own 720 hrs are valued at ≈ $25,000 (implied $34.72/hr). | Pending — my decision. |
| 3 | **The $20,000 fixed cost isn't in the problem statement.** It appears only indirectly ("fixed seasonal costs") in the last paragraph. It doesn't change which mix is best (it is paid whatever is planted), only whether the season is profitable. | Addressed: added "fixed costs of $20,000" to the problem statement. |
| 4 | **Two objectives.** "Greatest financial returns" and "maximize the number of productive beds" can give different answers; a bed can be productive (revenue ≥ its costs) and still add less profit than planting nothing there would save in labor. The model needs one objective — profit for the season. | Pending — my decision. |
| 5 | **The "productive bed" definition is the right idea, in the right variable.** Defining a tipping point where the next bed's labor makes it cost more than it earns puts the diminishing returns on bed count, as the review asked. The mesclun and carrot bullets now use the labor equation and incremental cost per added bed. | No change needed. |
| 6 | **The tomato bullet doesn't apply that definition.** Tomatoes get "the remaining 14 beds" — a leftover, not a marginal argument. It never says at which tomato bed the next one stops being productive. By my own definition, the check is each tomato bed's extra labor against its $7,920 revenue after fertilizer; at $17.36/hr, the 14th tomato bed adds ≈ 746 hrs (≈ $12,950). | Pending — my decision. |
| 7 | **"Roughly" is back.** The review asked for it to be dropped; it softens the three numbers the brief commits to. | Addressed: removed "roughly" from the hypothesis. |
| 8 | **The falsification test is present but can't pass.** "Different than 14 tomato beds" is a clear threshold tied to a model output, which the review asked for. But since 14/30/20 is over the labor ceiling, the Solver cannot return it, so the test is failed before the model runs. A one-sided threshold ("fewer than N" or "more than N") would test the reasoning rather than the exact number. | Pending — my decision. |
| 9 | **Voice is still from outside.** "It is acknowledged that…", "an optimization exercise must be completed", "we need to". The review asked for it to be written as the one deciding. | Pending — my decision. |
| 10 | **Typos.** "can company health" (a missing verb — "can harm"?), "a conceptual a predictive analysis", "Carrots. secondly" (lowercase), "Carrots beds". | Pending — my decision. |

## 2026-09-29 — Final Stage 1 critique before submission
- Edits since the second critique, all my own wording, pasted in by the AI: added "fixed costs of $20,000 (fix cost total of the firm)" to the problem statement, removed "roughly," and rewrote the final hypothesis paragraph, bolding the falsification sentence.

**Prompt:** same as the second critique, run on the brief as submitted.

**What came back, and what I did about each point:**

| # | Critique point | What I did |
|---|---|---|
| 1 | **The predicted mix still breaks the labor ceiling.** 14 T / 30 M / 20 C needs ≈ 7,727 hrs against 6,480 available. Any mix that fills all 64 beds needs at least 14 tomato beds and is over the ceiling, so the "100% of beds are used" assumption can't hold. | Left as is. This is my prediction going in; where the model disagrees is for Stage 1.3. |
| 2 | **The falsification test is clear but already failed.** The bolded sentence ties the hypothesis to a model output, as the review asked. Because 14/30/20 is infeasible, the Solver cannot return 14 tomato beds. | Left as is. |
| 3 | **Fixed costs can't move the tomato count.** The final paragraph says fixed seasonal costs might force fewer tomato beds. The $20,000 is paid whatever is planted, so it changes whether the season is profitable, not which bed is worth adding. Only labor, fertilizer and revenue act at the margin. | Left as is. |
| 4 | **Hours are written as dollars.** "$6,480 ($720 staff + $5,760 temporary)" are labor hours; in dollars the temps alone cost ≈ $99,994. | Left as is. |
| 5 | **Two objectives.** "Greatest financial returns" and "maximize the number of productive beds" can disagree; the model will maximize season profit. | Left as is. |
| 6 | **The tomato bullet still doesn't apply the "productive bed" test.** Tomatoes get "the remaining 14 beds" rather than a stopping point where the next tomato bed's added labor exceeds its $7,920 revenue after fertilizer. | Left as is. |
| 7 | **Improved since the first review:** "roughly" is gone, the $20,000 fixed cost is in the problem statement, mesclun and carrots are argued bed by bed through the labor equation, and the final paragraph reads clearly. | No change needed. |
| 8 | **Voice and typos.** Still partly written from outside ("It is acknowledged…", "we need to"). Typos: "fix cost," "can company health" (missing verb), "Carrots. secondly," "Carrots beds," "14 tomatoes beds." | Left as is. |

- Submitted the brief for review as it stands.

## 2026-09-30 — Revised hypothesis (12 / 30 / 20)
- After the final critique showed 14 / 30 / 20 was over the 6,480-hr labor ceiling, I asked the AI how many tomato beds fit under that limit. It calculated the tomato labor for each bed count (12 beds = 3,390 hrs; 13 beds = 4,039 hrs) and the most tomato beds that fit alongside different mesclun/carrot counts (12 with 30 mesclun and 20 carrots).
- Using those numbers, I rewrote the hypothesis myself: 12 tomatoes, 30 mesclun, 20 carrots, with 2 beds left unused. I also corrected the labor ceiling from dollars to hours and added a "How I will Know I was wrong" section with a band of 12 ± 2 tomato beds.
- The AI pasted my text into the brief word for word. The only formatting was Markdown headings, bullets, and bolding the falsification sentence.

## 2026-10-05 — Built the marginal-analysis workbook from my spec
**Prompt:** "Read my spec at capabilities/marginal-analysis/spec.md. Build the workbook it specifies exactly: every input a named range with the stated unit, every calculated cell a formula (no pasted values), the validation rules computed in the workbook, and the published check figures treated as acceptance criteria. Save it as capabilities/marginal-analysis/model.xlsx. If anything in the spec is ambiguous, ask me questions before you build."

**Questions the AI asked before building, and my answers:**

| # | Ambiguity in the spec | My decision |
|---|---|---|
| 1 | The labor formula is written as (1 − DIM_RTRN)^q, but the text says each added bed should *increase* labor, which needs (1 + DIM_RTRN)^q. (1 + ) gives 10 / 20 / 30; (1 − ) gives 20 / 20 / 24. | Use (1 + DIM_RTRN)^q. |
| 2 | The check-sheet section says the optimum is 10 T / 20 C / 30 M; "Expected Outputs" says 12 / 20 / 30, which earns $40,298 against $42,775 for 10 / 20 / 30. | 10 / 20 / 30 is the acceptance criterion. 12 / 20 / 30 is shown on the Checks sheet for reconciliation only. |
| 3 | The spec formulas give $42,775.16 at 10 / 20 / 30, not the published $42,762 (+$13.16). Carrot hours of exactly 5/6 instead of 0.833 give $42,768.33, which still misses. | Pass the profit check within ± $25. |
| 4 | The published standalone P = MC points (~10 / ~10 / ~6) only come out if tomatoes use the temporary rate ($17.36/hr) and carrots and mesclun the permanent rate ($34.72/hr). | Show both rates; each check passes if either rate is within ± 1 bed. |

**Changes the AI made without asking (listed on the workbook's README sheet):**
- Bed-cap constraints are written "≥ cap" in the spec; built as "≤ cap".
- Excel names cannot contain % or spaces: `*_DIM_RTRN_%` → `*_DIM_RTRN_PCT`, `TEMP_HOURS_LABOR_PER WORKER` → `TEMP_HOURS_LABOR_PER_WORKER`. Typos `TOTAL_RETILIZER` and `SEASON_PROFIT` read as `TOTAL_FERTILIZER` and `SEASONAL_PROFIT`.
- No new model inputs were added. The only added constants are the two check tolerances on the Checks sheet.

**What came back:**
- `model.xlsx` with Summary, Inputs, Cost Structure, Marginal Cost (with price-vs-MC charts), Optimization, Enumeration and Checks sheets. Every calculation is a formula on named ranges; the Enumeration sheet evaluates every integer mix as a check that does not depend on Solver.
- Result: 10 tomato / 20 carrot / 30 mesclun beds, seasonal profit $42,775.16. Carrot and mesclun bed caps are binding; tomatoes stop on marginal cost (an 11th bed adds ≈ 490 labor hours and lowers profit by ≈ $590). 4 beds and ≈ 1,203 labor hours are unused.
- The AI recalculated the workbook in LibreOffice: 45 checks pass, 0 fail, 1 info row (the 12 / 20 / 30 reconciliation). It also entered a deliberately wrong mix to confirm the checks fail.

**Not yet done (me):**
- Solver was not run; the AI could not run Excel. The decision cells hold 10 / 20 / 30 from the Enumeration sheet. I still need to run Solver (GRG Nonlinear, integer) in Excel and confirm it returns the same mix.
- Review the workbook myself before relying on it, and update `capabilities/marginal-analysis/README.md`, which still calls the spec and model placeholders.
