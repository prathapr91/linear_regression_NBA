## **Predicting NBA Player Salary using Linear Regression and Web Scraping**

 

**Background**

Because of my passion for basketball, I decided to focus my linear regression project on the NBA. Every free agency, we see bloated contracts handed out to underperforming players and value contracts handed out to budding stars that no one saw coming. These disparities between contract value and output can be the difference between championships and disappointment. The goal of this project is to predict NBA players' yearly salary using linear regression. With a data-driven approach to valuing a player, front offices can make more calculated decisions. They are better equipped to negotiate new contracts, trade for undervalued players, or trade away their own players they deem underperforming.



 **Data Description**

- My target variable is player salary
- My independent variables are the following:
  - Age
  - Year of Season
  - Games Played
  - Minutes
  - Field Goal Attempts
  - Field Goal %
  - 3PT Attempts
  - 3PT %
  - Free Throw Attempts
  - Free Throw %
  - Offensive Rebounds
  - Defensive Rebounds
  - Assists
  - Steals
  - Blocks
  - Turnovers
  - Personal Fouls
  - Points
  - Offensive Rating
  - Defensive Rating
- I am envisioning a unit of my analysis to be the R-squared result of the regression



**Data Sources**

I will be web scraping from the following sources:

- Player statistics from the 2017-2018 to 2020-2021 seasons taken from [Basketball-Reference](www.basketball-reference.com)
- NBA salary data taken from [ESPN](http://www.espn.com/nba/salaries)

 

**Tools**

- Beautiful Soup for web scraping
- Pandas, Numpy, and Scikit-learn for data analysis, manipulation, and statistical analysis
- Matplotlib and Seaborn for data visualization



**Work Location**

- **LR_Deck.pdf**: High level summary of my analysis
- **LR_Writeup.md**: Detailed writeup of methodology and analysis
- **Coding notebooks**
  - **1_web_scrape.ipynb**: Web scraping off basketball-reference
  - **2_LR_mergepd.ipynb**: Cleaning and merging web scraped data
  - **3_LR_final.ipynb**: Regression analysis