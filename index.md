---
layout: default
title: Home
---

## Introduction
League of Legends (LoL) is a multiplayer online battle arena video game developed and published by Riot Games. Players pilot “Champions” battle in teams of 5 each, in a player-vs-player format. League of Legends is the world's largest esport, with several large international tournaments. We are working with a dataset of professional LoL matches developed by Oracle’s Elixir. In this project, we only used data for matches that took place in 2025. Each row represents a single match played by a summoner, including key in-game metrics such as gold earned, kills, assists, and objective control at various points in the game.
The focus of this project is on the following question:
Can early-game 15-minute statistics predict who wins a professional League of Legends match?

This question is important because early-game performance often drives match outcomes, and identifying predictive factors early can help teams make strategic decisions during live play. For fans and analysts, understanding early indicators of victory makes the game more engaging and provides insight into the mechanics of competitive play.

The dataset had 120636 rows and 165 columns, of which, we used: 

| Column               | Description                                                                                              |
| -------------------- | -------------------------------------------------------------------------------------------------------- |
| `league`             | Name of the professional league in which the match took place.                                           |
| `patch`              | Game version or patch number at the time of the match.                                                   |
| `side`               | The team’s side (e.g., blue or red).                                                                     |
| `firstPick`          | Indicates whether the team had the first pick during champion selection.                                 |
| `result`             | Match outcome: 1 if the team won, 0 if the team lost.                                                    |
| `firstblood`         | 1 if the team secured first blood, 0 otherwise.                                                          |
| `firstdragon`        | 1 if the team secured the first dragon, 0 otherwise.                                                     |
| `firstherald`        | 1 if the team secured the first Rift Herald, 0 otherwise.                                                |
| `firsttower`         | 1 if the team destroyed the first tower, 0 otherwise.                                                    |
| `firstmidtower`      | 1 if the team destroyed the first mid-lane tower, 0 otherwise.                                           |
| `firsttothreetowers` | 1 if the team destroyed three towers before the opponent, 0 otherwise.                                   |
| `golddiffat15`       | Gold difference between the team and opponent at 15 minutes. Positive values indicate the team is ahead. |
| `xpdiffat15`         | Experience difference at 15 minutes. Positive values indicate the team is ahead.                         |
| `csdiffat15`         | Creep score (minion kills) difference at 15 minutes.                                                     |
| `killsat15`          | Number of kills the team had at 15 minutes.                                                              |
| `assistsat15`        | Number of assists the team had at 15 minutes.                                                            |
| `deathsat15`         | Number of deaths the team had at 15 minutes.                                                             |

Each `gameid` corresponds to up to 12 rows – one for each of the 5 players on both teams and 2 containing summary data for the two teams.
These columns capture early-game performance and objective control, which are crucial for predicting match outcomes. By focusing on the first 15 minutes of the game, we aim to identify which metrics most strongly indicate victory, providing insights for players, coaches, analysts, and fans alike.

## Data Cleaning

We started our cleaning process by removing all columns corresponding to individual players and only keeping team data. We also filtered out any columns where the data was incomplete. We then kept the following columns to use as part of our preliminary analysis: `league`, `patch`, `side`, `firstPick`, `result`, `firstblood`, `firstdragon`, `firstherald`, `firsttower`, `firstmidtower`, `firsttothreetowers`, `golddiffat15`, `xpdiffat15`, `csdiffat15`, `killsat15`, `assistsat15`, `deathsat15`. 
Most of these columns contain important early-game metrics to predict `result`, and we kept the `league`, `patch` and `side` columns to check if these categorical variables had any influence on `results`.

### Univariate Analysis

We began our univariate analysis by examining distributions of `golddiffat15` and `killsat15`. These columns were selected because:

- `golddiffat15` indicates the team’s overall gold advantage early in the game, which is extremely important in LoL matches.  
- `killsat15` captures early aggression and fighting success, which correlates with control of the map and objectives.

<div style="display: flex; flex-wrap: wrap; gap: 20px;">

  <div style="flex: 1; min-width: 300px;">
    <iframe src="assets/plots/plot_golddiff.html" width="100%" height="400" style="border:none;"></iframe>
    <p style="text-align:center;">Histogram of Gold Difference at 15 Minutes</p>
  </div>

  <div style="flex: 1; min-width: 300px;">
    <iframe src="assets/plots/plot_kills.html" width="100%" height="400" style="border:none;"></iframe>
    <p style="text-align:center;">Histogram of Kills at 15 minutes</p>
  </div>

</div>

We noticed that the `golddiffat15` plot looked almost normally distributed, while the `killsat15` histogram is heavily skewed to the right, which is understandable, as it may be rare for teams to get a very high number of kills this early in the game.

### Bivariate Analysis

For bivariate analyses, we explored relationships between each of these columns and the `result` variable (win/loss). This step helped us assess which early-game metrics are most predictive of success and informed feature selection for later modeling.

