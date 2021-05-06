# data_science_projects
This repository shows the performances on statistical data analysis, data visualisation and data wrangling works.
The analysis was performed on NBA data sets where performance of the players of different teams of a season was evaluated.
The Data of the variables were needed lot of cleaning. 
Sanity checks, missing value handling, impossible values handling, outlier detections were performed.
# details of the data
The data set is provided by Basketball-Reference.com1(1https://www.basketball-reference.com/leagues/NBA 2021 advanced.html),
which contains stats of 492 NBA (National Basketball Association) players during 2020-2021 season. Note that a few
players appeared more than once in the data since they played for dierent teams in the
same season. 

The description of each column in this data set is given below.
• Rk { Rank
• Pos { Position, the value can be only PF, PG, C, SG, SF, PG-SG, or SF-PF.
• Age { Player's age on February 1 of the season
• Tm { Team, the value can be only MIA, MIL, NOP, SAS, PHO, MEM, TOT, BRK,
CLE, ORL, LAL, POR, TOR, CHI, WAS, UTA, SAC, CHO, NYK, DEN, LAC,
GSW, OKC, MIN, DET, DAL, IND, ATL, PHI, BOS, HOU.
• G { Games, the number of games played, each team has a maximum of 82 games in a season}.
• GS { Games Started, the number of games played as a starter.
• MP { Minutes Played, regardless of overtime, each game has 48 minutes.
• FG { Field Goals, all Field Goals including 2-Point Field Goals and 3-Point Field Goals.
• FGA { Field Goal Attempts, the number of field goal attempts, including 2-Point
Field Goal Attempts and 3-Point Field Goal Attempts.
• FG% { Field Goal Percentage, equals to FG
• 3P { 3-Point Field Goals, the number of 3-Point Field Goals. A 3-point eld goal
can be scored 3 points.
• 3PA { 3-Point Field Goal Attempts, the number of 3-Point Field Goal Attempts.
• 3P% { 3-Point Field Goal Percentage, equals to 3P
• 2P { 2-Point Field Goals, the number of 2-Point Field Goals. A 2-point eld goal
can be scored 2 points.
• 2PA { 2-point Field Goal Attempts, the number of 2-Point Field Goal Attempts.
• 2P% { 2-Point Field Goal Percentage, equals to 2P
• FT { Free Throws, the number of Free Throws, one free throw goal worth 1 point.
• FTA { Free Throw Attempts, the number of Free Throw Attempts.
• FT% { Free Throw Percentage, equals to FT
• ORB { Ofensive Rebounds, the number of Ofensive Rebounds.
• DRB { Defensive Rebounds, the number of Defensive Rebounds.
• TRB { Total Rebounds, the number of Ofensive Rebounds and Defensive Rebounds.
• AST { Assists, the number of Assists.
• STL { Steals, the number of Steals.
• BLK { Blocks, the number of Blocks.
• TOV { Turnovers, the number of Turnovers.
• PF { Personal Fouls, the number of Personal Fouls. The NBA allots players six
personal fouls per game; players are automatically disqualied from competition
upon incurring their sixth foul, and a referee will eject them from the game.
• PTS { The Total Points, including 2 points, 3 points and free throws, and the value
must be less than 2000.

# details of the task:
1) Clean the data. Remove anamolies, handle missing values, outliers.
2) Explore the players' total points: Please analyze the composition of the total points
of the top five players with the most points.
3) Assuming that the data collector makes an entry error when collecting data, it can
be ensured that the error occurred in the 3P, 3PA and 3P% columns, but it is not
sure which player's information the error lies on. Please try to explore the error by
visualization to identify how many errors there are and try to fix it.
4) Please analyze the relationship between the player's total points and the rest features
(columns). Please use at least three other columns. 
5) Create a report detailing the tasks mentioned above.




