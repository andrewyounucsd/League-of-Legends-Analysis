# League of Legends Atakhan Relevance Analysis
Conducted by: Andrew Youn

## Introduction
### League of Legends
League of Legends (LoL) is one of the most popular Massive Multipler Online (MMO) games in history. Released on October 27. 2009 by Riot Games, LoL quickly took the world by storm and amassed nearly 30-40 million active players daily as of 2025. Beyond casual play, the game has also established a massive professional esports scene, with regional leagues and international tournaments watched by millions.

In League of Legends, two teams of five players compete on a map with the primary objective of destroying the opposing team’s Nexus, a central structure located within their base. To reach the Nexus, teams must strategically work together to destroy defensive structures, secure neutral objectives, and gain advantages in gold, experience, and map control. Matches require a combination of mechanical skill, teamwork, and strategic decision-making, making LoL a rich environment for data-driven analysis.

This project focuses on analyzing professional League of Legends match data, provided by Orcale's Elixir, to better understand how in-game objectives—specifically Atakhan—relate to team success and win outcomes. Despite its popularity, Atakhan is scheduled to be removed in an upcoming League of Legends patch as part of Riot Games’ ongoing efforts to keep the game fresh and prevent gameplay from becoming stale. Major game updates and seasonal changes are a core part of League of Legends’ design philosophy, even if it means retiring impactful in-game objectives.

Atakhan is a epic monster in the game that is also known as an "objective" which is not necessary to win the game but helps immensely by giving many benefits to the team that slays it. Given Atakhan’s impending removal, this project seeks to better understand its role in professional play while it is still present in the game. Specifically, this analysis aims to answer the following research question:

How likely are professional League of Legends teams to win a match if they secure Atakhan?

By examining match-level data from professional games, this study explores whether securing Atakhan is meaningfully associated with higher win rates and how predictive this objective is of match outcomes.

### Oracle's Elixir Dataset
The dataset provided by Orcale's Elixir focuses on professional League of Legends matches that were played during 2025. There are 188,932 rows and 164 columns, but we will only be taking a look at a couple of columns in this dataset to conduct our analysis.

- 'gameid': Represents an index to represent each match that was played and used to differentiate different matches in the dataset.
  
- 'datacompleteness': Represents the completeness status of professional matches that were played.
  
- 'league': Represents the tournament in which the professional match took place.
  
- 'position': Represents what role the individual players played in each game. Either "top", "jng", "mid", "bot", "sup", or "team" to represent the entire team.
  
- 'side': Represents which team in the game the players were on, either the blue team or the red team.
  
- 'result': Represents which team won as a boolean. 0 Means a team lost, 1 means a team won.
  
- 'teamkills': Represents the total amount of a kills each team ended the match with.
  
- 'atakhans': Represents if the team secured atakhan as a boolean. 0 Means they didn't secure Atakhan, 1 means they did secure Atakhan.
  
- 'opp_atakhans': Represents if the enemy team secured atakhan as a boolean. 0 Means they didn't secure Atakhan, 1 means they did secure Atakhan"
  
- 'totalgold': Represents the total amount of money that was generated in a match. This column is very important as gold is the main way players in League of Legends become stronger. Generally, the more gold a team has, the stronger they are. Individual players will have their total gold amounts and the team row will have the entire gold generated from the team summed up.
  
- 'goldat20': Represents the total amount of money that was generated in a match 20 minutes into the game. The reason this column is so relevant to our analysis is because the objective Atakhans spawns into the game at 20 minutes, meaning this statistic is very important to consider. The rows are repsented identically to that of totalgold.
  
- 'opp_goldat20': Represents the total amount of money that was generated in a match by the enemy team 20 minutes into the game. Identically important to our analysis like the previous column. The rows are repsented identically to that of totalgold.
  
- 'xpdiffat20': Represents the difference in total Experience in a match at 20 minutes. Experience (xp) is another means of getting stronger in League of Legends. By having more experience levels, the characters in the game are able to get stronger spells, more health points, and many other benefits. A positive integer means the player has more experience and a negative integer means the player has less experience than their counterpart position.

- 'killsat20': Represents the amount of kills each player/team had 20 minutes into the match.

For this dataset, we will be focusing on games in which the matches were completed, meaning we will get rid of all the rows where the games did not finish, leaving us with 109,128 rows.


##Data Cleaning and Exploratory Data Analysis
###Data Cleaning
As stated previously, only 14 columns are being kept as they are the only ones that are relevant to the analysis.
