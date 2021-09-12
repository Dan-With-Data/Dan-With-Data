# relevant installs
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# assigning dataset
pl_data = pd.read_csv('C:/Projects/Adjusted Data For Python/Premier_League_2020-2021_Data.csv', index_col='Date')

# displaying first 5 rows in the dataset
pl_data.head(5)

# verifying all matches are included in the dataset
number_of_matches = pl_data['Time'].count()
print('Total number of matches in the dataset = ', number_of_matches)

# summary of values in the dataset
print(pl_data.info())

# converting index type from object to datetime
pl_data.index = pd.to_datetime(pl_data.index)

# counting the number of goals in the season and finding the avg 
total_goals_scored = pl_data['FTHG'].sum() + pl_data['FTAG'].sum()
print(total_goals_scored)
mean_goals_scored = total_goals_scored/number_of_matches
print('Average number of goals scored in a match was = ', mean_goals_scored)

# looking at the distribution of goals scored over the season using a step histogram
plt.style.use("seaborn-whitegrid")
fig, ax = plt.subplots()
ax.hist(pl_data['FTHG'], label='Home Goals', bins=[0,1,2,3,4,5,6,7,8,9], histtype='step', color='red')
ax.hist(pl_data['FTAG'], label='Away Goals', bins=[0,1,2,3,4,5,6,7,8,9], histtype='step', color='blue')
plt.xticks(np.arange(min(pl_data['FTHG']), max(pl_data['FTHG'])+1, 1.0))
ax.set_xlabel('Number of Goals')
ax.set_ylabel('Number of Games')
ax.set_title('Distribution of Goals Scored Over the 2020-2021 Season')
ax.legend()
plt.show()

# comparing Man United's HGs against AGs using a time series graph
MU_home_games = pl_data[pl_data['HomeTeam'] == 'Man United']
MU_home_games.head()    
MU_away_games = pl_data[pl_data['AwayTeam'] == 'Man United']
MU_away_games.head()
MU_cum_hg = np.cumsum(MU_home_games['FTHG'])
MU_cum_ag = np.cumsum(MU_away_games['FTAG'])
plt.style.use("ggplot")
fig, ax = plt.subplots()
ax.plot(MU_home_games.index, MU_cum_hg, label = 'Home Game Goals', color='red')
ax.plot(MU_away_games.index, MU_cum_ag, label = 'Away Game Goals', color='blue')
plt.xticks(rotation=90)
ax.set_xlabel('Dates')
ax.set_ylabel('Cumulative Goals Scored')
ax.set_title('Cumulative Sum of Manchester United Goals Over the 2020-2021 Season')
ax.legend()
plt.show()

# defining a function which compares a team's HGs against AGs
def home_against_away_goals(team):
    home_games = pl_data[pl_data['HomeTeam'] == team]  
    away_games = pl_data[pl_data['AwayTeam'] == team]
    cum_hg = np.cumsum(home_games['FTHG'])
    cum_ag = np.cumsum(away_games['FTAG'])
    fig, ax = plt.subplots()
    ax.plot(home_games.index, cum_hg, label = 'Home Game Goals', color='red')
    ax.plot(away_games.index, cum_ag, label = 'Away Game Goals', color='blue')
    plt.xticks(rotation=90)
    ax.set_xlabel('Dates')
    ax.set_ylabel('Cumulative Goals Scored')
    ax.set_title('Cumulative Sum of ' + team + ' Goals Over the 2020-2021 Season')
    ax.legend()
    plt.show()

# using created function to observe other teams with one line of code
home_against_away_goals('Fulham')
home_against_away_goals('Everton')

# assigning new datasets containing xG data
xG_20_21data = pd.read_csv('C:/Projects/Adjusted Data For Python/Premier_League_data_with_xG_2020-2021.csv')
xG_19_20data = pd.read_csv('C:/Projects/Adjusted Data For Python/Premier_League_data_with_xG_2019-2020.csv')
xG_18_19data = pd.read_csv('C:/Projects/Adjusted Data For Python/Premier_League_data_with_xG_2018-2019.csv')
xG_17_18data = pd.read_csv('C:/Projects/Adjusted Data For Python/Premier_League_data_with_xG_2017-2018.csv')

# creating a new datasets which only include team name and xG
xG_17_18_short = xG_17_18data[['Squad', 'xG']]
xG_18_19_short = xG_18_19data[['Squad', 'xG']]
xG_19_20_short = xG_19_20data[['Squad', 'xG']]
xG_20_21_short = xG_20_21data[['Squad', 'xG']]

# using an inner join to join all 4 of the new datasets into 1
xG_17_19_short = xG_17_18_short.merge(xG_18_19_short, on = 'Squad', suffixes = ('_17_18', '_18_19'))
xG_19_21_short = xG_19_20_short.merge(xG_20_21_short, on = 'Squad', suffixes = ('_19_20', '_20_21'))
xG_17_21_short = xG_17_19_short.merge(xG_19_21_short, on = 'Squad')

# displaying first 5 rows in the new dataset
xG_17_21_short.head(5)

# comparing xG between seasons to vizualize action
fig, ax = plt.subplots()
ax.boxplot([xG_17_21_short['xG_17_18'], xG_17_21_short['xG_18_19'], xG_17_21_short['xG_19_20'], xG_17_21_short['xG_20_21']]) 
ax.set_xticklabels(['17-18', '18-19', '19-20', '20-21'])
ax.set_xlabel('Football Season')
ax.set_ylabel('xG')
ax.set_title('xG Across Football Seasons')
plt.show()
xG_17_21_short['xG_19_20'].max()
print(xG_19_20data[['Squad', 'W', 'D', 'L', 'GF', 'GA', 'xG']][xG_19_20data['xG'] == 93.0])

# assigning new dataset containing total transfer window expenditure between 2014-2015 and 2020-2021
Expenditure_14_21 = pd.read_csv('C:/Projects/Adjusted Data For Python/ExpenditureEurosPL14-15_20-21.csv')

# displaying first 5 rows in the new dataset
Expenditure_14_21.head()

# summary of values in the dataset
Expenditure_14_21.info()

# in order to create a scatter plot using the expenditure dataset and an xG dataset without throwing an error, 
# the number of rows need to be equal, the expenditure dataset contains more teams so a left join on the xG dataset is required
Expenditure_14_21 = Expenditure_14_21.rename(columns={'Club': 'Squad'})
Expenditure_14_21.head()
Exp_and_xG = xG_20_21data.merge(Expenditure_14_21, on = 'Squad')
Exp_and_xG[['xG', 'Expenditure_in_Billion']].count()

# creating list for teams for scatter plot legend
t = ['Manchester City', 'Manchester Utd', 'Liverpool', 'Chelsea',
'Leicester City', 'West Ham', 'Tottenham', 'Arsenal', 'Leeds United',
'Everton', 'Aston Villa', 'Newcastle Utd', 'Wolves', 'Crystal Palace',
'Southampton', 'Brighton', 'Burnley', 'Fulham', 'West Brom', 'Sheffield Utd']

# comparing 7 years of expenditure to the xG average in the 2020-2021 season for all teams using a scatter plot
ax = sns.scatterplot(x=Exp_and_xG['Expenditure_in_Billion'], y=Exp_and_xG['xG'], hue=t)
ax.set_xlabel('Expenditure (€bn)')
ax.set_ylabel('xG (season average)')
ax.set_title('Transfer Window Expenditure Against Average xG')
ax.legend(bbox_to_anchor=(1.05, 1), loc='upper left', borderaxespad=0.)
plt.show()
