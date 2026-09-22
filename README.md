import pandas as pd

# Load dataset
df = pd.read_csv('IPL_Matches_2008_2022.csv')

# 1. Drop unwanted columns
df = df.drop(columns=['method'])

# 2. Standardize team names
team_mapping = {
    'Royal Challengers Bangalore': 'Royal Challengers Bengaluru',
    'Delhi Daredevils': 'Delhi Capitals',
    'Deccan Chargers': 'Sunrisers Hyderabad',
    'Kings XI Punjab': 'Punjab Kings'
}
df['team1'] = df['team1'].replace(team_mapping)
df['team2'] = df['team2'].replace(team_mapping)
df['winner'] = df['winner'].replace(team_mapping)
df['toss_winner'] = df['toss_winner'].replace(team_mapping)

# 3. Convert date column to datetime
df['date'] = pd.to_datetime(df['date'])

# 4. Handle missing values
df['city'] = df['city'].fillna(df['venue'])
df['result_margin'] = df['result_margin'].fillna(0)
df['target_runs'] = df['target_runs'].fillna(df['target_runs'].median())
df['target_overs'] = df['target_overs'].fillna(20.0)
df['winner'] = df['winner'].fillna('No Result')
df['player_of_match'] = df['player_of_match'].fillna('None')

# 5. Verify missing values are resolved
print(df.isnull().sum())
