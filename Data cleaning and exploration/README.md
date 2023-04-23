**Purpose**

The main aim of this analysis is to perform data cleaning and exploratory analysis

data source: https://www.basketball-reference.com/leagues/NBA 2021 advanced.html


**Files Description**

- Data: Raw_Data_NBA_players_stats.csv 

- Cleaned Data (after performing data cleaning using ipynb file): cleaned_NBA_players_stats.csv

- Jupyter notebook for analysis in python : project_data_exploration.ipynb

- Deatiled report for describing the codes of ipynb file and providing further insights : report.pdf

**How to Run The ipynb script**

Easy way to run the script in anyones's local machine is:

  a)  Download "project_data_exploration.ipynb" file  and 
      Raw_Data_NBA_players_stats.csv dataset from the repository.

  b)  Place both files in the same folder and set that folder as current directory in jupyter-notebook.

  c)  From jupyter-notebook, browse to the "project_data_exploration.ipynb".

  d)  Click on kernel tab and then click 'Restart & Run All'from the dropdown list

  e)  Wait for running the whole script that will provide analysis and will generate cleaned_NBA_players_stats.csv file
 
**Task Description**

------------------------------------------Task 1: Data Preparation--------------------------------------------

Have a look at the file NBA players stats.csv 

This data set contains statistics of 492 NBA (National Basketball Association) players during 2020-2021 season. 

Note that a few players appeared more than once in the data since they played for different teams in the
same season. The description of each column in this data set is given below.

• Rk – Rank
• Pos – Position, the value can be only PF, PG, C, SG, SF, PG-SG, or SF-PF.
• Age – Player’s age on February 1 of the season
• Tm – Team, the value can be only MIA, MIL, NOP, SAS, PHO, MEM, TOT, BRK,
CLE, ORL, LAL, POR, TOR, CHI, WAS, UTA, SAC, CHO, NYK, DEN, LAC,
GSW, OKC, MIN, DET, DAL, IND, ATL, PHI, BOS, HOU.
• G – Games, the number of games played, each team has a maximum of 82 games in
a season.
• GS – Games Started, the number of games played as a starter.
• MP – Minutes Played, regardless of overtime, each game has 48 minutes.
• FG – Field Goals, all Field Goals including 2-Point Field Goals and 3-Point Field
Goals.
• FGA – Field Goal Attempts, the number of field goal attempts, including 2-Point
Field Goal Attempts and 3-Point Field Goal Attempts.
• FG% – Field Goal Percentage, equals to FG/FGA.
• 3P – 3-Point Field Goals, the number of 3-Point Field Goals. A 3-point field goal
can be scored 3 points.
• 3PA – 3-Point Field Goal Attempts, the number of 3-Point Field Goal Attempts.
• 3P% – 3-Point Field Goal Percentage, equals to 3P/3PA.
• 2P – 2-Point Field Goals, the number of 2-Point Field Goals. A 2-point field goal
can be scored 2 points.
• 2PA – 2-point Field Goal Attempts, the number of 2-Point Field Goal Attempts.
• 2P% – 2-Point Field Goal Percentage, equals to 2P/2PA.
• FT – Free Throws, the number of Free Throws, one free throw goal worth 1 point.
• FTA – Free Throw Attempts, the number of Free Throw Attempts.
• FT% – Free Throw Percentage, equals to FT/FTA.
• ORB – Offensive Rebounds, the number of Offensive Rebounds.
• DRB – Defensive Rebounds, the number of Defensive Rebounds.
• TRB – Total Rebounds, the number of Offensive Rebounds and Defensive Rebounds.
• AST – Assists, the number of Assists.
• STL – Steals, the number of Steals.
• BLK – Blocks, the number of Blocks.
• TOV – Turnovers, the number of Turnovers.
• PF – Personal Fouls, the number of Personal Fouls. The NBA allots players six
personal fouls per game; players are automatically disqualified from competition
upon incurring their sixth foul, and a referee will eject them from the game.
• PTS – The Total Points, including 2 points, 3 points and free throws, and the value
must be less than 2000.

Being a careful data scientist, you know that it is vital to carefully check any available
data before starting to analyse it. Your task is to prepare the provided data for analysis.
You will start by loading the CSV data from the file (using appropriate pandas functions)
and checking whether the loaded data is equivalent to the data in the source CSV file. Then,
you need to clean the data by using the knowledge we taught in the lectures. You need to
deal with all the potential issues/errors in the data appropriately and then write the cleaned
data into a comma-separated values (csv) file named ‘cleaned NBA players stats.csv’.

---------------------------------Task 2: Data Exploration------------------------------------------

Explore the provided data based on the following steps:

1. Explore the players’ total points: Please analyze the composition of the total points
of the top five players with the most points.
2. Assuming that the data collector makes an entry error when collecting data, it can
be ensured that the error occurred in the 3P, 3PA and 3P% columns, but it is not
sure which player’s information the error lies on. Please try to explore the error by
visualization to identify how many errors there are and try to fix it.
3. Please analyze the relationship between the player’s total points and the rest features
(columns). Please use at least three other columns.

---------------------------------Task 3: Report----------------------------------------------------

Write a report and save it in a file called report.pdf, 
• Create a heading called “Data Preparation” in your report.

– Provide a brief explanation of how you addressed the task. For the steps of
dealing with the potential issues/errors, please create a sub-section for each
type of errors you dealt with (e.g. typos, extra whitespaces, sanity checks for
impossible values, and missing values etc), and also explain and justify how you
dealt with each kind of errors.

• Create a heading called “Data Exploration” in your report.

– For each numbered step in Task 2 above, create a sub-section with corresponding
numbering