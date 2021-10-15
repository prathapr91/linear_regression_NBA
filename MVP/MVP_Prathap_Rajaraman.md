**Minimum Viable Product**



The goal of this project is to use linear regression to predict NBA player salaries based on their stats.



To obtain the statistics, I leveraged beautifulsoup to web scrape player statistics from 2018 - 2021 via [Basketball-Reference](www.basketball-reference.com) and player salary data in the same time frame from [ESPN](http://www.espn.com/nba/salaries). I chose this time frame not only for a greater quantity, but also for credibility reasons. The NBA had a one-time spike in salary cap in 2016, which would lead to an apples to oranges comparison if we go back too far.



Afterwards, I merged and cleaned the resulting dataframes into one single dataframe to be used for linear regression analysis. I made the following transformations so far:

- Dependent Variable: First, I took the salary and scaled it up to 2021 values. To do this, I scaled the salary based on the salary cap of the year (for example, if the 2018 salary cap is 100m and the 2021 salary cap is 110m, I multiplied all salaries in 2018 by 1.1). Because of the large value of salary relative to the independent variables (salaries are in the millions whereas dependent variables are typically below 100), I took the log of the salary. The final Y variable is the log of the salary expressed in 2021 dollars.
- Independent Variable: The following statistics are used for prediction: age, games played, minutes, field goal attempts, 3-Pointers made, effective field goal percentage, free throw attempts, free throw percentage, offensive/defensive rebounds, assists, steals, blocks, turnovers, personal fouls, points, year, offensive rating, defensive rating
- This tool is meant to predict an NBA player's salary. Due to current salary cap and contract regulations, this analysis won't be appropriate for all players. A player's first deal, a rookie contract, is set in stone and superstar athletes have a max contract provision. To account for this, I will be flagging these players. For simplicity, I am assuming that rookies are under the age of 23 and max contract players are those whose salary is 25% of the cap or greater.



An initial diagnostic plot of the regression can be shown below:

![mvp_plot](/Users/prathaprajaraman/Documents/Data_Science/Metis/Linear_Regression/project/mvp_plot.png)

I have the following commentary on my initial findings:

- The r-squared and adjusted r-squared is exceptional, at 0.994 each
- The overall p-value is close to 0
- There is strong multicollinearity, which is expected given the sheer volume of independent variables
- There are variables where the coefficients are suspect, presumably stemming from multicollinearity. For example, the coefficients for the following fields are negative: 3 pointers made, effective field goal percentage, free throw percentage, offensive rebounds, assists, and offensive rating. These are all fields that  unquestionably boost player value.



Next Steps:

- Conduct VIF testing to reduce variables in an effort to combat multicollinearity and create simplicity.
- Regularization and model testing to compare different models.

