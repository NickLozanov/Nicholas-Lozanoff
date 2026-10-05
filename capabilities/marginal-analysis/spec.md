# Spec: Marginal Analysis

Define here what this capability does, its inputs, and its expected output — before you build it.
# Capability
The farm must choose integer quantities of tomato, carrot, and mesclun grow beds to maximize seasonal farm profit. The desicion is not simply to maximize revenue or fill all 64 beds. The capability (also called "model" in this spec interchangeably) must account for crop-specific diminishing returns in labor, the farm's staff available labor, fertilizer costs, fixed costs, crop bed caps, the 64-bed farm capacity, and the temporary-worker capacity. The primary objective is to maximize the profit for the 36-week season and the primary decision variables is determining quantity of growbeds dedicated to each type of crop, tomato, carrot, and mesclum. All three shall be non-negative integers. 

# Inputs
Every input shall exist as a named input/range or equivalent named parameter in the workbook. The model shall not encode these values only inside formulas. The model shall expose them in an inputs region so a reviewer can change a case assumption without editing formulas. 

# Expected Outputs

Replace this placeholder once you start this capability.
