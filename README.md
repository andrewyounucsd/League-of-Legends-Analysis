# League of Legends Atakhan Relevance Analysis
Conducted by: Andrew Youn

## Introduction
### League of Legends
League of Legends (LoL) is one of the most popular Massive Online Battle Arena (MOBA) games in history. Released on October 27. 2009 by Riot Games, LoL quickly took the world by storm and amassed nearly 30-40 million active players daily as of 2025. Beyond casual play, the game has also established a massive professional esports scene, with regional leagues and international tournaments watched by millions.

In League of Legends, two teams of five players compete on a map with the primary objective of destroying the opposing team’s Nexus, a central structure located within their base. To reach the Nexus, teams must strategically work together to destroy defensive structures, secure neutral objectives, and gain advantages in gold, experience, and map control. Matches require a combination of mechanical skill, teamwork, and strategic decision-making, making LoL a rich environment for data-driven analysis.

This project focuses on analyzing professional League of Legends match data, provided by Oracle's Elixir, to better understand how in-game objectives—specifically Atakhan—relate to team success and win outcomes. Despite its popularity, Atakhan is scheduled to be removed in an upcoming League of Legends patch as part of Riot Games’ ongoing efforts to keep the game fresh and prevent gameplay from becoming stale. Major game updates and seasonal changes are a core part of League of Legends’ design philosophy, even if it means retiring impactful in-game objectives.

Atakhan is an epic monster in the game that is also known as an "objective" which is not necessary to win the game but helps immensely by giving many benefits to the team that slays it. Given Atakhan’s impending removal, this project seeks to better understand its role in professional play while it is still present in the game. Specifically, this analysis aims to answer the following research question:

How likely are professional League of Legends teams to win a match if they secure Atakhan?

By examining match-level data from professional games, this study explores whether securing Atakhan is meaningfully associated with higher win rates and how predictive this objective is of match outcomes.

### Oracle's Elixir Dataset
The dataset provided by Oracle's Elixir focuses on professional League of Legends matches that were played during 2025. There are 188,932 rows and 164 columns, but we will only be taking a look at a couple of columns in this dataset to conduct our analysis.

- `gameid`: Represents an index to represent each match that was played and used to differentiate different matches in the dataset.
  
- `datacompleteness`: Represents the completeness status of professional matches that were played.
  
- `league`: Represents the tournament in which the professional match took place.
  
- `position`: Represents what role the individual players played in each game. Either "top", "jng", "mid", "bot", "sup", or "team" to represent the entire team.
  
- `side`: Represents which team in the game the players were on, either the blue team or the red team.
  
- `result`: Represents which team won as a boolean. 0 Means a team lost, 1 means a team won.
  
- `teamkills`: Represents the total amount of a kills each team ended the match with.
  
- `atakhans`: Represents if the team secured atakhan as a boolean. 0 Means they didn't secure Atakhan, 1 means they did secure Atakhan.
  
- `opp_atakhans`: Represents if the enemy team secured atakhan as a boolean. 0 Means they didn't secure Atakhan, 1 means they did secure Atakhan"
  
- `totalgold`: Represents the total amount of money that was generated in a match. This column is very important as gold is the main way players in League of Legends become stronger. Generally, the more gold a team has, the stronger they are. Individual players will have their total gold amounts and the team row will have the entire gold generated from the team summed up.
  
- `goldat20`: Represents the total amount of money that was generated in a match 20 minutes into the game. The reason this column is so relevant to our analysis is because the objective Atakhans spawns into the game at 20 minutes, meaning this statistic is very important to consider. The rows are repsented identically to that of totalgold.
  
- `opp_goldat20`: Represents the total amount of money that was generated in a match by the enemy team 20 minutes into the game. Identically important to our analysis like the previous column. The rows are repsented identically to that of totalgold.
  
- `xpdiffat20`: Represents the difference in total Experience in a match at 20 minutes. Experience (xp) is another means of getting stronger in League of Legends. By having more experience levels, the characters in the game are able to get stronger spells, more health points, and many other benefits. A positive integer means the player has more experience and a negative integer means the player has less experience than their counterpart position.

- `killsat20`: Represents the amount of kills each player/team had 20 minutes into the match.



## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
As stated previously, only 14 columns are being kept as they are the only ones that are relevant to the analysis. In this analysis we will be focusing on games in which the matches were completed, meaning we will get rid of all the rows where the games did not finish which was done by making sure all values in the column `datacompleteness` were complete. 

