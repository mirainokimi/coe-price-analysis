# Is COE Price in Singapore affected by CPI and bank interest rates?
This repository contains the data analysis project investigating how Certificate of Entitlement (COE) prices in Singapore relate to Consumer Price Index (CPI) and bank interest rates.

## Datasets

The project uses publicly available data from the data.gov.sg :

### Current Banks Interest Rates (End Of Period), Monthly

Source: MONETARY AUTHORITY OF SINGAPORE
<br>URL: https://data.gov.sg/datasets/d_5fe5a4bb4a1ecc4d8a56a095832e2b24/view
<br>Description:
<br>This dataset provides a historical series of monthly bank interest rates in Singapore, covering the period from January 1988 through May 2025. It includes multiple series such as:
<br>1)Government bond yields (2‑, 5‑, 10‑, 15‑, 20‑year)
<br>2)Treasury bill yield (1‑year)
<br>3)Overnight money market rates

### COE Bidding Results / Prices
Source: LTA (Land Transport Authority)
<br>URL: https://data.gov.sg/datasets/d_69b3380ad7e51aff3a7dcc84eba52b8a/view
<br>Description:
<br>A detailed record of Certificate of Entitlement (COE) bidding results in Singapore, including data for each bidding exercise:
<br>1)Month (YYYY‑MM)
<br>2)Bidding No (round number)
<br>3)Vehicle Class (A, B, C, D, E)
<br>4)Quota (number of COEs released)
<br>5)Bids Success (successful bids count)
<br>6)Bids Received (total bids)
<br>7)Premium (winning bid price)

### Consumer Price Index (CPI), 2019 As Base Year, Monthly, Seasonally Adjusted
Source: SINGAPORE DEPARTMENT OF STATISTICS
<br>URL: https://data.gov.sg/datasets/d_ba8a05c8908b5e1dc13540286d585f8a/view
<br>Description: This dataset provides the monthly, seasonally adjusted Consumer Price Index (CPI) for Singapore, using 2019 as the base year. The CPI measures the average price changes over time of a fixed basket of goods and services commonly purchased by resident households. Seasonal adjustment removes recurring fluctuations due to seasonal patterns, offering a clearer view of underlying inflation trends. It includes overall CPI as well as breakdowns by categories such as food, housing, transport, and healthcare.
## Justification for Variable Selection
As a measure of CPI, I chose “All Items” as it reflects the broadest measure of inflation faced by consumers, incorporating both core and volatile components (like food and transport). This makes it more representative of the general cost of living and better aligned with macroeconomic sentiment and household purchasing power.

<br>For bank interest rates, although government bond yields and compounded SORA (e.g. 3-month) were available, I used SORA because:
It serves as a cleaner reflection of short-term interest rate movements set by MAS.
The interest rate recorded for each month represents the value at the very end of that month, not an average over the month. The data being recorded on a monthly basis matches better with the bi-monthly updates of COE prices than the rest.
It is less smoothed than 3-month compounded alternatives and therefore may capture more immediate shifts in interest rate policy, which could impact vehicle financing costs, consumer borrowing, and bidding behavior.
It also aligns better with the monthly frequency of the CPI and COE datasets I used, maintaining consistency in time granularity.
## Data Preprocessing
The datasets were preprocessed to ensure temporal alignment and methodological consistency. First, all records were filtered to begin from January 2019, harmonizing the analysis period with the CPI’s base year adjustments.For the Singapore Overnight Rate Average (SORA), daily values were aggregated into monthly averages to match the COE bidding cycle’s temporal resolution.
<br>I then merged the datasets using the ‘Month’ field as the primary key, creating a unified time-series framework. To mitigate scale disparities, I standardised across features (e.g., COE premiums vs. CPI indices), all numeric variables were standardized to a mean of 0 and unit variance using z-score normalization.

## Exploratory Data Analysis (EDA)

Trend Plots: Time series plots show CPI and SORA both generally increasing post-COVID.
<br>Correlation Matrix showed that COE premiums has a moderate positive correlation with both CPI and SORA.

## Modelling & Analysis
1) Linear Regression (OLS)
<br>Formula: COE Premium ~ CPI + SORA
<br>R^2 Score (Test): ~0.196
<br>Coefficient Signs: CPI (+), SORA (-)

2) Ridge and Lasso Regression
<br>Ridge Coefficients: [15672.96, -2913.35]
<br>Lasso Coefficients: [15710.57, -2946.21]
<br>R^2 Scores similar to OLS (~0.196)

3) Variance Inflation Factor (VIF)
<br>CPI VIF: 3.06
<br>SORA VIF: 3.06
<br>Conclusion: No severe multicollinearity

4) Residual Analysis
<br>Residuals are roughly normally distributed, suggesting good model fit assumptions.

## Findings

COE premiums show a moderate correlation with both CPI and SORA, with a stronger relationship observed with CPI. Linear regression results indicate that CPI has a positive effect on COE prices. As the overall cost of living rises, COE premiums also tend to increase. This suggests that inflation, which may reflect stronger consumer demand or economic confidence, can drive up car ownership demand. In contrast, SORA has a negative relationship with COE prices. This makes sense, since higher short-term interest rates raise borrowing costs, which can reduce demand for car loans and discourage COE bidding activity. However, the model’s R² score is only around 0.2, meaning CPI and SORA explain just a small portion of the variation in COE prices. This suggests that other factors, such as quota changes, government policy, or market sentiment, likely play a much larger role.







