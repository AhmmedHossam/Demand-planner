# Demand-planner

Step 1: Demand Forecasting
We will use historical sales data to forecast demand for September. A common forecasting method is Moving Average or Exponential Smoothing.

Method Selection
Simple Moving Average (SMA): If demand is stable.

Exponential Smoothing (ETS): If demand shows trends or seasonality.

Since we have weekly data for 8 months (≈32 weeks), an SMA of the last 4 weeks or ETS in Excel will be appropriate.

Steps in Excel
Insert a new sheet: Name it "Demand Forecast".

Use Moving Average Formula:

Select the last 4 weeks of Quantity Ordered for each product.

Use the formula:

excel

=AVERAGE(OFFSET(INDIRECT("Historical Sales Data!D2"), MATCH(A2, INDIRECT("Historical Sales Data!A:A"), 0)-1, 0, 4, 1))
This calculates the 4-week moving average.

Drag the formula down for all products.

Using Exponential Smoothing (ETS)

Use Excel’s built-in ETS function:

excel

=FORECAST.ETS(TODAY()+7, D2:D32, B2:B32)
Replace D2:D32 with the historical sales for that product.

Step 2: Safety Stock & Reorder Point Calculation
We define:

Safety Stock (SS) = (Max weekly demand − Average weekly demand) × Lead Time

excel

= (MAX(D2:D32) - AVERAGE(D2:D32)) * 5
Reorder Point (ROP) = (Lead Time × Avg. Weekly Demand) + Safety Stock

excel

= (5 * AVERAGE(D2:D32)) + SS
Steps in Excel
Insert a new sheet: Name it "Replenishment Plan".

Use formulas for SS and ROP for each product.

Step 3: Classification into MTS or MTO
MTS (Make-to-Stock): If demand is consistent.

MTO (Make-to-Order): If demand fluctuates significantly.

Formula to Classify
excel

=IF(STDEV(D2:D32)/AVERAGE(D2:D32) < 0.3, "MTS", "MTO")
If Coefficient of Variation (CV) < 0.3, product is MTS.

Otherwise, it is MTO.

Step 4: Replenishment Plan
To determine the replenishment quantity and order date:

Recommended Replenishment Quantity

excel

=IF(F2 < G2, G2 - F2, 0)
If stock (F2 = Quantity on Hand) falls below ROP (G2), we order.

Replenishment Order Date

excel

=IF(H2>0, TODAY(), "")
If we need to replenish, order today.
