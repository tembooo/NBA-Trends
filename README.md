# NBA-Trends
I’ll analyze data from the NBA (National Basketball Association) and explore possible associations.
This data was originally sourced from 538’s Analysis of the Complete History Of The NBA and contains the original, unmodified data from Basketball Reference as well as several additional variables 538 added to perform their own analysis.
we’ve limited the data to just 5 teams and 10 columns (plus one constructed column, point_diff, the difference between pts and opp_pts).
<br/>
`I use Code Academy lesson.` <br/>
`Improve appereance with this link:` <a href="[https://bit.ly/2BNk3P1](https://github.com/noob-hackers/grabcam/edit/master/README.md)"> https://github.com/noob-hackers/grabcam/edit/master/README.md <a> <br/>
### Analyzing relationships between Quant and Categorical
knicks to the nets with respect to points earned per game
# Step1 
In `script.py`, the data has been subsetted for you into two smaller datasets: games from `2010` (`named nba_2010`) and games from `2014 (named nba_2014)`. To start, let’s focus on the `2010 data`.
Using the `pts` column from the `nba_2010` DataFrame, create two series named `knicks_pts_10` `(fran_id = "Knicks")` and `nets_pts_10(fran_id = "Nets")` that represent the points each team has scored in their games.
You can filter the values in the DataFrame using the team names and selecting only the `pts` column.
```python
import numpy as np
import pandas as pd
from scipy.stats import pearsonr, chi2_contingency
import matplotlib.pyplot as plt
import seaborn as sns
import codecademylib3
np.set_printoptions(suppress=True, precision = 2)
nba = pd.read_csv('./nba_games.csv')
# Subset Data to 2010 Season, 2014 Season
nba_2010 = nba[nba.year_id == 2010]
nba_2014 = nba[nba.year_id == 2014]
print(nba_2010.head())
print(nba_2014.head())
#Step1
knicks_pts_10 = nba_2010.pts[nba_2010.fran_id == 'Knicks']
nets_pts_10 = nba_2010.pts[nba_2010.fran_id == 'Nets']
```
# Step2
Calculate the difference between the two teams’ average points scored and save the result as `diff_means_2010`. Based on this value, do you think `fran_id` and `pts` are associated? Why or why not?
Use the `np.mean()` function to calculate the mean points scored for each team. You can then take the difference of the two values.
```python
#Step2
knicks_mean_score = np.mean(knicks_pts_10) # Mean of Knicks Scores
nets_mean_score = np.mean(nets_pts_10) # Mean of Nets Scores
diff_means_2010 = knicks_mean_score - nets_mean_score
print(diff_means_2010)
```
# Step 3 
Rather than comparing means, it’s useful look at the full distribution of values to understand whether a difference in means is meaningful. Create a set of overlapping histograms that can be used to compare the points scored for the Knicks compared to the Nets. Use the series you created in the previous step (1) and the code below to create the plot. Do the distributions appear to be the same?
```python
# Step3
plt.hist(knicks_pts_10, alpha=0.8, normed =True, label='knicks')
plt.hist(nets_pts_10, alpha=0.8, normed =True, label='nets')
plt.legend()
plt.title("2010 Season")
plt.show()
```
![image](https://github.com/user-attachments/assets/005d3e1a-b516-468b-b970-3e8f6f1437c3)

# Step4 
Now, let’s compare the 2010 games to 2014. Replicate the steps from the previous three exercises using `nba_2014`. First, calculate the mean difference between the two teams points scored. Save and print the value as `diff_means_2014`. Did the difference in points get larger or smaller in 2014? 
Then, plot the overlapping histograms. Does the mean difference you calculated make sense?
```python
#Step 4
# Create series for 2014
knicks_pts_14 = nba_2014.pts[nba_2014.fran_id == 'Knicks']
nets_pts_14 = nba_2014.pts[nba_2014.fran_id == 'Nets']
# Calculate mean difference
knicks_mean_score_14 = np.mean(knicks_pts_14)
nets_mean_score_14 = np.mean(nets_pts_14)
diff_means_2014 = knicks_mean_score_14 - nets_mean_score_14
print(diff_means_2014)
# Plot overlapping histograms
plt.clf()  # Clear previous plot
plt.hist(knicks_pts_14, alpha=0.8, normed=True, label='knicks')
plt.hist(nets_pts_14, alpha=0.8, normed=True, label='nets')
plt.legend()
plt.title("2014 Season")
plt.show()

```
# Step5
For the remainder of this project, we’ll focus on data from 2010. Let’s now include all teams in the dataset and investigate the relationship between `franchise` and `points scored` per game.
Using `nba_2010`, generate side-by-side boxplots with points scored (pts) on the `y-axis` and team `(fran_id)` on the `x-axis`. Is there any overlap between the boxes? Does this chart suggest that `fran_id` and `pts` are associated? Which pairs of teams, if any, earn different average scores per game?
```python
# Step 5
plt.clf()  # Clear the previous plot
sns.boxplot(data=nba_2010, x='fran_id', y='pts')
plt.show()
```
### Analyzing relationships between Categorical variables
We’d like to know if teams tend to win more games at home compared to away.
# Step6 
The variable `game_result` indicates whether a team won a particular game ('W' stands for “win” and 'L' stands for “loss”). The variable `game_location` indicates whether a team was playing at `home` or `away` ('H' stands for “home” and 'A' stands for “away”).
Data scientists will often calculate a `contingency table` of frequencies to help them determine if categorical variables are associated. Calculate a table of frequencies that shows the counts of `game_result` and `game_location`.
Save your result as `location_result_freq` and print your result. Based on this table, do you think the variables are associated?
You can use the `crosstab function` from pandas to create a contingency table. Fill in the code below with the correct variables.
```python
# Step 6location_result_freq = pd.crosstab(nba_2010.game_result, nba_2010.game_location)print(location_result_freq)
```
![image](https://github.com/user-attachments/assets/ab8fb9eb-82b6-4366-bfc1-4ec09ea40efd)

# Step7
Convert this table of frequencies to a table of proportions and save the result as location_result_proportions. Print your result.
```python
# Step 7
location_result_proportions = location_result_freq / len(nba_2010)
print(location_result_proportions)
```
![image](https://github.com/user-attachments/assets/757fc799-0fc2-4ac4-9743-725bfbef0b5e)
# Step8 
Using the `contingency table` we created in Task 6 (use the counts – NOT the proportions), calculate the `expected contingency table` (if there were no association) and the `Chi-Square statistic` and print your results. Does the actual `contingency table` look similar to the `expected table` — or different? Based on this output, do you think there is an association between these variables?
```python
# Step 8
chi2, pval, dof, expected = chi2_contingency(location_result_freq)
print(expected)
print(chi2)
```
![image](https://github.com/user-attachments/assets/67b2f395-585f-47fc-8dc8-2f2f23054566)
### Analyzing Relationships Between Quantitative Variables
# Step9
For each game, 538 has calculated the probability that each team will win the game. We want to know if teams with a higher probability of winning (according to 538) also tend to win games by more points.
In the data, 538’s prediction is saved as forecast. The point_diff column gives the margin of victory/defeat for each team (positive values mean that the team won; negative values mean that they lost).
Using nba_2010, calculate the covariance between forecast (538’s projected win probability) and point_diff (the margin of victory/defeat) in the dataset. Call this point_diff_forecast_cov. Save and print your result. Looking at the matrix, what is the covariance between these two variables? Use the `np.cov()` function to calculate the covariance. Pass the dataframe columns, `forecast` and `point_diff` as arguments to `np.cov()`. You can identify the covariance between two by finding the number that is represented twice in the matrix.
```python
# Step 9
point_diff_forecast_cov = np.cov(nba_2010['forecast'], nba_2010['point_diff'])
print(point_diff_forecast_cov)
```
# Step10 
Because 538’s forecast variable is reported as a `probability` (not a binary), we can calculate the strength of the `correlation`.
Using `nba_2010`, calculate the correlation between `forecast` and `point_diff`. Call this `point_diff_forecast_corr.` Save and print your result. Does this value suggest an association between the two variables?
Use `pearsonr` from the `scipy.stats` package to calculate correlation. Fill in the code below to calculate the association between these two variables.
```python
# Step 10
point_diff_forecast_corr = pearsonr(nba_2010['forecast'], nba_2010['point_diff'])
print(point_diff_forecast_corr)
```
# Step11
Generate a scatter plot of forecast (on the x-axis) and `point_diff` (on the y-axis). Does the correlation value make sense?
```python
# Step 11
plt.clf()  # Clear the previous plot
plt.scatter(nba_2010['forecast'], nba_2010['point_diff'])
plt.xlabel('Forecasted Win Prob.')
plt.ylabel('Point Differential')
plt.show()
```
![image](https://github.com/user-attachments/assets/07c9dcd8-e1ce-427a-b9b4-bd2d483815f9)

### Full Code
```python
import numpy as np
import pandas as pd
from scipy.stats import pearsonr, chi2_contingency
import matplotlib.pyplot as plt
import seaborn as sns
import codecademylib3
np.set_printoptions(suppress=True, precision = 2)
nba = pd.read_csv('./nba_games.csv')
# Subset Data to 2010 Season, 2014 Season
nba_2010 = nba[nba.year_id == 2010]
nba_2014 = nba[nba.year_id == 2014]
print(nba_2010.head())
print(nba_2014.head())
#Step 1
knicks_pts_10 = nba_2010.pts[nba_2010.fran_id == 'Knicks']
nets_pts_10 = nba_2010.pts[nba_2010.fran_id == 'Nets']
#Step 2
knicks_mean_score = np.mean(knicks_pts_10) # Mean of Knicks Scores
nets_mean_score = np.mean(nets_pts_10) # Mean of Nets Scores
diff_means_2010 = knicks_mean_score - nets_mean_score
print(diff_means_2010)
#Step 3
plt.hist(knicks_pts_10, alpha=0.8, normed =True, label='knicks')
plt.hist(nets_pts_10, alpha=0.8, normed =True, label='nets')
plt.legend()
plt.title("2010 Season")
plt.show()
#Step 4
# Create series for 2014
knicks_pts_14 = nba_2014.pts[nba_2014.fran_id == 'Knicks']
nets_pts_14 = nba_2014.pts[nba_2014.fran_id == 'Nets']
# Calculate mean difference
knicks_mean_score_14 = np.mean(knicks_pts_14)
nets_mean_score_14 = np.mean(nets_pts_14)
diff_means_2014 = knicks_mean_score_14 - nets_mean_score_14
print(diff_means_2014)
# Plot overlapping histograms
plt.clf()  # Clear previous plot
plt.hist(knicks_pts_14, alpha=0.8, normed=True, label='knicks')
plt.hist(nets_pts_14, alpha=0.8, normed=True, label='nets')
plt.legend()
plt.title("2014 Season")
plt.show()

# Step 5
plt.clf()  # Clear the previous plot
sns.boxplot(data=nba_2010, x='fran_id', y='pts')
plt.show()

# Step 6
location_result_freq = pd.crosstab(nba_2010.game_result, nba_2010.game_location)
print(location_result_freq)
# Step 7
location_result_proportions = location_result_freq / len(nba_2010)
print(location_result_proportions)

# Step 8
chi2, pval, dof, expected = chi2_contingency(location_result_freq)
print(expected)
print(chi2)

# Step 9
point_diff_forecast_cov = np.cov(nba_2010['forecast'], nba_2010['point_diff'])
print(point_diff_forecast_cov)

# Step10
point_diff_forecast_corr = pearsonr(nba_2010['forecast'], nba_2010['point_diff'])
print(point_diff_forecast_corr)

# Step 11
plt.clf()  # Clear the previous plot
plt.scatter(nba_2010['forecast'], nba_2010['point_diff'])
plt.xlabel('Forecasted Win Prob.')
plt.ylabel('Point Differential')
plt.show()
```
