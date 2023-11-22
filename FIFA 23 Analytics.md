# FIFA 23 Analytics

## Overview
This project involves cleaning and analyzing the FIFA 23 dataset using Python and Pandas. The dataset contains information about football players, including their overall ratings, potential, nationality, club details, and more.

### Importing Data from CSV File
```python
import pandas as pd
df_fifa = pd.read_csv('path\\Fifa23Data.csv')
```

## Data Cleaning and Formatting

### Removing Duplicates
- Used the `drop_duplicates` function to eliminate rows with identical values in the `Full Name` and `Known As` columns.
```python
# Remove Duplicates
df_fifa = df_fifa.drop_duplicates(subset=['Full Name', 'Known As'])
```

### Data Type Conversion to Currency
Converted columns `Value(in Euro)` and `Wage(in Euro)` to numeric format, treating the values as currency.
```python
# Convert to Currency
currency_columns = ['Value(in Euro)', 'Wage(in Euro)']
df_fifa[currency_columns] = df_fifa[currency_columns].replace('[\€,]', '', regex=True).astype(float)
```

### Handling Missing Values
```python
Set 'National Team Name' to match 'Nationality' where it's '-'
Set 'National Team Position' to match 'Best Position' where it's '-'
```

## Data Analysis

### Overall Player Stats
Calculated average overall rating, maximum and minimum potential ratings.
```python
average_overall = df_fifa['Overall'].mean()
max_potential = df_fifa['Potential'].max()
min_potential = df_fifa['Potential'].min()

# Data
categories = ['Average Overall', 'Max Potential', 'Min Potential']
values = [average_overall, max_potential, min_potential]

# Create a bar plot
plt.bar(categories, values, color=['blue', 'green', 'red'],alpha = 0.5)

# Add labels and title
plt.xlabel('Categories')
plt.ylabel('Values')
plt.title('Player Statistics Summary')

# Show the plot
plt.show()
```

