# NFL Big Data Bowl 2021

This repo covers the analysis done as part of the [Nfl Big Dtaa Bowl 2021](https://www.kaggle.com/c/nfl-big-data-bowl-2021/overview).

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

MSE:  `112.23`
MAE:  `7.46`
RMSE Squared:  `10.59`
R Squared:  `0.00014`
Explained Variance:  `0.4097`

Note that the estimates had very little variance. Linear regression isn't a great method here, as we don't have a linear relationship between the data and the predictors.

### Random forest regression
TODO (mviz)

## Predicting pass outcome

Classification task of predicting `pass result`.

### Random forest classifier
The random forest classifier always predicted the most common result: `complete`.
Here are the weighted results:

Weighted recall = `0.58`
Weighted precision = `0.34` 
Weighted F(1) Score = `0.43`
Weighted F(0.5) Score = `0.37`
Weighted false positive rate = `0.58`

## Comparing plays

# Files overview
