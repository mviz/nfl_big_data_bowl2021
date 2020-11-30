# NFL Big Data Bowl 2021

This repo covers the analysis done as part of the [Nfl Big Data Bowl 2021](https://www.kaggle.com/c/nfl-big-data-bowl-2021/overview).

The goal of the analysis was to deteremine whether the setup of a play was a good predictor for the outcome of the play using the [plays](https://www.kaggle.com/c/nfl-big-data-bowl-2021/data?select=plays.csv) from the competition. That is, I asked whether:
*   Offense formation
*   Number and breakdown of defenders (# of linebackers, # of safeties, etc)
*   Number and breakdown of offensemen (# of receivers, # of tackles, etc)

Could predict either:
*   Net yards gained by the play
*   The outcome of the pass (complete, incomplete, etc)

I confirmed my hypothesis that in fact these features would do a poor job predicting the play outcome - not because they are irrelevant, but because any strategic advantage would be counter-strategized by the defenders. 

As a fallback, I compared a few strategic scenarios and looked at which offensive formations had the longest average yards gained, and the smallest variance in yards gained.

# Method & Results
Target variables:
*   `Net yards gained`: number of yards advanced towards the endzone in the play
*   `Pass result`: the outcome of the pass, either `complete`, `incomplete`, `sack`, `intercepted`.

Features:
*   `Offense formation`: the layout of the offensive players. See list of formations [here](https://en.wikipedia.org/wiki/List_of_formations_in_American_football)
*   `Number of players in the box`: Count of defensive players close to line of scrimmage
*   `Number of pass rushers`: Count of offensive players rushing the quarter back
*   `Breakdown of defenders`: multiple counts for the number of players in each position (linebackers, safeties, etc)
*   `Breakdown of offenders`: multiple counts for the number of players in each position (tackles, receivers, etc)

## Predicting net yards gained 

Regression task of predicting the numeric value: `net yards gained`.

### Linear regression
Using a simple linear regression gave these results:

*   MSE:  `112.23`
*   MAE:  `7.46`
*   RMSE Squared:  `10.59`
*   R Squared:  `0.00014`
*   Explained Variance:  `0.4097`

Note that the estimates had very little variance. Linear regression isn't a great method here, as we don't have a linear relationship between the data and the predictors.

### Random forest regression
TODO (mviz)

## Predicting pass outcome

Classification task of predicting `pass result`.

### Random forest classifier
The random forest classifier always predicted the most common result: `complete`.
Here are the weighted results:

*   Weighted recall = `0.58`
*   Weighted precision = `0.34` 
*   Weighted F(1) Score = `0.43`
*   Weighted F(0.5) Score = `0.37`
*   Weighted false positive rate = `0.58`

## Comparing plays
Given the challenges with predicting yards gained, and the pass result, I wanted to explore some discrete breakdowns of the data.

The idea here is that on the whole there may not be a large differences in play outcomes, there may be things we can learn about basic strategy by looking at the average yards gained, and their standard deviations when you look at different scenarios, including:

*   All plays
*   Plays in the 4th quarter
*   Plays when the difference in score is less than 10 points
*   Plays with fewer than 15 yards to the endzone
*   Plays with more than 50 yards to the end zone

### Breakdowns of Net Yards Gained

*   Bolded categories have the "best" point estimates, but aren't guaranteed to be statistically different from other values
*   Note that plays `near endzone` are skewed because the maximum yardage to-be-gained 

**This table shows the summary stats of `net yards gained` for the breakdown categories.**


| Summaries |           |             |             |               |                  |
|----------:|-----------|-------------|-------------|---------------|------------------|
|           | All plays | 4th Quarter | Tight Score | Near Endzone* | Far from Endzone |
| Count     | 19239     | 5294        | 12937       | 326           | 11784            |
| Mean      | 6.47      | 6.33        | **6.5**         | 2.56          | 6.53             |
| Std Dev   | 10.57     | **10.54**       | 10.77       | 6.31          | 10.58            |
| 25%       | 0         | 0           | 0           | 0             | 0                |
| 50%       | 4         | 4           | 4           | 1             | 4                |
| 75%       | 11        | 10          | 11          | 4             | 11               |

**This table shows the average `net yards gained` for each of the different scenarios**

| Average Yards Gained |              |             |             |                  |                    |
|----------------------|--------------|-------------|-------------|------------------|--------------------|
|                      | All plays    | 4th Quarter | Tight Score | Far from Endzone | Close to Endzone** |
| Pistol               | 7.41 (251)   | 6.74 (57)   | 7.66 (181)  | **7.98** (137)       | 0.5 (6)            |
| Wildcat              | 3.97 (2569)  | 1.75 (630)  | 3.69 (29)   | **4.1** (20)         | 2.33 (3)           |
| Singleback           | 7.90 (36)    | 8.01 (8)    | **8.00** * (2074) | 7.67 (1691)      | 2.39 (59)          |
| I Form               | 8.51 (2790)  | 8.19 (162)  | 8.49 (676)  | **8.92** (549)       | 6.18 (22)          |
| Jumbo                | 1.73 (915)   | 1.8 (10)    | 1.92 (39)   | **2.77** (26)        | 0.72 (22)          |
| Shotgun              | 6.11 (12627) | **6.22** (3964) | 6.03 (8175) | 6.20 (7813)      | 2.55 (169)         |

**This table shows the standard devation of `net yards gained` for each of the different scenarios**.

| StdDev Yards Gained |               |              |              |                  |                  |
|---------------------|---------------|--------------|--------------|------------------|------------------|
|                     | All plays     | 4th Quarter  | Tight Score  | Far from Endzone | Close to Endzone |
| Pistol              | 11.69 (251)   | 10.30 (57)   | 12.11 (181)  | 9.87 (137)       | 4.14 (6)         |
| Wildcat             | 8.83 (2569)   | 4.20 (8)     | 9.51 (29)    | 10.66 (20)       | 2.08 (3)         |
| Singleback          | 12.06 (36)    | 13.27 (463)  | 12.17 (2074) | 11.76 (1691)     | 5.30 (59)        |
| I Form              | 12.19 (2790)  | 11.38 (162)  | 12.49 (676)  | 12.79 (549)      | 10.79 (22)       |
| Jumbo               | 7.17 (915)    | 2.70 (10)    | 8.09 (39)    | 9.95 (26)        | 1.28 (22)        |
| Shotgun             | 10.14 (12627) | 10.22 (3964) | 12.27 (8175) | 10.23 (7813)     | 6.60 (169)       |

### Breakdowns for probabilities of gaining N yards

**This table shows the prebability each play gains positive yardage**

| Prob of positive yards |           |             |             |                  |                  |
|------------------------|-----------|-------------|-------------|------------------|------------------|
|                        | All plays | 4th Quarter | Tight Score | Far from Endzone | Close to Endzone |
| Pistol                 | 0.64      | 0.60        | 0.65        | 0.69             | 0.50             |
| Wildcat                | 0.56      | 0.38        | 0.52        | 0.55             | 0.67             |
| Singleback             | 0.59      | 0.58        | 0.59        | 0.58             | 0.56             |
| I Form                 | 0.62      | 0.66        | 0.61        | 0.61             | 0.64             |
| Jumbo                  | 0.45      | 0.70        | 0.44        | 0.35             | 0.64             |
| Shotgun                | 0.58      | 0.59        | 0.57        | 0.58             | 0.53             |

**This table shows the prebability each play gains at least 3 yards**

Note that plays close to endzone are skewed because there may not be more than 3 yards to go.

| Prob of >3 yards |           |             |             |                  |                  |
|------------------|-----------|-------------|-------------|------------------|------------------|
|                  | All plays | 4th Quarter | Tight Score | Far from Endzone | Close to Endzone |
| Pistol           | 0.61      | 0.58        | 0.61        | 0.67             | 0.33             |
| Wildcat          | 0.53      | 0.38        | 48          | 0.50             | 0.67             |
| Singleback       | 0.55      | 0.52        | 0.55        | 0.54             | 0.29             |
| I Form           | 0.59      | 0.60        | 0.57        | 0.58             | 0.50             |
| Jumbo            | 0.10      | 0.20        | 0.10        | 0.12             | 0.09             |
| Shotgun          | 0.55      | 0.56        | 0.54        | 0.55             | 0.38             |

**This table shows the prebability each play gains at least 10 yards**

Note I did not include "close to endzone" because many of the plays had fewer than 10 yard sto go.

| Prob of >10 yards |           |             |             |                  |
|-------------------|-----------|-------------|-------------|------------------|
|                   | All plays | 4th Quarter | Tight Score | Far from Endzone |
| Pistol            | 0.31      | 0.30        | 0.31        | 0.34             |
| Wildcat           | 0.14      | 0.00        | 0.10        | 0.10             |
| Singleback        | 0.34      | 0.32        | 0.35        | 0.34             |
| I Form            | 0.35      | 0.36        | 0.34        | 0.36             |
| Jumbo             | 0.02      | 0.00        | 0.23        | 0.04             |
| Shotgun           | 0.27      | 0.27        | 0.26        | 0.27             |

# Files overview