### Results:
![image](https://github.com/Vardhan-v/Vardhan-v/assets/149605800/2fe0b729-215a-4848-a1bd-d90c43f3432b)

#### Average Overall Rating:
The average overall rating of players is approximately 65.81.
This provides an insight into the general skill level of the players included in the dataset.

#### Maximum Potential Rating:
The highest potential rating among the players is 95.
This indicates the maximum growth or improvement potential a player can achieve in terms of skill and performance.

#### Minimum Potential Rating:
The lowest potential rating observed is 48.
This represents the minimum expected skill level a player might reach over time.


### Club Analysis
Determined the top clubs based on the average player rating and total wages.

```python
top_clubs = df_fifa.groupby('Club Name')[['Overall', 'Wage(in Euro)']].mean().sort_values(by='Overall', ascending=False).head(10)
print('Top Clubs are:',top_clubs)
```
#### Results:
Top Clubs are: 
| Club Name            | Overall | Wage (in Euro)     |
|----------------------|---------|--------------------|
| FC Bayern München    | 80.59   | 68681.82           |
| Paris Saint-Germain  | 79.81   | 85519.23           |
| Inter                | 79.73   | 65730.77           |
| Liverpool            | 78.67   | 106266.67          |
| Manchester City      | 78.52   | 135912.96          |
| AFC Richmond         | 78.11   | 48421.05           |
| Villarreal CF        | 78.08   | 34000.00           |
| RB Leipzig           | 77.71   | 48150.00           |
| Juventus             | 77.43   | 78566.67           |
| AC Milan             | 77.39   | 54677.42           |



```python
top_clubs = top_clubs.reset_index()  # Resetting index for plotting

# Plotting
fig, ax1 = plt.subplots(figsize=(13, 8))

# Bar plot for Overall Rating

# Bar plot for Overall Rating with increased distance between bars
bars = ax1.bar(top_clubs['Club Name'], top_clubs['Overall'], color='b', alpha=0.3, label='Average Overall Rating', width=0.6)


# Adding data labels for bars
for bar in bars:
    yval = bar.get_height()
    plt.text(bar.get_x() + bar.get_width()/2, yval, round(yval, 2), ha='center', va='bottom')


# Creating a secondary axis for Wage
ax2 = ax1.twinx()
ax2.plot(top_clubs['Club Name'], top_clubs['Wage(in Euro)'], color='r', marker='o', label='Average Wage', linewidth=2)
ax1.tick_params(axis='x', which='both', bottom=False, top=False, labelbottom=True)

# Adding labels and title
ax1.set_xlabel('Club Name',wrap=True)
ax1.set_ylabel('Average Overall Rating', color='b',wrap=True)
ax2.set_ylabel('Average Wage (in Euro)', color='r',wrap=True)
plt.title('Top 10 Clubs - Average Overall Rating and Wage')

# Rotating x-axis labels for better readability
plt.xticks(rotation=45, ha='right',wrap=True)

# Display the plot
plt.tight_layout()
plt.show()
```

#### Results:
![image](https://github.com/Vardhan-v/Vardhan-v/assets/149605800/1cebaf4e-f179-45d6-af8b-091e5b0f13ee)

#### Overall Ratings:
The top clubs have high average overall ratings, indicating a strong performance of players within these clubs.
FC Bayern München, Paris Saint-Germain, and Inter are the top three clubs with the highest average overall ratings.

#### Wage Distribution:
The wage distribution varies among the top clubs, with some clubs paying higher average wages than others.
Clubs like Manchester City and Liverpool have relatively high average wages, reflecting their investment in player salaries.

#### Financial Investment:
The average wage values give insights into the financial investment made by each club in securing talented players.
AFC Richmond, despite having a slightly lower overall rating, maintains a competitive position with a comparatively lower average wage.




### Positional Insights
Analyzed the distribution of players in each position and their average ratings.
```python
position_distribution = df_fifa['Best Position'].value_counts()
average_position_ratings = df_fifa.groupby('Best Position')['Overall'].mean()

# Display Position Distribution
print("Position Distribution:")
print(position_distribution)

# Display Average Position Ratings
print("\nAverage Position Ratings:")
print(average_position_ratings)

```
##### Results:
#### Position Distribution Table
| Best Position | Count |
|---------------|-------|
| CB            | 3615  |
| ST            | 2539  |
| CAM           | 2282  |
| GK            | 2045  |
| RM            | 1430  |
| CDM           | 1389  |
| CM            | 1087  |
| RB            | 916   |
| LB            | 859   |
| LM            | 782   |
| RWB           | 413   |
| LWB           | 401   |
| RW            | 295   |
| LW            | 214   |
| CF            | 70    |

#### Average Position Ratings Table
| Best Position | Overall Rating |
|---------------|----------------|
| CAM           | 65.314636      |
| CB            | 65.852282      |
| CDM           | 67.037437      |
| CF            | 71.171429      |
| CM            | 68.035879      |
| GK            | 64.437653      |
| LB            | 65.998836      |
| LM            | 65.832481      |
| LW            | 68.827103      |
| LWB           | 66.077307      |
| RB            | 65.800218      |
| RM            | 64.553147      |
| RW            | 68.142373      |
| RWB           | 66.220339      |
| ST            | 65.530130      |

#### Visualizing Position Distribution
```python
# Visualize Position Distribution
plt.figure(figsize=(10, 6))
position_distribution.plot(kind='bar', color='skyblue')
plt.title('Position Distribution')
plt.xlabel('Position')
plt.ylabel('Count')
plt.show()
```
![image](https://github.com/Vardhan-v/Vardhan-v/assets/149605800/7822ce68-cc3f-42f7-9622-b603c8904a90)


#### Visualizing Average Position Ratings
```python
plt.figure(figsize=(12, 6))
average_position_ratings.sort_values().plot(kind='barh', color='orange')
plt.title('Average Position Ratings')
plt.xlabel('Overall Rating')
plt.ylabel('Position')
plt.show()
```
![image](https://github.com/Vardhan-v/Vardhan-v/assets/149605800/8e4cdc0e-e32b-4fa0-96a7-37bf2de37759)





### Age Group Analysis
Grouped players into different age groups and calculated the average overall ratings for each group.

```python
age_bins = [0, 25, 30, 35, 40, 100]
age_labels = ['18-25', '26-30', '31-35', '36-40', '40+']
df_fifa['Age Group'] = pd.cut(df_fifa['Age'], bins=age_bins, labels=age_labels)
average_age_group_ratings = df_fifa.groupby('Age Group')['Overall'].mean()
```

### Nationality Insights
Explored the distribution of players across different nationalities and their average ratings.

```python
top_nationalities = df_fifa['Nationality'].value_counts().head(10)
average_nationality_ratings = df_fifa.groupby('Nationality')['Overall'].mean()
```
### Contract Durations
Investigated the distribution of player contracts until a certain year.

```python
contract_distribution = df_fifa['Contract Until'].value_counts()
```
### Export to Excel
Saved the cleaned dataset to an Excel file.
```python
df_fifa.to_excel('F://Fifa_CF_DA_Py_Simple.xlsx', index=False)
```
