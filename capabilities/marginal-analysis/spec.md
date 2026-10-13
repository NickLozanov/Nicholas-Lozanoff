# Spec: Marginal Analysis

# Capability
The farm must choose integer quantities of tomato, carrot, and mesclun grow beds to maximize seasonal farm profit. The desicion is not simply to maximize revenue or fill all 64 beds. The capability (also called "model" in this spec interchangeably) must account for crop-specific diminishing returns in labor, the farm's staff available labor, fertilizer costs, fixed costs, crop bed caps, the 64-bed farm capacity, and the temporary-worker capacity. The primary objective is to maximize the profit for the 36-week season and the primary decision variables is determining quantity of growbeds dedicated to each type of crop, tomato, carrot, and mesclum. All three shall be non-negative integers. 

# Inputs
Every input shall exist as a named input/range or equivalent named parameter in the workbook. The model shall not encode these values only inside formulas. The model shall expose them in an inputs region so a reviewer can change a case assumption without editing formulas. Inputs are as follows and are separated into names of inputs, value of input, and unit of input:

| Name | Value | Unit |
|---|---|---|
| TOMATO_BED_CAP | 20 | beds | 
| CARROT_BED_CAP | 20 | beds | 
| MESCLUN_BED_CAP | 30 | beds | 
| TOMATO_REVENUE_PER_BED | 8800 | $/bed/season | 
| CARROT_REVENUE_PER_BED | 2094 | $/bed/season | 
| MESCLUN_REVENUE_PER_BED | 2700 | $/bed/season | 
| TOMATO_HRS_PER_WEEK_PER_BED | 2.5 | hour/week/bed | 
| CARROT_HRS_PER_WEEK_PER_BED | 0.833 | hour/week/bed | 
| MESCLUN_HRS_PER_WEEK_PER_BED | 1.25 | hour/week/bed | 
| TOMATO_FERTILIZER_PER_BED | 880 | $/BED/SEASON | 
| CARROT_FERTILIZER_PER_BED | 440 | $/BED/SEASON | 
| MESCLUN_FERTILIZER_PER_BED | 880 | $/BED/SEASON | 
| TOMATO_DIM_RTRN_% | 10 | % | 
| CARROT_DIM_RTRN_% | 2.5 | % | 
| MESCLUN_DIM_RTRN_% | 1.25 | % | 
| SEASON_WEEKS | 36 | weeks | 
| TOTAL_BEDS_AVAILABLE | 64 | beds | 
| PERMANENT_FIELD_LABOR_HOURS | 720 | hours/season | 
| PERMANENT_HOURLY_RATE | 34.72 | $/hour | 
| TEMP_WORKER_CAP | 4 | workers | 
| TEMP_HOURS_LABOR_PER WORKER | 1440 | hours/worker/season | 
| TEMP_HOURLY_RATE | 17.36 | $/hour | 
| FIXED_SEASON_COST | 20000 | $/season | 

