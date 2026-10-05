# Spec: Marginal Analysis

Define here what this capability does, its inputs, and its expected output — before you build it.
# Capability
The farm must choose integer quantities of tomato, carrot, and mesclun grow beds to maximize seasonal farm profit. The desicion is not simply to maximize revenue or fill all 64 beds. The capability (also called "model" in this spec interchangeably) must account for crop-specific diminishing returns in labor, the farm's staff available labor, fertilizer costs, fixed costs, crop bed caps, the 64-bed farm capacity, and the temporary-worker capacity. The primary objective is to maximize the profit for the 36-week season and the primary decision variables is determining quantity of growbeds dedicated to each type of crop, tomato, carrot, and mesclum. All three shall be non-negative integers. 

# Inputs
Every input shall exist as a named input/range or equivalent named parameter in the workbook. The model shall not encode these values only inside formulas. The model shall expose them in an inputs region so a reviewer can change a case assumption without editing formulas. Inputs are as follows and are separated into names of inputs, value of input, and unit of input:
name = TOMATO_BED_CAP, quantity = 20, unit = beds
name = CARROT_BED_CAP, quantity = 20, unit = beds
name = MESCLUN_BED_CAP, quantity = 30, unit = beds
name = TOMATO_REVENUE_PER_BED, quantity = 8800, unit = $/bed/season
name = CARROT_REVENUE_PER_BED, quantity = 2094, unit = $/bed/season
name = MESCLUN_REVENUE_PER_BED, quantity = 2700, unit = $/bed/season
name = TOMATO_HRS_PER_WEEK_PER_BED, quantity = 2.5, unit = hour/week/bed
name = CARROT_HRS_PER_WEEK_PER_BED, quantity = 0.833, unit = hour/week/bed
name = MESCLUN_HRS_PER_WEEK_PER_BED, quantity = 1.25, unit = hour/week/bed
name = TOMATO_FERTILIZER_PER_BED, quantity = 880, unit = $/BED/SEASON
name = CARROT_FERTILIZER_PER_BED, quantity = 440, unit = $/BED/SEASON
name = MESCLUN_FERTILIZER_PER_BED, quantity = 880, unit = $/BED/SEASON
name = TOMATO_DIM_RTRN_%, quantity = 10, unit = %
name = CARROT_DIM_RTRN_%, quantity = 2.5, unit = %
name = MESCLUN_DIM_RTRN_%, quantity = 1.25, unit = %
name = SEASON_WEEKS, quantity = 36, unit = weeks
name = TOTAL_BEDS_AVAILABLE, quantity = 64, unit = beds
name = PERMANENT_FIELD_LABOR_HOURS, quantity = 720, unit = hours/season
name = PERMANENT_HOURLY_RATE, quantity = 34.72, unit = $/hour
name = TEMP_WORKER_CAP, quantity = 4, unit = workers
name = TEMP_HOURS_LABOR_PER WORKER, quantity = 1440, unit = hours/worker/season
name = TEMP_HOURLY_RATE, quantity = 17.36, unit = $/hour
name = FIXED_SEASON_COST, quantity = 20000, unit = $/season






# Expected Outputs

Replace this placeholder once you start this capability.
