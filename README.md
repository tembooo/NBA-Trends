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