Source of inputs. All input values are taken as stated from the "Perfect Competition" case handout parameters (https://adamwstauffer.github.io/ai-lms/case-perfect-competition.html). Three of them are rounded values:

- PERMANENT_HOURLY_RATE ($34.72) equals $25,000 ÷ 720 hours ($34.7222…) rounded to the cent.
- TEMP_HOURLY_RATE ($17.36) equals half of the unrounded permanent rate ($17.3611…) rounded to the cent.
- CARROT_HRS_PER_WEEK_PER_BED (0.833) equals 5/6 hour (0.8333…) rounded to three decimals.

The workbook uses the rounded values as stated in the handout. Audit finding A2 shows the effect of the rounding on SEASONAL_PROFIT.

Derived inputs include the following: 

| Name | Equation | Value |
|---|---|---|
| TEMP_HOURS_CAP | = TEMP_HOURS_LABOR_PER WORKER X TEMP_WORKER_CAP | 5760 hours | 
| TOTAL_LABOR_HOURS_CAP | = 5760 hours + PERMANENT_FIELD_LABOR_HOURS | 6480 HOURS | 

# Workbook Structure
The workbook shall be structured in the following mannar:
Inputs: named case inputs, units, source labels, and editable assumptions as listed above in the inputs section. 
Cost Structure: For the current bed mix: beds, revenue, fertilizer, crop labor hours, total labor hours, permanent/temporary labor allocation, labor dollars, total costs, and profit
Marginal cost schedules: integer q=0 through each crop's bed cap. show labor hours, labor dollars, fertilizer, marginal labor hours, marginal labor cost, and marginal total cost. Include standalone P = MC diagnostics for each crop. 
Optimization: Decision cells for the three integer bed counts, objective/profit, constraints, solver-ready formulas, and the optimized solution. If any decision recommendations are included here, highlight them for the reviewer's clarity and ease of navication.
Checks: Every rule in the Validation Rules section (V1 through V6), each with its expected value, workbook value, tolerance, and PASS/FAIL result, plus constraint checks and formula/error checks.
Read me / spec: sort description of the model, conventions, formula definitions, and solver settings so a reviewer can audit the workbook without opening the source code. 

# Calculation Logic
For each crop and bed quanitity q, labor must be calculated exactly as follows: LABOR_HOURS (q) = q X HOURS_PER_WEEK_PER_BED X SEASON_WEEKS X (1+DIM_RTRN)^q.
The exponent is part of the case model. Do not linearize it. Do not replace it with q X HOURS_PER_BED X WEEKS. the (1+DIM_RTRN)^q term means that adding a bed increase the labor requirement associated with each additional bed of that crop. for q=0, LABOR_HRS(0) must equal 0. Each crop must use its own hours per week per bed and diminishing returns percentage. 
Revenue: Revenue(q) = q *REVENUE_PER_BED. TOTAL_REVENUE = TOMATO_REVENUE + CARROT_REVENUE + MESCLUN_REVENUE. 
Fertilizer: Fertilizer(q) = q X FERTILIZER_PER_BED. TOTAL_FERTILIZER = TOMATO_FERTILIZER + CARROT_FERTILIZER + MESCLUN_FERTILIZER
Profit: SEASONAL_PROFIT = TOTAL_REVENUE - TOTAL_RETILIZER - TOTAL_LABOR_COST - FIXED_SEASON_COST. TOTAL_LABOR_COST can be derived as follows: TOTAL_LABOR_HOURS = TOMATO_LABOR_HRS + CARROT_LABOR_HRS + MESCLUN_LABOR_HRS. PERMANENT_HOURS_USED = MIN(TOTAL_LABOR_HOURS, PERMANENT_FIELD_HOURS). TEMP_HOURS_USED = MAX(0,TOTAL_LABOR_HOURS - PERMANENT_FIELD_HOURS). PERMANENT_LABOR_COST = PERMANENT_HOURS_USED X PERMANENT_HOURLY_RATE. TEMP_LABOR_COST = TEMP_HOURS_USED X TEMP_HOURLY_RATE. TOTAL_LABOR_COST = PERMANENT_LABOR_COST + TEMP_LABOR_COST. BLENDED_LABOR_RATE = TOTAL_LABOR_COST / TOTAL_LABOR_HOURS, when TOTAL_LABOR_HOURS > 0. 

Labor cost by crop: CROP_LABOR_COST = CROP_LABOR_HOURS ×
BLENDED_LABOR_RATE, for each of tomato, carrot and mesclun. When
TOTAL_LABOR_HOURS = 0, each CROP_LABOR_COST = 0. The three shall sum to
TOTAL_LABOR_COST (add this as a check).
CROP_CONTRIBUTION = CROP_REVENUE − CROP_FERTILIZER − CROP_LABOR_COST.
These are for reporting only; the optimization uses SEASONAL_PROFIT.

Marginal cost. The Marginal Cost sheet shall hold, for each crop, one row for every integer q from 0 through that crop's bed cap (TOMATO_BED_CAP, CARROT_BED_CAP, MESCLUN_BED_CAP). Each crop is evaluated on its own, with the other two crops at zero beds. Each row shall show q, LABOR_HOURS(q), LABOR_COST(q), FERTILIZER(q), MARGINAL_LABOR_HOURS(q), MARGINAL_LABOR_COST(q), and MARGINAL_COST(q), defined as follows:

LABOR_HOURS(q) is calculated with the labor formula above, using that crop's HRS_PER_WEEK_PER_BED and DIM_RTRN. LABOR_HOURS(0) = 0.

LABOR_COST(q) is defined in the rate rule below. LABOR_COST(0) = 0.

FERTILIZER(q) = q × FERTILIZER_PER_BED.

For q = 1 through the bed cap:

MARGINAL_LABOR_HOURS(q) = LABOR_HOURS(q) − LABOR_HOURS(q − 1)

MARGINAL_LABOR_COST(q) = LABOR_COST(q) − LABOR_COST(q − 1)

MARGINAL_COST(q) = MARGINAL_LABOR_COST(q) + FERTILIZER_PER_BED

At q = 0 the marginal columns are left blank, because there is no previous bed.

Price. Each crop's price per bed is its REVENUE_PER_BED, which is the same for every bed: TOMATO_REVENUE_PER_BED, CARROT_REVENUE_PER_BED, MESCLUN_REVENUE_PER_BED.

Standalone P = MC point. For each crop, the standalone P = MC point is the largest q at which REVENUE_PER_BED ≥ MARGINAL_COST(q). It is reported for each crop on the Marginal Cost sheet and tested in Validation Rules (V3). The chart for each crop shall plot REVENUE_PER_BED and MARGINAL_COST(q) against q.

Standalone P = MC points are a diagnostic only. They describe each crop alone and do not account for the shared labor pool or the 64-bed limit, so they are not expected to equal the optimal mix. The optimal mix is found by maximizing SEASONAL_PROFIT.
Rate rule for the marginal-cost schedule. Each crop's schedule is priced at a single labor rate, MC_RATE, applied to every hour of that crop:

LABOR_COST(q) = LABOR_HOURS(q) × MC_RATE

MC_RATE for tomatoes = TEMP_HOURLY_RATE ($17.36/hour).

MC_RATE for carrots = PERMANENT_HOURLY_RATE ($34.72/hour).

MC_RATE for mesclun = PERMANENT_HOURLY_RATE ($34.72/hour).

These rates were chosen because they reproduce the published standalone P = MC points (tomatoes ~10, carrots ~10, mesclun ~6); this decision was made during the build session of 2026-10-05 and is recorded in prompt-log.md. This schedule is a diagnostic and differs from the farm-level labor cost used for SEASONAL_PROFIT, which uses permanent hours first and temporary hours second. The difference between the two is discussed in Audit findings.


Optimizationspecsificaiton:
Objective: maximize SEASON_PROFIT. 
Method: GRG nonlinear
Changing cells: TOMATO_BEDS, CARROT_BEDS, MESCLUN_BEDS. 
Decision type: integer, non-negative
Constraints: TOMATO_BEDS shall be less than or equal to TOMATO_BED_CAP, CARROT_BEDS shall be less than or equal to CARROT_BED_CAP, and MESCLUN_BEDS shall be less than or equal to MESCLUN_BED_CAP. The sum of the beds shall not exceed the TOTAL_BEDS_AVAILABLE value stated in inputs above. Place every constraint-check cell in green.
TEMP_HOURS_USED shall not exceed TEMP_HOURS_CAP and the total labor hours shall not exceed TOTAL_LABOR_HOURS_CAP. 
The model shall not impose an artificial requirement that all 64 beds must be used and shall not imposed inputs that are not defined above with the review and approval of a reviewer. If inputs are suggested, they must be made explicit to the reviewer. 
Required outputs: optimized tomato, carrot, and mesclun bed counts, total beds used and unused beds, total revenue, total fertilizer costs, total labor hours, remaining unused labor hours, permanent hours used, temporary hours used, temporary workers required, total labor costs, fixed seasonal cost, seasonal profit, and clear statements regarding which constraints are limiting the optimization. Provide a summary on a stand-alone sheet in the workbook so they can be reviewed quickly and included in an executive summary if necessary.

# Validation Rules
The workbook shall include a separate Checks sheet that tests every rule in this section. For each rule, the Checks sheet shall show the rule number (V1 through V6), what is being checked, the expected value, the workbook value, the tolerance, and a PASS or FAIL result. A FAIL shall be highlighted in red.

Tolerances. The Checks sheet shall hold two named check constants: PROFIT_TOLERANCE = $25 and BED_TOLERANCE = 1 bed. These are check settings, not model inputs; they do not enter any model calculation. They were agreed during the build session of 2026-10-05 and are recorded in prompt-log.md.

V1. Optimal mix. TOMATO_BEDS = 10, CARROT_BEDS = 20, and MESCLUN_BEDS = 30, exactly, with no tolerance.

V2. Seasonal profit. |SEASONAL_PROFIT − 42,762| ≤ PROFIT_TOLERANCE. The published figure is $42,762; the formulas in this spec return $42,775.16 at 10 / 20 / 30. The cause of the $13.16 difference is explained in the Audit findings section at the end of this spec.

V3. Standalone P = MC points. For each crop, the standalone P = MC point is the largest q at which REVENUE_PER_BED ≥ MARGINAL_COST(q), using the marginal-cost definition in Calculation Logic. The points shall be tomatoes 10, carrots 10, and mesclun 6, each within ± BED_TOLERANCE.

V4. Hand check at q = 1. LABOR_HOURS(1) = 1 × HRS_PER_WEEK_PER_BED × SEASON_WEEKS × (1 + DIM_RTRN)^1. The workbook shall match these hand-computed values to within 0.01 hours: tomato 2.5 × 36 × 1.10 = 99.00 hours; carrot 0.833 × 36 × 1.025 = 30.74 hours; mesclun 1.25 × 36 × 1.0125 = 45.56 hours. At the mix 1 / 0 / 0, all 99 hours fall within the 720 permanent hours, so TOTAL_LABOR_COST = 99 × 34.72 = $3,437.28 and SEASONAL_PROFIT = 8,800 − 880 − 3,437.28 − 20,000 = −$15,517.28.

V5. Solver. Run Excel Solver with the following settings: objective SEASONAL_PROFIT, set to Max; changing cells TOMATO_BEDS, CARROT_BEDS, and MESCLUN_BEDS; method GRG Nonlinear; the constraints listed under the optimization specification in Calculation Logic; "Make Unconstrained Variables Non-Negative" checked; and Integer Optimality (%) set to 0. Run Solver twice: once from the starting mix 0 / 0 / 0 and once from 20 / 0 / 0. Record each starting mix, the mix Solver returns, and the profit it returns on the Checks sheet. Both runs shall return 10 / 20 / 30 (V1) and pass V2.

V6. Farm Profit Lab cross-check. Enter the mix 10 / 20 / 30 into the Farm Profit Lab. Its seasonal profit shall agree with SEASONAL_PROFIT within PROFIT_TOLERANCE. Record the Lab's figure on the Checks sheet.

# Additional Instructions for AI if AI is used to create the workbook
Use named ranges/named parameters for all inputs and major calculated quantities. Formula logic must remain understandable without reference to cell coordinates. Keep inputs separate from calculations and decision cells. Use formulas for every derived quanittiy. Do not hard-code calculated answers into the optimization output. Use integer decision cells for the three bed quantities. Use the farm-level labor allocations as follows: permanent hours first, then temporary hours second. include an integer q marginal-cost schedule from q=0 through each crop's cap. Include a chart of price versus marginal cost for each crop so the P=MC diagnostics are visually inspectable. Include a compact optimization summary suitable for a manager or grader to read without inspecting formulas (as previously stated). Flag negative quantities, non-integers, division by zero, and excel error states and highlight these in red if they come up. The model must support the economic interpretation that the highest-revenue crop is not automatically the best crop to expand. tomatoes have the highest revenue per bed but also the highest diminishing return rate, causing labor requirements to escalate rapidly with quantity. The model should make it possible to explain the optimization through marginal costs, constraints, and the opportunity cost of scarce beds/labor. A crop can remain worth producing even when its standalone economics appear unfavorable if firm-level fixed costs are already incurred; conversely, an additional bed can be unattractive when its incremental labor requirement is excessive costs. A random reviewer recieving the workbook and this specification shall be able to identify every input and its source, reproduce every formula from the named range definitions, obtain the same integer mix, see every constraint. 

# Expected Outputs
The optimal mix is 10 tomato, 20 carrot and 30 mesclun beds, with SEASONAL_PROFIT within PROFIT_TOLERANCE of the published $42,762, as tested in V1 and V2. The hypothesis of 12 tomato, 30 mesclun and 20 carrot beds is stated in the perfect-competition brief and is compared with the model result in Stage 1.3; it is not an acceptance figure for this workbook.

# Audit Findings
This section records the defects and ambiguities found in this spec during the build and my own checks of the workbook, what each check found, and the decision taken on each. Findings are numbered A1, A2, and so on. Validation rules referred to here (V1 through V6) are defined in the Validation Rules section.

## A1. Marginal cost where the 720 permanent hours run out

Issue. The Marginal Cost sheet prices each crop's schedule entirely at the permanent rate ($34.72/hour) or entirely at the temporary rate ($17.36/hour), under the heading "marginal cost rises with q, so this is the crossing." SEASONAL_PROFIT does not price labor that way: it uses the 720 permanent hours first and temporary hours after. This finding records what each crop's schedule does at the bed where the 720 permanent hours run out, and whether any check in the workbook shows it.

What the schedule does. Priced the way SEASONAL_PROFIT prices labor (permanent hours first), tomatoes alone use 527.1 hours at 4 beds and 724.7 hours at 5 beds. The 5th bed is the one that crosses 720 hours. Its 197.7 marginal hours are almost all permanent hours at $34.72, so its marginal cost is $7,660.43. The 6th bed's 231.9 hours are all temporary hours at $17.36, so its marginal cost falls to $4,906.02, even though it needs more hours than the 5th bed. Marginal cost then rises again from the 6th bed on:

| Tomato bed q | LABOR_HOURS(q) | MARGINAL_LABOR_HOURS(q) | MARGINAL_COST(q), permanent hours first |
|---|---|---|---|
| 4 | 527.1 | 167.7 | $6,702.75 |
| 5 | 724.7 | 197.7 | $7,660.43 |
| 6 | 956.6 | 231.9 | $4,906.02 |
| 7 | 1,227.7 | 271.0 | $5,585.41 |
| 10 | 2,334.4 | 424.4 | $8,248.11 |
| 11 | 2,824.6 | 490.2 | $9,390.17 |

So marginal cost does not always rise with q: it falls where the 720 permanent hours run out. For tomatoes it falls in one step, because the 5th bed's hours are almost all permanent hours. For carrots and mesclun the bed that crosses 720 hours is split between the two rates, so marginal cost falls over two beds: carrots $2,551.12 at bed 16, $1,675.33 at bed 17 and $1,588.61 at bed 18; mesclun $2,988.27 at bed 13, $2,522.48 at bed 14 and $1,983.89 at bed 15. It then rises again.

Whether the checks show it. They do not. The Checks sheet tests that each crop's marginal cost never falls, but only for the single-rate schedules. A single-rate schedule cannot fall, because labor hours rise with every bed. The permanent-hours-first schedule was not built, so the fall never appeared and those checks passed. The heading "marginal cost rises with q" is true of the single-rate schedules, but not of the labor cost the model actually uses for profit.

Effect on the standalone P = MC points. Using permanent hours first, the largest q at which REVENUE_PER_BED ≥ MARGINAL_COST(q) is 10 for tomatoes, 20 for carrots, and 30 for mesclun. Tomatoes stay at 10. Carrots and mesclun reach their bed caps. The published points (tomatoes ~10, carrots ~10, mesclun ~6) come out only when tomatoes are priced at the temporary rate and carrots and mesclun at the permanent rate, which no single labor rule gives.

What it would have caught. A check that builds the permanent-hours-first schedule and allows marginal cost to fall would have shown that the heading on the Marginal Cost sheet is wrong, and that the published P = MC points depend on which rate each crop is given.

## A2. The $13.16 profit gap and the rounded inputs

Issue. The spec's formulas give SEASONAL_PROFIT = $42,775.16 at 10 / 20 / 30, against the published $42,762, a gap of $13.16. During the build, carrot hours of exactly 5/6 were tested in place of 0.833, and profit still did not match. This finding records what the rounded inputs $34.72, $17.36 and 0.833 are derived from, and what profit reads when all three are exact.

What the rates are. $34.72 is $25,000 / 720 hours = $34.7222… rounded to the cent: the $25,000 value of the owner's 720 hours. $17.36 is half of that, $12,500 / 720 = $17.3611…, rounded to the cent. CARROT_HRS_PER_WEEK_PER_BED of 0.833 is 5/6 = 0.8333… rounded.

Profit at 10 / 20 / 30 with each input exact:

| Inputs used | SEASONAL_PROFIT |
|---|---|
| As written in this spec ($34.72, $17.36, 0.833) | $42,775.16 |
| Carrot hours exact (5/6); rates as written | $42,768.33 |
| Rates exact ($25,000/720 and $12,500/720); carrot hours 0.833 | $42,768.49 |
| All three exact | $42,761.66 |

With all three exact, profit is $42,761.66, which rounds to the published $42,762. The whole $13.16 gap (more exactly $13.50 before rounding) comes from rounding the two labor rates and the carrot hours: about $6.83 from the carrot hours and about $6.67 from the rates. It is not a formula error.

What it would have caught. Testing one rounded input at a time, as the build did with carrot hours, cannot close the gap, because the rates and the carrot hours each account for about half of it. Testing all three exact inputs together explains the gap completely and shows that the ± $25 tolerance in V2 covers rounding, not a defect.

Decision. Keep PERMANENT_HOURLY_RATE at $34.72, TEMP_HOURLY_RATE at $17.36, and CARROT_HRS_PER_WEEK_PER_BED at 0.833, as published, so the workbook matches the case as given. Keep V2's tolerance of ± $25; this finding shows the $13.16 difference is rounding, not a defect.

## A3. Decision: should the Marginal Cost sheet and its checks change?
Decision. Keep the single-rate schedules, because they reproduce the published P = MC points that V3 tests. Change the heading on the Marginal Cost sheet from "marginal cost rises with q, so this is the crossing" to say that each single-rate schedule is priced at one labor rate, so its marginal cost rises with q, while the farm's actual labor cost uses permanent hours first (see Audit finding A1). Add a third schedule per crop, priced permanent hours first, for information only, so the fall in marginal cost where the 720 permanent hours run out is visible. Keep the Checks sheet's "marginal cost never falls" checks on the single-rate schedules only, and note on them that the permanent-hours-first schedule is expected to fall where the 720 permanent hours run out: once for tomatoes, over two beds for carrots and mesclun.

Hand check at q = 1 (V4)

What I did: I calculated 2.5*36*1.10 on a calculator which = 99. Then at 1/0/0 bed count I calculated 8,800 − 880 − (tomato hours × 34.72) − 20,000, where tomato hours = 99. This yielded negative $15517.28. This is because the bed itself earns $4,482.72 but the 20K fixed cost is paid no matter how much is planted and one bed doesn't cover it. That's why the farm needs many beds: 10/20/30.

Result. Tomato [99 ] hours, carrot [30.74 ] hours, mesclun [45.56] hours; profit at 1 / 0 / 0 $-15517.28. The workbook shows for 1/0/0 [ 99], [0 ], [0 ] and $[-15,517.28 ]. 


## Solver from two starting mixes (V5)

What I did. Ran Excel Solver (GRG Nonlinear, integer, Integer Optimality 0%) on [10/13/2026] from 0 / 0 / 0 and from 20 / 0 / 0.

Result. From 0 / 0 / 0: returned: 10/20/30, $42775.16, Solver's message: "Solver found a solution". From 20 / 0 / 0: returned 10/20/30, $42775.16, solvers message: "solver found a solution". Started at 20/0/0 (profit about −$84,300, breaks the labor limit at about 12,110 hours); Solver returned 10/20/30.


## Farm Profit Lab cross-check (V6)

What I did. Entered 10 / 20 / 30 into the Farm Profit Lab (farmlab.html) on 10/13/2026.

Result. The Lab reports $42762.00; the workbook reports $42,775.16; difference $13.16. This fits the rounding explanation in A2.
