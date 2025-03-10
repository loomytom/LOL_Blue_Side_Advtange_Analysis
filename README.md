
# League of Legends Red vs Blue Side Analysis

Welcome!

This project is a project conducted at UCSD with multiple sets of analyses including hypothesis testing, missingness testing, and modeling. The main objective of this project is to determine whether or not there is a significant difference between win rates of teams playing from Blue Side and playing from Red Side.

## Project Objectives

- Examine the differences between red and blue side performance.
- Use t-tests and statistical methods for analysis.
- Provide insights into game balance.
[View the Repository on GitHub](https://github.com/loomytom/LOL_Blue_Side_Advantage_Analysis)

## Introduction 

League of Legends (LOL) is a globally popular multiplayer online battle arena (MOBA) game developed by Riot Games. LOL has established itself as one of the most prominent titles in competitive gaming with millions of players worldwide. The data for this project is sourced from Oracle's Elixir, a professional-grade data platform that compiles gameplay statistics from professional matches. This dataset pulls match records from professional play, offering a rich array of features such as kills, deaths, gold differences at 10 minutes, barons, dragons, and more.

One of the most intriguing dynamics in League of Legends is the perceived advantage or disadvantage tied to which side of the map a team starts on — Red Side or Blue Side. This perceived asymmetry has sparked debates among both casual and professional players  as they believe there may be a meaningful difference in win rates between the two sides. Riot Games has also acknowledged this potential issue, making it a relevant area of study.

The focus of this analysis is to investigate whether these differences in win rates are statistically significant. By using professional match data, this project aims to find whether Red Side or Blue Side holds an advantage, and if so, what metrics might drive this disparity. For instance, does Blue Side tend to secure more barons? Is Red Side more likely to take the first dragon? To further explore these patterns, a predictive model was developed to determine a team’s side based on key gameplay metrics. This model provides valuable insights into potential biases associated with playing on each side and can inform both in-game decision-making and future balancing efforts by Riot Games. 

# Data 

The data that was used for this analysis was pulled from the professional matches in 2022. In the data there are 150180 rows and 161 columns. Out of these columns, the ones that are important to this analysis are listed below.

- <span style="background-color:#f0f8ff; border-radius:8px; padding:4px 8px; font-weight:bold; display:inline-block; box-shadow:0px 2px 4px rgba(0, 0, 0, 0.1);">side</span>: This column represents what the team affiliation for each game, and it is either Red or Blue. 
- <span style="background-color:#f0f8ff; border-radius:8px; padding:4px 8px; font-weight:bold; display:inline-block; box-shadow:0px 2px 4px rgba(0, 0, 0, 0.1);">result</span>: This column is a binary indication of the outcome of the match, 1 for a win and 0 for a loss.
- <span style="background-color:#f0f8ff; border-radius:8px; padding:4px 8px; font-weight:bold; display:inline-block; box-shadow:0px 2px 4px rgba(0, 0, 0, 0.1);">firstdragon</span>: This column represents whether or not a team was the first team to secure a dragon. 1 means they got the first dragon and 0 means they did not.
- <span style="background-color:#f0f8ff; border-radius:8px; padding:4px 8px; font-weight:bold; display:inline-block; box-shadow:0px 2px 4px rgba(0, 0, 0, 0.1);">heralds</span>: The 'heralds' column denotes how many rift heralds, another epic monster, each team takes.
- <span style="background-color:#f0f8ff; border-radius:8px; padding:4px 8px; font-weight:bold; display:inline-block; box-shadow:0px 2px 4px rgba(0, 0, 0, 0.1);">barons</span>: The ‘barons’ column records how many barons, a third type of epic monster, each team takes.
- <span style="background-color:#f0f8ff; border-radius:8px; padding:4px 8px; font-weight:bold; display:inline-block; box-shadow:0px 2px 4px rgba(0, 0, 0, 0.1);">golddiffat10</span>: This column represents how much gold a team is up by 10 minutes into the game. A positive value tells us that they are leading the other team by a certain amount, and a negative value means they are behind by that amount.
- <span style="background-color:#f0f8ff; border-radius:8px; padding:4px 8px; font-weight:bold; display:inline-block; box-shadow:0px 2px 4px rgba(0, 0, 0, 0.1);">golddiffat20</span>: Similar to ‘golddiffat15’, this column specifically denotes the gold differential of a team at 20 minutes.
- <span style="background-color:#f0f8ff; border-radius:8px; padding:4px 8px; font-weight:bold; display:inline-block; box-shadow:0px 2px 4px rgba(0, 0, 0, 0.1);">golddiffat25</span>: Similar to ‘golddiffat20’, this column specifically denotes the gold differential of a team at 25 minutes. A null value in the golddiffat25 suggests that the game ended before 25 minutes.

# Data Cleaning 

### 1. Filtering for Complete Data
The first step towards cleaning the data was to only grab data that is considered 'complete', which was under the column 'datacompleteness' in the original dataframe. The column was divided into 'complete' and 'partial' so the data was queried to only use rows that had complete data. 

### 2. Aggregating Team-Level Data
The next step was to query out each player's performance and only grab information regarding the team for each game. This was done by looking at the 'position' column in the dataframe and setting it to only grab position = 'team'.

### 3. Selecting Relevant Columns
The last step towards cleaning the data was to query only the relevant columns that were listed above. This is what the cleaned Dataframe looks like:

| side   |   result |   firstdragon |   heralds |   dragons |   barons |   golddiffat10 |   golddiffat20 |   golddiffat25 |
|:-------|---------:|--------------:|----------:|----------:|---------:|---------------:|---------------:|---------------:|
| Blue   |        0 |             0 |         2 |         1 |        0 |           1523 |           -944 |             88 |
| Red    |        1 |             1 |         0 |         3 |        0 |          -1523 |            944 |            -88 |
| Blue   |        0 |             0 |         1 |         1 |        0 |          -1619 |          -5140 |          -7280 |
| Red    |        1 |             1 |         1 |         4 |        2 |           1619 |           5140 |           7280 |
| Blue   |        1 |             1 |         1 |         4 |        1 |           -103 |           1744 |           4145 |

# Univariate Analysis 
The first analysis that was performed was looking at overall wins on blue side vs red side 
<iframe
  src="assets/win_rate_sides.html"
  width="700"
  height="500"
  frameborder="0"
></iframe>

This pie chart shows that there is a 4.6 point difference between win rates of teams on blue side to red side. From the graph, it shows that teams playing on blue side win more than teams playing on red side. 

# Bivariate Analysis 
The second analysis that was done looked at how many rift heralds each team took when they won and when they lost
<iframe
  src="assets/heralds_sides.html"
  width="700"
  height="500"
  frameborder="0"
></iframe>

This bar chart shows that blue side teams take more rift heralds when they win, but they also take more rift heralds when they lose compared to red side. No matter win or lose, on average blue side will take more rift heralds than red side.

# Interesting Aggregates
These were some of the interesting aggregates that were found when grouping by result, either win or loss, and summing the columns. 

|   result |   firstdragon |   heralds |   dragons |   barons |
|---------:|--------------:|----------:|----------:|---------:|
|        0 |          4491 |      8146 |     15394 |     2372 |
|        1 |          6129 |     12817 |     32334 |    11971 |

From this above we can see that winning teams take more dragons, heralds, and barons, but also more likely to take the first dragon compared to teams that lost. This makes sense because taking objectives helps teams get ahead in the game, and contributing to them being stronger than the enemy team.

# NMAR Analysis 
In looking at the columns of interest, the column **firstdragon**, could be NMAR because the value of **firstdragon** depends on itself. For exmaple, if **firstdragon** is missing, it is possible that the team did not take the first dragon due to their strategy or simply did not try and contest the dragon. In this case the missingness of **firstdragon** is dependent on the value of the column, which would make it NMAR.

# Missingness Dependency 
To test for missingnes, the column **golddiffat25** was tested to see if its missingness was dependent on any columns. The significance level that was chosen for these tests was a = 0.05 and the test statistic was Kolmogorov–Smirnov.

The first column tested with **golddiffat25** was the column **baron**.

**Null Hypothesis**: The distribution of **barons** is the same when **golddiffat25** is missing then when it is not missing.

**Alternative Hypothesis**: The distribution of **barons** is different when **golddiffat25** is missing than when it is not missing.

Below is a graph showing what the distribution of values look like:
<iframe
  src="assets/baron_missing.html"
  width="700"
  height="500"
  frameborder="0"
></iframe>

From the graph, it can be seen that observed difference is much larger than any of the tests. The p-value was **0.0**, lower than the 0.05 threshold, showing that it is statistically significant, rejecting the null, and the missingness of **golddiffat5** is related to the **barons** column. This makes sense because if there are a lot of baron's taken, the game likely ran longer than 25 minutes because the first baron is spawned in at 20 minutes. 

The second column tested with **golddiffat25** was the column **firstdragon**.

**Null Hypothesis**: The distribution of **firstdragon** is the same when **golddiffat25** is missing then when it is not missing.

**Alternative Hypothesis**: The distribution of **firstdragons** is different when **golddiffat25** is missing than when it is not missing.

Below is a graph showing what the distribution of values look like:
<iframe
  src="assets/first_dragon_missing.html"
  width="700"
  height="500"
  frameborder="0"
></iframe>

From the graph, it can be seen that observed difference is smaller than most of the permuted tests. The p-value was 0.928, above the 0.05 threshold, showing that it is not statistically significant, the null is not rejected, and the missingness of **golddiffat5** is not related to the **firstdragon** column. 

# Hypothesis Testing

For the hypothesis test, the goal was to find out if there is a significant difference between win rates on blue side and win rates on red side. This is important to understanding possible biases in League of Legends and being able to address them to ensure fairness in gameplay. 

**Null Hypothesis**: The win rate of teams on Blue side is the same as the win rate of teams on Red side.

**Alternative Hypothesis**: The win rate of teams on Blue side is greater than the win rate of teams on Red side.

**Test Statistic**: Difference in mean

**Significance Level**: 0.05

**Result P-Value**: 0.00

**Conclusion**: From the hypothesis test, the resulting p-value was **0.00** which means that on a 0.05 level of significance, the difference between win rate on blue side and win rate on red side is statistically significant, and the null is rejected. In other words, the difference seen in the univariate analysis is likely not due to chance but that there are some other factors that are pushing the win rate of blue side teams to be higher than red side teams. These factors may include how the map is set up, or other columns like rift heralds taken, or frist dragon acquired.

# Prediction Problem 

From the hypothesis test, it was learned that the win rate differences between Blue side and Red side are likely not due to random chance but that there are factors that are affecting the win rate. These factors will be further analyzed in this section where a prediction model is built to predict what side a team is playing from depending on features like first dragon, heralds taken, barons taken, total dragons, and gold differential at 10 minutes. 

This model can be built using classification algorithm techniques to answer the question, **Can team side be predicted based on other features of the game?** 

For the data itself, it happened to work out that the data was already set, where binary columns where already encoded to either be a 1 or a 0 and the other columns could be directly used. The dataset was split into 75% training data and 25% test data, and the final results were evaluated using accuracy score and comparing the training and test sets. 

The columns that were used for the prediction model are shown below:

| Side  | Result | First Dragon | Heralds | Dragons | Barons | GoldDiff10 | GoldDiff25 |
|-------|--------|--------------|---------|---------|--------|------------|------------|
| Blue  | 0      | 0            | 2       | 1       | 0      | 1523       | 88         |
| Red   | 1      | 1            | 0       | 3       | 0      | -1523      | -88        |
| Blue  | 0      | 0            | 1       | 1       | 0      | -1619      | -7280      |
| Red   | 1      | 1            | 1       | 4       | 2      | 1619       | 7280       |
| Blue  | 1      | 1            | 1       | 1       | 4      | -103       | 4145       |

# Baseline Model

For the baseline model, the columns **Heralds** and **Dragons** were fit using a DecisionTreeClassifier. Both of these columns are quantitative, so they were passed through a StandardScalar() transformer which standardizes the data to have a mean of 0 and a standard deviation of 1. For the DecisionTreeClassifier, I had played around with several different values of tree depth and I ended up using a max depth of 10 as I found it provided the highest training and test accuracy scores. 

Looking at the performance of the model, the training set had an accuracy of **0.612** and the test set had an accuracy of **0.601**. Overall, this was not a bad performance given the fact that Red Side and Blue Side are normally 50/50 metrics. Being able to get above a 10% difference from the normal was encouraging to see, and it meant that these 2 features that were being passed into the model were in fact somewhat useful in determining what side a team played from. However, there is still a lot of room to improve from this model as the accuracy is not extremely high, which means it still has a good chance of being incorrect for any given data. 

# Final Model

For the final model, 2 more features were added which was **golddiffat10** and **firstdragon**. These columns were chosen to be added to the model because they are important in describing some of the hallmarks of blue side vs red side. On the map, the rift herald pit and the dragon pit is placed on opposite sides of the map, with Red side having direct access to dragon pit and blue side having direct access to herald pit. In order for red side to either take rift herald or blue side to take dragon, they would need to go around a wall to reach the pit, making it more difficult. Because of this difference, it would make sense as to why Blue Side teams take more rift heralds more often, becuase it is an easier objective for them to secure. This is also important to explain why **golddiffat10** might also be an strong feature to implement. Rift Herald is a monster that when slain, on average grants teams 400-500 gold when used. This objective is taken before 20 minutes because once 20 minutes into the game, the rift herald becomes baron. Therefore if blue side takes more rift heralds on average, it is also likely that they will have a gold lead early in the game because of it.

The final model also uses RandomForestClassifier which is usually better than DecisionTreeClassifier because it is more robust against overfitting and is better at handling complex data. First Dragon is a column that is already in a binary form so it doesn't require any furthing encoding. For golddiffat10, a QuantileTransformer was used because it would help shape the distribution of gold differentials to prevent uneven or extreme shapes that may hurt the model. The hyperparameters that were chosen were maximum depth and number of estimators, and the intervals that were tested were 50-300 by 50 for the number of estimators, and 2-14 by 2 for the maximum depth. Ultimately, the GridSearch found that 50 estimators and a maximum depth of 6 performed the best for the model. The final model had a training accuracy of **0.641** and a test accuracy of **0.608**. Although the increase in accuracy between the base model and the final model is not very significant, the final model is more resistant to changes in the data that may cause the base model to overfit. Small increases in the model are also still important because when dealing with a variable that is 50/50 and balanced, it is often very difficult to make any differences in accuracy. 

# Fairness Analysis

The fairness analysis looks to see whether the model is fair for different groups. In the fairness analysis that was run, the question was **Does the model perform worse for teams that take the first dragon compared to teams that do not take the first dragon?**. A permutation test was used to test this and the null and alternative hypothesis are below.

**Null Hypothesis**: There is no difference between the model's accuracy for teams that have taken the first dragon from the teams that haven't

**Alternative Hypothesis**: The model is biased depending on whether or not the team has taken the first dragon or not. 

The test statistic that was used to test this hypothesis was a absoulute difference of means. The final result was **p-value = 0.0** which is statistically significant at the a = 0.05 level, rejecting the null. This means that the model is biased when predicting the team's side for teams that have taken the first dragon and the teams that haven't. 