| gameid           | datacompleteness   | league   | position   | side   |   result |   teamkills |   atakhans |   opp_atakhans |   totalgold |   goldat20 |   opp_goldat20 |   xpdiffat20 |   killsat20 |
|:-----------------|:-------------------|:---------|:-----------|:-------|---------:|------------:|-----------:|---------------:|------------:|-----------:|---------------:|-------------:|------------:|
| LOLTMNT03_179647 | complete           | LFL2     | top        | Blue   |        0 |           3 |        nan |            nan |       10668 |       6473 |           7012 |         -490 |           1 |
| LOLTMNT03_179647 | complete           | LFL2     | jng        | Blue   |        0 |           3 |        nan |            nan |        7429 |       5668 |           7357 |        -1339 |           0 |
| LOLTMNT03_179647 | complete           | LFL2     | mid        | Blue   |        0 |           3 |        nan |            nan |        9032 |       6332 |           7990 |        -1439 |           0 |
| LOLTMNT03_179647 | complete           | LFL2     | bot        | Blue   |        0 |           3 |        nan |            nan |        9407 |       6782 |           8489 |        -1248 |           1 |
| LOLTMNT03_179647 | complete           | LFL2     | sup        | Blue   |        0 |           3 |        nan |            nan |        5719 |       4220 |           4530 |          314 |           0 |

You will see that the both the columns containing atakhan are empty, however this isn't a problem because these columns are only represented in team rows. We will be mainly taking a look at the team-rows represented in the `position` columns as "team", however for now, in order to do our univariate and bivariate analysis, we must keep all the rows.  

### Univariate Analysis
<iframe
  src="assets/goldat20_hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The distribution of total gold at 20 minutes exhibits two clearly distinct clusters, corresponding to individual player-level rows and team-level rows. Individual gold values are concentrated between approximately 4,000 and 10,000 gold, while team-level gold values range from roughly 30,000 to 45,000 gold. These two distributions reflect fundamentally different units of analysis and therefore should not be treated as a single population.

Neither distribution appears to be normally distributed. Both exhibit right skewness and natural lower bounds, violating the symmetry and unboundedness assumptions of a normal distribution. As a result, summary statistics and modeling approaches should account for this non-normality and the mixed granularity of the data.

### Bivariate Analysis
<iframe
  src="assets/atakhan_wl.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The plot indicates that teams securing Atakhan have a substantially higher win rate than loss rate, with nearly four out of five games resulting in a win. While this does not establish causation, it suggests that securing Atakhan is strongly associated with match success in professional play.

### Interesting Aggregates

|   atakhans |   teamkills |   totalgold |   goldat20 |   opp_goldat20 |   xpdiffat20 |   killsat20 |
|-----------:|------------:|------------:|-----------:|---------------:|-------------:|------------:|
|          0 |     12.5073 |     55779.7 |    33680.8 |        35671.6 |     -1575.77 |     6.08912 |
|          1 |     20.5682 |     63295.7 |    35672.1 |        33566.4 |      1666.68 |     8.33663 |

This table compares average team-level statistics for matches where Atakhan was not secured (0) versus secured (1). Teams that secured Atakhan tend to have higher team kills, greater total and mid-game gold, and a positive experience difference at 20 minutes, suggesting that Atakhan is associated with stronger early and overall in-game performance. As atakhan spawns at 20 minutes, we can also infer that teams that secure atakhan tend to have more gold and experience, meaning that they were already stronger than their opponents before atakhan was secured.

## Assessment of Missingness
### NMAR Analysis

There are several columns in this dataset that contain missing values, including `playername`, `playerid`, `champion`, and draft-related columns such as `pick1`, `pick2`, and `pick3`. However, this missingness is structural rather than NMAR. These columns are undefined for certain rows by design: team-level rows do not correspond to individual players and therefore cannot have player-specific attributes, while player-level rows do not contain team draft order information.

Similarly, many objective-related columns (such as `firstdragon`, `dragons`, and `void_grub`) are team-level statistics and are therefore structurally missing for individual players. In these cases, the missingness does not depend on the unobserved value itself, but rather on the role of the row (team vs. player).

It is important to distinguish this from NMAR (Not Missing At Random) missingness, where the probability that a value is missing depends on the value itself. Based on the available data, there is no strong evidence that any column in this dataset is truly NMAR. Instead, most missingness can be explained by observed variables such as row type (team vs. player) or league, making it either structural or MAR.

For example, the missingness of atakhans is largely structural, as Atakhan is a team-level objective and is undefined for player-level rows. However, even within this constraint, we do not find evidence that missingness depends on league or side, which supports the idea that the missingness is largely structural.

### Missingness Dependency

