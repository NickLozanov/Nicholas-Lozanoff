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

Derived inputs include the following: TEMP_HOURS_CAP = TEMP_HOURS_LABOR_PER WORKER X TEMP_WORKER_CAP = 5760 hours
TOTAL_LABOR_HOURS_CAP = 5760 hours + PERMANENT_FIELD_LABOR_HOURS = 6480 HOURS.
# Workbook Structure
The workbook shall be structured in the following mannar:
Inputs: named case inputs, units, source labels, and editable assumptions as listed above in the inputs section. 
Cost Structure: For the current bed mix: beds, revenue, fertilizer, crop labor hours, total labor hours, permanent/temporary labor allocation, labor dollars, total costs, and profit
Marginal cost schedules: integer q=0 through each crop's bed cap. show labor hours, labor dollars, fertilizer, marginal labor hours, marginal labor cost, and marginal total cost. Include standalone P = MC diagnostics for each crop. 
Optimization: Decision cells for the three integer bed counts, objective/profit, constraints, solver-ready formulas, and the optimized solution. If any decision recommendations are included here, highlight them for the reviewer's clarity and ease of navication.
Checks: Acceptance tests, constraint checks, formula/error checks, hand-calculation check, and reconciliation to the published test suite. 
Read me / spec: sort description of the model, conventions, formula definitions, and solver settings so a reviewer can audit the workbook without opening the source code. 
# Calculation Logic
For each crop and bed quanitity q, labor must be calculated exactly as follows: LABOR_HOURS (q) = q X HOURS_PER_WEEK_PER_BED X SEASON_WEEKS X (1-DIM_RTRN)^q.
The exponent is part of the case model. Do not linearize it. Do not replace it with q X HOURS_PER_BED X WEEKS. the (1+DIM_RTRN)^q term means that adding a bed increase the labor requirement associated with each additional bed of that crop. for q=0, LABOR_HRS(0) must equal 0. Each crop must use its own hours per week per bed and diminishing returns percentage. 
Revenue: Revenue(q) = q *REVENUE_PER_BED. TOTAL_REVENUE = TOMATO_REVENUE + CARROT_REVENUE + MESCLUN_REVENUE. 
Fertilizer: Fertilizer(q) = q X FERTILIZER_PER_BED. TOTAL_FERTILIZER = TOMATO_FERTILIZER + CARROT_FERTILIZER + MESCLUN_FERTILIZER
Profit: SEASONAL_PROFIT = TOTAL_REVENUE - TOTAL_RETILIZER - TOTAL_LABOR_COST - FIXED_SEASON_COST. TOTAL_LABOR_COST can be derived as follows: TOTAL_LABOR_HOURS = TOMATO_LABOR_HRS + CARROT_LABOR_HRS + MESCLUN_LABOR_HRS. PERMANENT_HOURS_USED = MIN(TOTAL_LABOR_HOURS, PERMANENT_FIELD_HOURS). TEMP_HOURS_USED = MAX(0,TOTAL_LABOR_HOURS - PERMANENT_FIELD_HOURS). PERMANENT_LABOR_COST = PERMANENT_HOURS_USED X PERMANENT_HOURLY_RATE. TEMP_LABOR_COST = TEMP_HOURS_USED X TEMP_HOURLY_RATE. TOTAL_LABOR_COST = PERMANENT_LABOR_COST + TEMP_LABOR_COST. BLENDED_LABOR_RATE = TOTAL_LABOR_COST / TOTAL_LABOR_HOURS, when TOTAL_LABOR_HOURS > 0. 
Optimizationspecsificaiton:
Objective: maximize SEASON_PROFIT. 
Method: GRG nonlinear
Changing cells: TOMATO_BEDS, CARROT_BEDS, MESCLUN_BEDS. 
Decision type: integer, non-negative
Constraints: TOMATO_BEDS shall be greater than or equal to TOMATO_BED_CAP and the same for the carrot beds and mesclun beds. The sum of the beds shall not exceed the TOTAL_BEDS_AVAILABLE value stated in inputs above. Place every constraint-check cell in green.
TEMP_HOURS_USED shall not exceed TEMP_HOURS_CAP and the total labor hours shall not exceed TOTAL_LABOR_HOURS_CAP. 
The model shall not impose an artificial requirement that all 64 beds must be used and shall not imposed inputs that are not defined above with the review and approval of a reviewer. If inputs are suggested, they must be made explicit to the reviewer. 
Required outputs: optimized tomato, carrot, and mesclun bed counts, total beds used and unused beds, total revenue, total fertilizer costs, total labor hours, remaining unused labor hours, permanent hours used, temporary hours used, temporary workers required, total labor costs, fixed seasonal cost, seasonal profit, and clear statements regarding which constraints are limiting the optimization. Provide a summary on a stand-alone sheet in the workbook so they can be reviewed quickly and included in an executive summary if necessary. 
# Additional Instructions for AI if AI is used to create the workbook
Use named ranges/named parameters for all inputs and major calculated quantities. Formula logic must remain understandable without reference to cell coordinates. Keep inputs separate from calculations and decision cells. Use formulas for every derived quanittiy. Do not hard-code calculated answers into the optimization output. Use integer decision cells for the three bed quantities. Use the farm-level labor allocations as follows: permanent hours first, then temporary hours second. include an integer q marginal-cost schedule from q=0 through each crop's cap. Include a chart of price versus marginal cost for each crop so the P=MC diagnostics are visually inspectable. Include a compact optimization summary suitable for a manager or grader to read without inspecting formulas (as previously stated). Flag negative quantities, non-integers, division by zero, and excel error states and highlight these in red if they come up. The model must support the economic interpretation that the highest-revenue crop is not automatically the best crop to expand. tomatoes have the highest revenue per bed but also the highest diminishing return rate, causing labor requirements to escalate rapidly with quantity. The model should make it possible to explain the optimization through marginal costs, constraints, and the opportunity cost of scarce beds/labor. A crop can remain worth producing even when its standalone economics appear unfavorable if firm-level fixed costs are already incurred; conversely, an additional bed can be unattractive when its incremental labor requirement is excessive costs. A random reviewer recieving the workbook and this specification shall be able to identify every input and its source, reproduce every formula from the named range definitions, obtain the same integer mix, see every constraint. Include a separate check sheet in the workbook which includes the following: optimal mix = tomatoes = 10, carrots = 20, and mesclun =30. seasonal profit should be $42,762 and standalone P=MC points should be tomatoes ~10, carrots ~10 mesclun ~6 beds. 



# Expected Outputs

It is expected that the optimal mix of beds will be 12 tomatoes, 30 mesclun, and 20 carrots as discussed and detailed in the perfect-competition-brief
