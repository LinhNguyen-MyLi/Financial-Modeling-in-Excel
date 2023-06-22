# Financial-Modeling
Based on my experience, I will summarize a guideline to build up financial modeling in Excel. Please feel free to correct my mistakes.
Before we begin, let's take a brief look at the illustration below to know the procedure for making a valuation model:
![image](https://github.com/LinhNguyen-MyLi/Financial-Modeling-in-Excel/assets/128978862/4e1392f3-1e3f-4f28-82b4-cb0183375919)

1. Determine the range for historical data. General, if we want to forecast for the next 05 years then we should need 10 years of historical data. However, please notice that it depends on the nature of the financial company, for example: if the firm has just changed its accounting standard then you definitely shouldn't choose the data before that changing point. Last but not least, don't try to make a forecasted time period that is too long cause it will lose its accuracy.
   
2. Download and smooth the data
   
3. As the first step stated, we will start with the revenue and COGS. So how we can know whether the revenue will go up or go down? The key feature is the market/industry. In general, a performance of a company will follow the trend of the market and we can use the forecasted growth rate from the trustworthiness organization and companies like Bloomberg, Eurostat... However, I recommend that you should analyze and break down all products and services of the core business of the company. From that, we will list down all the key factors that affect the output product price. For example, if a company produces tires then we should care about the price of natural rubber, chemicals, petroleum, and oil. You might be surprised but the tire industry uses a huge amount of petroleum and oil cause those are key materials to make synthetic rubber - one of the main kinds of rubber for producing tires. In my case, the company is working specializes in manufacturing, transporting, and distributing Compressed Natural Gas (CNG Vietnam Joint Stock Company) -> their output product is affected by Crude oil prices, Coal prices, and foreign exchange rates. The company has 1 main factory (Phu My factory) and other factories and the inputs above will go through those factories to be compressed, the final product is CNG. When mentioning the factory we take into account the capability of each factory. Based on that we calculate the product price and make some adjustments for direct costs related to employees, machinery, outsourcing cost...
               
4. For SG&A, normally I will forecast based on the position of the company in its life cycle. Ex: for companies in the mature stage -> their SG&A is stable.
   
5. For PPE (an important feature), we always forecast it with Capex (money a company uses to purchase, maintain, or expand fixed assets) and the disposal plan of the company. Why?. Because we need to eliminate accumulated depreciation as well as its recognition of all disposal assets, record income cash flow from disposal assets, added and start to depreciate new assets. Please note that PPE is a very important feature cause it affects all 03 parts of the financial statement. We don't depreciate land. Last but not least, if your company has a plan for revaluing PPE in the future, you need to stop depreciating it and use its fair value as a new carrying amount for the remaining time.
   
6. For Debt, break down each of the debts. As for existing debt, try to determine the amount of remaining interest in the debts, whether those debts are frequent and tend to repeat after a period of time, what bank your company purchases the debt from, the volatility of the offered interest rate (you will estimate the future interest rate by adding the volatility to the current interest rate). Note: to estimate interest rate you could use the GARCH model (the use of GARCH model for forecasting has been mentioned in my other repo)
   
7. Tackle the "loop" problem when we forecast. What is the "loop"? -> You can see this via the graph below.
   ![image](https://github.com/LinhNguyen-MyLi/Financial-Modeling-in-Excel/assets/128978862/be952c11-61d1-44ed-b71d-e32c3928dc7c)
So to solve this loop in Excel we go to the Option function -> click Formulas -> stick the "Enable iterative calculation"

8. DCF
   ![image](https://github.com/LinhNguyen-MyLi/Financial-Modeling-in-Excel/assets/128978862/d53f7588-0ff0-4f4a-8dd2-6c2b3a9396f9)
We can easily find FCFF and FCFE based on the already calculated (forecasted) data. The discounted factors will be present following

9. For Ke (cost of equity), the formula: Ke = risk-free rate + beta * (equity risk premium)
- The risk-free rate can be LIBOR, SOFR, or the government bond, treasury. (1)
- The beta calculation: (2)
   + Collect quotations daily, weekly, and quarterly of your stock price and the index (in my case is VN-Index) (time length is the same as historical data), and use the closing price.
   + Run logarithm function for the stock price and index to find daily, weekly, and quarterly returns - ln(stock price/index)
   + Run regression function for daily, weekly, and quarterly returns with the independent variable (explanatory variable) as the stock price (x-axis) and dependent variable as the index (y-axis).
   + The result of a regression will be looked like this (the yellow is our beta):
     
     ![image](https://github.com/LinhNguyen-MyLi/Financial-Modeling-in-Excel/assets/128978862/0264e159-a4c7-41d6-87ca-7f50ea9bc991)

     We have 03 beta now (daily, weekly, quarterly) -> pick the highest beta cause it means a stock has a similar level of volatility or systematic risk as the overall market. However, this beta is beta L (beta leveraged) -> we should transfer to beta U (beta unleveraged) -> beta U = beta L / (1+ D/E * (1-tax rate)). At this stage, we don't use the D/E of the firm -> use D/E industry = Total debt industry/Total Equity industry => we obtain beta U industry
     With the beta U industry, we calculate beta L for the company => beta L for the company = beta U * (1+ D/E * (1-tax rate)). Using D/E of the company for this calculation.
     
- The equity risk premium (ERP)(apply Damodaran method), we will find a coefficient correlation between the standard deviation of the index and the standard deviation of the S&P500, this correlation will be used as an adjustment number.
  ERP VN = ERP US * (correlation) (A)
  ERP VN = ERP US + CRP VN (B)
  => Average (A) & (B) is ERP (3)

From (1) (2) (3) => Ke

11. When we have Ke, it's easy to find WACC. WACC = Ke*We + Kd*Wd*(1-tax rate)
12. After calculating FCFE we divide it by the number of shares outstanding to obtain equity value per share. As for FCFF, we need to minus the value of debts before dividing it by the number of shares outstanding to obtain equity value per share.