The `atakhans` column contains missing values primarily because Atakhan is a team-level objective, and therefore undefined for player-level rows. This suggests the missingness is structural rather than NMAR.

To verify whether missingness depends on other observed variables, we conducted permutation tests using the variance of missingness rates across groups as the test statistic. Specifically, we tested whether the missingness of `atakhans` depends on league or side.

First, we will show you the Missingness vs. League and then the Missingness vs. Side

Null Hypothesis: The missingness of the `atakhans` column is independent of league. Any observed differences in missingness rates across leagues are due to random chance.

Alternative Hypothesis: The missingness of the `atakhans` column depends on league, meaning some leagues have systematically higher or lower missingness rates than others.

| league      |   atakhan_missing = False |   atakhan_missing = True |
|:------------|--------------------------:|-------------------------:|
| AL          |                0.0323114  |               0.0322006  |
| ASI         |                0.00463167 |               0.00461579 |
| Asia Master |                0.01985    |               0.019782   |
| CD          |                0.0328628  |               0.0331458  |
| CT          |                0.0055139  |               0.00549499 |
| EBL         |                0.0210631  |               0.0209909  |
| EM          |                0.0485223  |               0.0483559  |
| EWC         |                0.00363917 |               0.00362669 |
| FST         |                0.00385973 |               0.00384649 |
| HC          |                0.0220556  |               0.02198    |
| HLL         |                0.0242611  |               0.0243098  |
| HM          |                0.0209528  |               0.020881   |
| HW          |                0.0125717  |               0.0125286  |
| IC          |                0.00187472 |               0.0018683  |
| LAS         |                0.0539259  |               0.053741   |
| LCK         |                0.061094   |               0.0610164  |
| LCKC        |                0.0594398  |               0.0596316  |
| LCP         |                0.0320909  |               0.0319808  |
| LEC         |                0.033745   |               0.0336293  |
| LFL         |                0.0348478  |               0.0349921  |
| LFL2        |                0.0256948  |               0.0256066  |
| LIT         |                0.0149978  |               0.015342   |
| LJL         |                0.0409131  |               0.0411685  |
| LPLOL       |                0.0176445  |               0.017584   |
| LRN         |                0.016652   |               0.0165949  |
| LRS         |                0.0153286  |               0.0152761  |
| LTA         |                0.00441112 |               0.00439599 |
| LTA N       |                0.0234892  |               0.0234087  |
| LTA S       |                0.0243714  |               0.0244197  |
| LVP SL      |                0.0286723  |               0.0291015  |
| MSI         |                0.00882223 |               0.00879198 |
| NACL        |                0.0304367  |               0.0303323  |
| NEXO        |                0.016652   |               0.0169905  |
| NLC         |                0.0273489  |               0.027387   |
| PCS         |                0.0334142  |               0.0332996  |
| PRM         |                0.0334142  |               0.0332996  |
| PRMP        |                0.00805029 |               0.00802268 |
| RL          |                0.0264667  |               0.0263759  |
| ROL         |                0.0229378  |               0.0228592  |
| TCL         |                0.0263564  |               0.0263979  |
| VCS         |                0.0155492  |               0.0154959  |
| WLDs        |                0.00926334 |               0.00923158 |

We used the variance of missingness rates across leagues as the test statistic, since larger variance would indicate league-specific missingness patterns.

The observed test statistic was 7.61 × 10⁻⁷, with a p-value of 1.0. The permutation distribution below shows that the observed statistic is well within the range expected under the null hypothesis.
<iframe
  src="assets/league_missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Because the p-value is much larger than a typical significance level (e.g. 0.05), we fail to reject the null hypothesis. This indicates there is no evidence that missingness in `atakhans` depends on league. Instead, missingness appears consistent across leagues, supporting the interpretation that it is structural.

Next, we examined whether the missingness of the `atakhans` column depends on a team’s side (Blue or Red). As before, we conducted a permutation test using the variance of missingness rates across groups as the test statistic. Larger variance would indicate side-specific missingness patterns.

Null Hypothesis: The missingness of the `atakhans` column is independent of side. Any observed differences in missingness rates between Blue side and Red side are due to random chance.

Alternative Hypothesis: The missingness of the `atakhans` column depends on side, meaning one side (Blue or Red) has systematically higher or lower missingness than the other.

| side   |   atakhan_missing = False |   atakhan_missing = True |
|:-------|--------------------------:|-------------------------:|
| Blue   |                       0.5 |                      0.5 |
| Red    |                       0.5 |                      0.5 |

