## **Predicting NBA Player Salary using Linear Regression and Web Scraping**

Prathap Rajaraman

**Abstract**

Because of my passion for basketball, I decided to focus my linear regression project on the NBA. Every free agency, we see bloated contracts handed out to underperforming players and value contracts handed out to budding stars that no one saw coming. These disparities between contract value and output can be the difference between championships and disappointment. The goal of this project is to predict NBA players' yearly salary using linear regression. With a data-driven approach to valuing a player, front offices can make more calculated decisions. They are better equipped to negotiate new contracts, trade for undervalued players, or trade away their own players they deem underperforming.



**Design**

To predict NBA salaries, the relationships between various box score statistics should be explored. The correlation between the stats and player salary, as well as each other for multicollinearity, should be considered. In addition, feature engineering will be implemented to account interaction variables and/or dummy indicators that aren't immediately available.



After data cleaning and manipulation, I split the data into training and test sets, and tested three different models (OLS, Ridge Regression, and Lasso Regression). I modeled based on the training set and judged the efficacy of the models on the test data. I also judged the models on r squared, Mean Absolue Error, and intuitive fit.



**Data**

I will be web scraping from the following sources:

- Player statistics from the 2017-2018 to 2020-2021 seasons taken from [Basketball-Reference](www.basketball-reference.com)
- NBA salary data taken from [ESPN](http://www.espn.com/nba/salaries)



**Algorithm**

*Data Cleaning*

- Players that did not have a corresponding salary match were dropped from the dataset
- Blank rows are removed. These are typically attributed to players that are technically on a roster but haven't received any or significant playing time.

*Data Manipulation*

- The following variables were collected and considered: player, position, age, team, games played/started, minutes, made and attempted Field Goals/Free Throws/3 Pointers, shooting percentages, offensive and defensive rebounds, assists, steals, blocks, turnovers, fouls, points, year, offensive/defensive rating, salary.

- This tool is meant to predict an NBA player's salary. Due to current salary cap and contract regulations, this analysis won't be appropriate for all players. A player's first deal, a rookie contract, is set in stone and superstar athletes are limited by maximum contract provisions. To account for this, I will be flagging these players. For simplicity, I am assuming that players on rookie contracts are under the age of 23 and max contract players are those whose salary is 25% of the cap or greater. Also, I will be filtering out players who played fewer than 10 games in a given season for credibility purposes



*Feagure Engineering*

Variables Added:

- "Stocks". This is steals + blocks, and is commonly used as an aggregate term in NBA Analytics circles to simplify defensive stats

- 3 and D interaction term. Versatility is in high demand, so I am including an interaction term of 3 pointers made multiplied by steals and blocks.

- [True Shooting Percentage](https://www.breakthroughbasketball.com/stats/tsp_calc.html) (TS%). True Shooting Percentage is commonly used in NBA as a metric to judge overall shooting. It takes into account field goals, three pointers, and free throws into one comprehensive number. The formula is Points / (2 * (FGA + 0.44 * FTA))

- Past peak indicator. Typically, players start to decline once they hit the wrong side of 35. I would expect age to be positively correlated with salary prior to age 35, to account for player improvement and reputation/basketball IQ, but be negatively correlated as players decline in skill and take on reduced roles and/or ring chase. This indicator helps address the nonlinear relationship


- Fouls per Minute. Fouls and Turnovers are both positively correlated with Salary, despite both being detrimental to team performance. The reason for this is because the best players are on the court longer, increasing the likelihood of them committing fouls and turnovers. TOV% mitigates this phenomenon for turnovers, and fouls per minute will for personal fouls.
- Turnover Rate (TOV%). Turnovers after being controlled for time spent on court. The formula is Turnovers / (2 * (FGA + 0.44 * FTA + Turnovers))
- Salary Scale: A salary from 2018 should not be treated the same as a salary from 2021 due to player contracts increasing over time. To account for this, salaries will be scaled up via the salary cap for that given year. For example, if the salary cap in 2018 is $100m and in 2021 is $110m, all player contracts in 2018 will have a 1.1x adjustment factor applied to it.
- Square root transformation of Salary. Because independent variables are small nominally (typically under 100) while Salary is in the millions, we risk explosiveness and volatility in the model. This also reduces heteroskedasticity.



*Variable Selection*

- After accounting for redundancy and obvious multicollinearity, I narrowed the selection to the following variables: year, age, past peak, field goal attempts, 3 pointers made, free throw attempts, offensive and defensive rebounds, assists, stocks, fouls per minute, points, offensive and defensive rating, 3 and D, TS%, TOV%. I performed Variance Inflation Factor analysis to narrow it down further.
- Variance inflation factor is used to calculate multicollinearity. There is no hard threshold for variable removal, but ideally we would like to keep the VIF below 15.
- I originally added offensive and defensive ratings because it could add color by providing a metric that goes beyond the box score. However, due to high multicollinearity and low overall correlation with salary, these are dropped.
- TS% has multicollinearity issues and has a surprisingly low correlation with salary. This will be removed.
- Field goal attempts has high multicollinearity and redundancy with points, so this will be removed.
- Turnovers are positively correlated with salary. While counterintuitive, this makes sense because better players get more playing time and run the offence more frequently, leading to more turnovers. In an attempt to control for this, I analyzed two commonly used metrics, assist-to-turnover ratio and turnover rate (or turnovers per 100 possessions estimate). Even after controlling for player skill and playing time, positive correlation still existed, suggesting that teams don’t typically punish turnovers. I kept TOV% out of the model for this reason.
- Although I experienced multicollinearity issues with points, I kept it in due to the obvious significance of this stat. Excluding this may lose credibility with potential stakeholders. This same logic applies to year and age.
- Despite controlling for salary cap changes, there is still a correlation between year and salary. Year has a slight negative relationship to salary, even after scaling for the increase in Salary Cap. The contracts are set in stone, with marginal raises, compared with the salary cap which has been increasing at a higher rate.
- The following variables will be used for regression analysis:
  - Year
  - Age
  - Past Peak
  - 3 Pointers made
  - Free Throw Attempts
  - Offensive Rebounds
  - Defensive Rebounds
  - Assists
  - Stocks
  - Fouls per Minute
  - Points
  - 3 and D
- As mentioned previously, the square root of salary scaled to 2021 salary cap will be the dependent variable
- In total, there are 1381 records of players from 2018-2021 that is being used for this analysis



**Results/Analysis**

Cross validation with the training data was conducted and the r squared, mean absolute error, and intuitive fit was assessed. I compared three models that use the same underlying variables: ordinary least squares, ridge regression, and lasso regression. Ultimately, I went with the ridge regression.

- The OLS had an average train r^2 score of 0.380, a test r^2 for 0.398, and a MAE of 809.77, or $655,731 in salary terms.
- The Ridge Regression had an average train r^2 score of 0.380, a test r^2 for 0.396, and a MAE of 816.9, or $667,317 in salary terms.
- The Lasso Regression had an average train r^2 score of 0.380, a test r^2 for 0.400, and a MAE of 813.68, or $662,069 in salary terms.

Although the OLS had slightly better results, the ridge regression was the best fit. OLS gave a negative coefficient for 3 pointers made, which is unrealistic given the benefits of this stat, and overestimated the fouls variable. On the other hand, Lasso removed the 3 pointers made, presumably due to it's close link to both 3 and D and points whereas Ridge tapered its impact. Because I believe 3 pointers are significant enough to keep and the difference in r^2 and MAE is very marginal, I went with Ridge. In addition, the difference between test and train r^2 suggests that there isn't significant overfitting and the MAE is relatively small, providing confidence in this model.

Below is a chart that outlines the weights given to each variable. Although age has the strongest link to salary, it is partially offset by the past peak indicator. Points is the second most valuable feature, but is not the only scoring feature that has a positive impact.

![coefficients](/Users/prathaprajaraman/Documents/Data_Science/Metis/Linear_Regression/project/coefficients.png)

**Tools**

- Pandas for data manipulation
- Beautifulsoup for web scraping
- Numpy, Scikit-learn, and statsmodels for statistical analysis
- Matplotlib and Seaborn for visualization



**Communication**

A brief deck on my exploration and analysis, along with visualizations.

