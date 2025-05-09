IPL Data Analysis PRoject
import pandas as pd
import numpy as np 
import matplotlib.pyplot as plt
import seaborn as sns 
import plotly_express as px
import plotly.graph_objects as go   
import plotly.io as pio
import plotly.colors as pc



df=pd.read_csv('ipl.csv')
df.head()
df.info()
df.isnull().sum()
df.describe()
df.duplicated().sum()
fig=px.histogram(df,x='match_winner',title='Match Winner Distribution')
fig.update_layout(xaxis_title='Match Winner',yaxis_title='Count')   
fig.show()
df.head(5)
team_wins = df['match_winner'].value_counts()

for team, wins in team_wins.items():
    print(f"{team} = {wins} wins")

team_runs = df.groupby('team1')['first_ings_score'].sum().add(
    df.groupby('team2')['first_ings_score'].sum(), fill_value=0)

for team, runs in team_runs.items():
    print(f"{team} = {runs} wins")
# Plotting total runs for each team
fig = px.bar(team_runs, 
             x=team_runs.index, 
             y=team_runs.values, 
             title="Total Runs by Each Team", 
             labels={'x': 'Team', 'y': 'Total Runs'},
             color=team_runs.index)

fig.update_layout(xaxis_title="Team", yaxis_title="Total Runs")
fig.show()
# Plotting total wins for each team
fig = px.bar(team_wins, 
             x=team_wins.index, 
             y=team_wins.values, 
             title="Total Wins by Each Team", 
             labels={'x': 'Team', 'y': 'Total Wins'},
             color=team_wins.index)

fig.update_layout(xaxis_title="Team", yaxis_title="Total Wins")
fig.show()
# Calculate total wickets taken by each team
team_wickets = df.groupby('team1')['second_ings_wkts'].sum().add(
    df.groupby('team2')['first_ings_wkts'].sum(), fill_value=0
)

# Display the total wickets for each team
for team, wickets in team_wickets.items():
    print(f"{team}: {wickets} wickets")
# Plotting total wickets taken by each team
fig = px.bar(team_wickets, 
             x=team_wickets.index, 
             y=team_wickets.values, 
             title="Total Wickets Taken by Each Team", 
             labels={'x': 'Team', 'y': 'Total Wickets'},
             color=team_wickets.index)

fig.update_layout(xaxis_title="Team", yaxis_title="Total Wickets")
fig.show()
# Calculate total innings played by each team
total_innings_played = df['team1'].value_counts().add(df['team2'].value_counts(), fill_value=0)

# Display the total innings played for each team
for team, innings in total_innings_played.items():
    print(f"{team}: {int(innings)} innings")
# Plotting total innings played by each team
fig = px.bar(total_innings_played, 
             x=total_innings_played.index, 
             y=total_innings_played.values, 
             title="Total Innings Played by Each Team", 
             labels={'x': 'Team', 'y': 'Total Innings'},
             color=total_innings_played.index)

fig.update_layout(xaxis_title="Team", yaxis_title="Total Innings")
fig.show()
# Create a DataFrame to display the required data
team_stats = pd.DataFrame({
    'Total Wickets': team_wickets,
    'Total Runs': team_runs,
    'Total Innings Played': total_innings_played
})

# Display the table
team_stats.reset_index().rename(columns={'index': 'Team'})
team_stats.to_csv('team_stats.csv', index=True)