The observed test statistic for this permutation test was 0, and the resulting p-value was 1.0. The permutation distribution shown below demonstrates that the observed statistic lies well within the range expected under the null hypothesis.
<iframe
  src="assets/side_missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Because the p-value is much larger than a standard significance level (e.g., 0.05), we fail to reject the null hypothesis. This indicates there is no evidence that the missingness of `atakhans` depends on side. In other words, missingness appears evenly distributed between Blue and Red sides, further supporting the conclusion that the missingness is structural rather than driven by gameplay factors.

## Hypothesis Testing

To investigate whether securing Atakhan is associated with match outcomes in professional League of Legends games, we conducted a permutation test comparing win rates between teams that secured Atakhan and teams that did not.

Null Hypothesis: The probability of winning a match is the same for teams that secure Atakhan and teams that do not secure Atakhan. Any observed difference in win rates is due to random chance

Alternative Hypothesis: The probability of winning a match differs between teams that secure Atakhan and teams that do not secure Atakhan.

We used the difference in win rates between the two groups (teams that secured Atakhan minus teams that did not) as the test statistic. This statistic is appropriate because our research question directly asks whether securing Atakhan is associated with a higher likelihood of winning, and win rate is a natural and interpretable measure of match success. We used a significance level of 0.05.

<iframe
  src="assets/hypothesis_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The observed difference in win rates between teams that secured Atakhan and teams that did not was 0.572, indicating that teams securing Atakhan won approximately 57 percentage points more often than teams that did not. Using a permutation test with the difference in win rates as the test statistic, we obtained a p-value of 0.0.

Because this p-value is well below the chosen significance level of 0.05, we reject the null hypothesis. This result provides strong evidence that the difference in win rates between the two groups is unlikely to be due to random chance alone. While this analysis does not establish a causal relationship, it suggests a strong association between securing Atakhan and a higher likelihood of winning in professional League of Legends matches.

## Framing a Prediction Problem

Our prediction task is a binary classification problem, where the goal is to predict whether a team wins or loses a professional League of Legends match. The response variable is `result`, with a value of 1 representing a win and 0 representing a loss. We chose this variable because match outcome is the clearest and most meaningful measure of success in League of Legends, and it directly connects to our main research question about what factors are associated with winning games.

At the time of prediction, we assume we only have access to in-game, team-level information available by 20 minutes, since Atakhan spawns at that point in the game. As a result, our model is trained using features such as side, whether Atakhan was secured, kills at 20 minutes, gold at 20 minutes, and experience difference at 20 minutes. All of these are values that would realistically be known before the game ends. We intentionally exclude any variables that rely on the final outcome of the match in order to avoid data leakage.

To evaluate our model, we use accuracy as the performance metric. Accuracy makes sense here because wins and losses are fairly balanced in the dataset, and there isn’t a strong reason to treat false positives or false negatives as more costly than the other. Although metrics like precision, recall, or F1-score could also be used, they are more useful in cases with class imbalance or when one type of error matters more. We compute accuracy on a held-out test set using a 70/30 train–test split, where 70% of the data is used for training and 30% is used for evaluation. Overall, accuracy provides a simple and interpretable way to measure how well our model performs on unseen data.


## Baseline Model

For my baseline model, I built a logistic regression classifier to predict whether a team wins or loses a professional League of Legends match. Since the response variable result is binary (1 = win, 0 = loss), logistic regression is a natural and interpretable starting point.

The model uses the following features, all of which would be known by 20 minutes into the game (the time when Atakhan spawns):

- Nominal Features (1):
    - `side` (blue or red side)
- Qualitative Features (4):
    - `atakhans` (whether the team secured it)
    - `killsat20` (number of kills at 20 minutes)
    - `goldat20` (total team gold at 20 minutes)
    - `xpdiffat20` (experience difference at 20 minutes)

There are no ordinal features in this model.

To prepare the data, I used a ColumnTransformer within a single sklearn Pipeline. The side feature was one-hot encoded using OneHotEncoder since it is categorical. The quantitative features were passed through a SimpleImputer with a median strategy to handle missing values. All preprocessing steps were fit only on the training data to avoid data leakage.

I split the data into training and testing sets using a 70/30 train–test split, stratified on the response variable to preserve the win/loss balance. I then trained the model on the training set and evaluated it on the held-out test set using accuracy as the evaluation metric.

The baseline model achieved an accuracy of approximately 82.5% on the test set. I believe this is a solid baseline result, especially given the simplicity of the model and the limited feature set. While the model is not perfect, it performs significantly better than random guessing and captures meaningful relationships between early-game team performance (gold, kills, experience) and match outcomes. This makes it a strong starting point for further feature engineering and model improvement in later steps.

## Final Model
