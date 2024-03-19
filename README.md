# Pricing
Finding **optimal pricing strategy** for each SKU to maximize demand or achieve specific business objectives.

## Problem Description
This problem addresses the challenge of finding the right pricing strategy for each SKU to balance **demand, markdown costs, and inventory** constraints.

### Problem Formula
We have historical sales data for *m* SKUs, which includes its sales quantity (also called *sold*) under certain markdown (MD). For **the set of SKUs**,  we define:
- **Demand Calculation (DEMAND):** Demand represents the total demand for all SKUs, calculated as the sum of the demand for each SKU.
  
  $$DEMAND = \sum_{i=1}^{m}demand_i = \sum_{i=1}^m (sold_i * MSRP_i * MD_i)$$
- **Inventory Calculation (INVENTORY):** Inventory represents the total inventory for all SKUs, accounting for sales quantities.
  
  $$INVENTORY = \sum^{m}(inventory_i - sold_-i)$$
- **Weighted Markdown (Weighted MD):** Weighted MD represents the weighted average of markdowns (discount rates) for all SKUs, where the weights are determined by demand, prices, and sales quantities. It measures the overall markdown effectiveness.

  $$Weighted \ MD = 1- {\sum_{i=1}^mDamand_i} / {((\sum_{i=1}^m Sold_i)(\sum_{i=1}^m MSRP_i))}$$

The problem also involves **fitting a demand curve** for each SKU to estimate the demand at different price points. The demand curve is typically estimated using one of three functions:
1. **Linear Function:**  $ sold = a*MD+b $
2. **Exponential Function:** $ sold= a*\exp(b*MD) $
3. **Logarithmic Function:** $ sold = a + b*\log(MD+1) $
Where $a$ and $b$ are constants determined by curve fitting

### Problem Constraints
- Each SKU has **upper and lower bounds for markdown (MD)** that must be adhered to.
- Each SKU has a**n initial inventory level** that impacts demand and sales.
- The optimization problem aims to **find the optimal markdown strategy** for each SKU.

## INPUT
1. *Pricing_Data.xlsx* : Historical sales data with the following columns

    | SKU | MSRP | SALES_QTY | SALES_AMT | MD_LOWER_BOUND | MD_UPPER_BOUND | INVENTORY_BOUND |
    |-----|------|-----------|-----------|----------------|----------------|-----------------|

2. `Target weighted MD` = x *(float bewteen 0 and 1)*

3. `Target demand` = y *(int)*

4. `Target inventory` = z *(int)*

## OUTPUT
***Pricing_Result.xlsx***

1. **SUMMARY** sheet
    summary of the optimization results,including `DEMAND`, `SALES_QTY`, `INVENTORY`, `WEIGHTED_MD` of following scenarios:

    ||Scenario Name|Description|
    |:---|:---|:---|
    |1|MAX_DEMAND|maximum totaal demand|
    |2|MIN_W_MD|minimize weighted MD|
    |3|MAX_W_MD|maximize weighted MD|
    |4|TGT_MD|when weighted MD = `x`，maximize total demand|
    |5|TGT_DEMAND|when demand = `y`，minimize weighted M|
    |6|TGT_INV|when inventory = `z`，minimize weighted M|
    |7|W_MD_DEMAND|when inventory < `z`，minimize weighted MD and meanwhile maximize demand|
    |8|DEMAND_INV|when weighted MD < `x`, maximize demand and meanwhile minimize inventory|
    |9|W_MD_INV|when demand > `y`, minimize weighted MD and meanwhile minimize inventory|
    |10|W_MD_DEMAND_INV|minimize weighted MD, maximize demand, and meanwhile minimize inventory|

1. **RESULT** sheet: 
    SKU-level optimization result

    | Column Name | Description |
    |-------------|-------------|
    | `SKU` | SKU identifier |
    | `MSRP` | Manufacturer's Suggested Retail Price |
    | `MD_LOWER_BOUND` | Lower bound for markdown |
    | `MD_UPPER_BOUND` | Upper bound for markdown |
    | `INV` | Inventory bound |
    | `a` | First parameter of the fitted curve |
    | `b` | Second parameter of the fitted curve |
    | `METHOD` | Fitted curve function type |
    | `MSE` | Mean Squared Error (MSE) of the fitted curve |
    | `scenario_name_MD` | Recommended markdown for each SKU under a certain scenario |
    | `scenario_name_SALES` | Sales quantity of each SKU based on the recommended markdown |
    | `scenario_name_DEMAND` | Demand for each SKU based on the recommended markdown |


2. **DEMAND_SALES** sheet: 
   demand and sales data for each SKU at different markdown points

    | Column Name | Description |
    |-------------|-------------|
    | `SKU` | SKU identifier |
    | `MD=m D` | Demand according to the fitted curve when MD = `m` |
    | `MD=m S` | Sales quantity according to the fitted curve when MD = `m` |
