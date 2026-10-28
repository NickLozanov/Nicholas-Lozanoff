# Marginal Analysis

This folder holds the marginal-analysis capability: evaluating the incremental cost or benefit of one additional unit of scope, so a decision can be made about whether the next increment is worth taking on.

- `spec.md` — the capability's inputs, method, and expected output, defined before the workbook was built
- `model.xlsx` — the workbook implementing the spec

`model.xlsx` is built from `spec.md`. Open it in Excel and start with the **README** and **Summary** tabs; the **Checks** tab tests the model against the spec's published check figures. See `docs/briefs/` for the engagement brief and `docs/decisions/` for the recommendation.

Exercised in: Perfect competition — farm bed-mix optimization: [brief](../../docs/briefs/perfect-competition-brief.md) (Stage 1), spec, build and audit (Stage 2; Solver confirmed 10 tomato / 20 carrot / 30 mesclun beds), [analysis with figures](../../analysis/2026-10-28-perfect-competition-analysis.md) and [decision memo](../../docs/decisions/2026-10-28-perfect-competition-memo.md) (Stage 3: recommend 10 / 20 / 30, seasonal profit $42,775.16).
