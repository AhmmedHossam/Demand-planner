# Demand-planner



1. Demand Forecast

Step 1: Organize the Data

You need to analyze historical weekly sales to project the future demand.

Data Columns:
	•	Product ID
	•	Product Name
	•	Number of Orders
	•	Quantity Ordered (This is the key metric for demand forecasting)
	•	Untaxed Total
	•	Quantity On Hand

Since data spans 8 months of weekly sales (~32 weeks), use that to calculate:
	•	Average Weekly Demand
	•	Recent Trend (last 4–8 weeks) if needed

⸻

Step 2: Choose Forecasting Method

You can use a simple moving average or exponential smoothing, depending on the product’s demand consistency.

For simplicity and broad applicability:

Method: 4-Week Moving Average

Use this for short-term prediction if demand patterns are relatively stable.

Formula (assuming weeks in columns):

=AVERAGE(B2:E2)   <-- for the last 4 weeks

If your data is in rows by product and columns by week:


=AVERAGE(B2:E2)   <-- for the last 4 weeks

You may also calculate:

Weighted Moving Average (if recent weeks are more important)

=(0.4*Week4 + 0.3*Week3 + 0.2*Week2 + 0.1*Week1)

Step 3: Forecast for September

Since September has ~4 weeks, multiply weekly forecast by 4.

Formula:

=Forecasted_Weekly_Demand * 4

Create “Demand Forecast” Sheet

Columns to include:
	•	Product ID
	•	Product Name
	•	Forecasted Weekly Demand
	•	Forecasted Monthly Demand

⸻

2. Replenishment Plan

note:  
{
Lead time in planning and supply chain management refers to the total time it takes from when a replenishment order is placed until the goods are received and ready for use or sale.

⸻

In Simple Terms:

It’s the delay between:
	•	Placing an order (to a supplier or internal manufacturing), and
	•	Receiving the stock into inventory.

⸻

Types of Lead Time:
	1.	Supplier Lead Time: Time taken by the supplier to process and deliver the order.
	2.	Manufacturing Lead Time: Time needed to produce the item (if made in-house).
	3.	Transportation Lead Time: Time for shipping/delivery.
	4.	Administrative Lead Time: Time spent on approvals, documentation, etc.

⸻

Why It Matters:
	•	Demand Forecasting: Helps determine when to place orders.
	•	Reorder Point (ROP) Planning: ROP = Demand during Lead Time + Safety Stock.
	•	Avoiding Stockouts: If you don’t consider lead time, you may run out of stock before the next batch arrives.
}
 
Step 1: Calculate Safety Stock

Assumption: Lead time = 5 days (~0.71 weeks)

Safety Stock Formula:

=Z * σ * √L

Z = Service level factor (e.g., 1.65 for 95% service level)

σ = Standard deviation of weekly demand

L = Lead time in weeks

In Excel:

=1.65 * STDEV(Week1:WeekN) * SQRT(0.71)

This ensures you’re covered for variability during lead time.

Step 2: Calculate Re-order Point (ROP)

ROP Formula:

= (Average Weekly Demand * Lead Time in weeks) + Safety Stock

In Excel:

=Forecasted_Weekly_Demand * 0.71 + Safety_Stock

Step 3: Calculate Replenishment Quantity

If current stock is below ROP, calculate how much to order

=IF(Quantity_On_Hand < ROP, Forecasted_Monthly_Demand - Quantity_On_Hand, 0)

Step 4: Determine Replenishment Order Date

Assume a 5-day lead time. If stock falls below ROP during a week, plan order 5 days before expected stock-out.

To calculate expected stock-out:
	•	Use a running stock depletion formula
	•	Estimate which week you’ll run out

⸻

Step 5: Classify Product (MTS vs MTO)

Basic Criteria.    
•	MTS: Products with consistent demand (low standard deviation, frequent orders)
•	MTO: Products with sporadic demand (high variability or low order frequency)

In Excel, use:

=IF(STDEV(Week1:WeekN)/AVERAGE(Week1:WeekN) < 0.5, "MTS", "MTO")

Create “Replenishment Plan” Sheet

Columns:

- Product ID
- Product Name
- Forecasted Weekly Demand
- Quantity On Hand
- Re-order Point
- Safety Stock Level
- Recommended Replenishment Quantity (if stock is projected to fall below the re-order point)
- Replenishment Order Date