<div style="display: flex; flex-wrap: wrap; gap: 20px;">

  <div style="flex: 1; min-width: 300px;">
    <iframe src="assets/plots/golddiff_win.html" width="100%" height="400" style="border:none;"></iframe>
    <p style="text-align:center;">Gold Difference vs. Win at 15 Minutes</p>
  </div>

  <div style="flex: 1; min-width: 300px;">
    <iframe src="assets/plots/kills_win.html" width="100%" height="400" style="border:none;"></iframe>
    <p style="text-align:center;">Kills vs. Win at 15 minutes</p>
  </div>

</div>

### Average Gold Difference by League and Side

We summarized average gold difference at 15 minutes across leagues and sides using a pivot table.

This table shows the average early-game gold advantage for blue and red sides in each league, allowing us to identify patterns such as:

- Certain leagues having higher average gold advantages, possibly due to differences in professional playstyles.
- Differences between blue and red side performance, which may influence strategic planning or draft advantages.

| League | Blue Side Avg Gold Diff | Red Side Avg Gold Diff | Blue Advantage |
|--------|------------------------|-----------------------|----------------|
| EWC    | 0.67                   | 0.33                  | 0.33           |
| IC     | 0.64                   | 0.36                  | 0.29           |
| NLC    | 0.60                   | 0.40                  | 0.20           |
| HW     | 0.60                   | 0.40                  | 0.19           |
| HC     | 0.59                   | 0.41                  | 0.18           |

The first 5 rows of the pivot table are shown above. The full table contains 44 rows.

## Missingness Analysis

### MNAR Analysis

One column that is likely MNAR in this dataset is `golddiffat25` wihch represents the gold difference between teams at the 25-minute mark of a match. In the dataset, this column is sometimes missing because some games end before 25 minutes which means the statistic cannot be recorded. This missingness comes from the data generating process of League of Legends matches. If a team wins extremely quickly (e.g. through a dominant early-game strategy), the match could possibly conclude before the 25-minute timestamp.

Because of this, the missingness of `golddiffat25` likely depends on the unobserved value itself. In games that end early, the true gold difference at 25 minutes would likely be very large in magnitude whihc often strongly favors the winning team. However, since the game ends before reaching 25 minutes, that value is never observed. As a result, rows with extreme gold advantages are more likely to have missing `golddiffat25` values, suggesting the missingness depends on the value that would have been recorded.

Additional data about game duration or an indicator for whether the match reached 25 minutes could help explain this missingness. Conditioning on these additional variables would make the missingness more explainable using observed data which might make the column Missing At Random (MAR) rather than MNAR.  


### Missingness Dependency

To understand whether missing values in `golddiffat25` are related to other observed features, specifically whether this qualifies as Missing At Random (MAR), we conducted permutation tests on relevant columns.

We considered the following features:

- `gamelength` – a numeric variable representing the duration of the match in seconds.
- `side` – a categorical variable indicating whether the team was on the blue or red side.

These columns were logical choices because the presence of NaN values in `golddiffat25` appears to depend on `gamelength` (matches that end early cannot have a `golddiffat25` value) but not necessarily on `side`.

We computed two test statistics:

1. Absolute difference in means for the numeric column `gamelength` between rows where `golddiffat25` is missing versus present.  
2. Total Variation Distance (TVD) for the categorical column `side` to quantify differences in the distribution between rows with missing and non-missing `golddiffat25` values.


<div style="display: flex; flex-wrap: wrap; gap: 20px;">

  <div style="flex: 1; min-width: 300px;">
    <iframe src="assets/plots/gamelength_dep.html" width="100%" height="400" style="border:none;"></iframe>
    <p style="text-align:center;">Gamelength with Golddiffat25 missing and not missing</p>
  </div>

  <div style="flex: 1; min-width: 300px;">
    <iframe src="assets/plots/gl_obs_test.html" width="100%" height="400" style="border:none;"></iframe>
    <p style="text-align:center;">Gamelength test statistic distribution and observed statistic</p>
  </div>

</div>

For `gamelength`, the observed mean difference of 604.1 seconds between matches with missing `golddiffat25` and those without is highly significant (p < 0.001). This indicates that shorter matches are more likely to have missing `golddiffat25` values, suggesting that missingness is not MCAR, and may be MAR with respect to game length.  
These results suggest that missingness in `golddiffat25` is highly likely to be MAR, dependent on observable numeric variables like `gamelength` but unrelated to categorical variables like `side`.

The orange histogram (matches with missing `golddiffat25`) is shifted slightly toward shorter games compared to the blue histogram (matches where `golddiffat25` is present). Shorter games seem to be more prone to missing mid-game gold differences.

The histogram shows the null distribution of absolute mean differences under randomly permuted missingness indicators. The dashed line marks the observed difference of 604.1 seconds, which is quite far away from the rest of the null distribution, which implies a statistically significant association between missingness and game length.

<div style="display: flex; flex-wrap: wrap; gap: 20px;">

  <div style="flex: 1; min-width: 300px;">
      <iframe src="assets/plots/side_obs_test.html" width="100%" height="400" style="border:none;"></iframe>
      <p style="text-align:center;">Side test statistic distribution and observed statistic</p>
  </div>

</div>

For `side`, the observed TVD of 0 with a p-value of 1.0 shows that the distribution of blue/red sides is identical for missing and non-missing rows, meaning that `side` does not influence missingness. This can also be seen in the graph above

## Model

## Results

## Conclusion
