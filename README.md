# League of Legends Competitive Matches Prediction: Is the First Baron So Important?
## by Steven Jiang

---

## Introduction

This project is a continuation of an EDA analysis of League of Legends professional matches. The goal is to predict whether a League of Legends team will win in a match game. One thing worth pointing out is that I have heard people saying the first baron is very important in a match, and we will figure it out in this project. Techniqally, we will use a binary classifier with evaluating our model with accuracy. Finally, we will find out if our model is fair between Blue and Red sides. 

---

## Cleaning the Data

The data cleaning process involved selecting relevant columns and handling missing values. We pick the team rows out of the whole dataset since we are predicting win rate of teams in matches. For both models, we are using accuracy as the performancec metric.

P.S.: Since we want to figure out the importance of firstbaron in predicting match results, our time of prediction is "right after the first baron" and we delete the matches that do not have firstbaron data. 

The key columns after cleaning are as follows:

- `playoffs`: Indicates if a team made to the playoffs.
- `league`: The tier and region the game was played in.
- `position`: if the row is a team or player (if so, the player's position)
- `side`: The side of the map the team played on: Blue or Red.
- `result`: Whether the team won. 
- `kills`, `deaths`, `assists`: Total kills, deaths, and assists of a team when the game ends.
- `damagetochampions`, `earnedgold`: Damage to champions and gold earned.
- `golddiffat10`, `xpdiffat10`, `csdiffat10`: Gold, experience, and creep (little minion) score differences between teams at 10 minutes.
- `killsat10`, `deathsat10`, `assistsat10`: Total kills, deaths, and assists of a team at 10 minutes.
- `first_`: Indicates if the team achieved the first objective (e.g., tower, dragon, baron).

Here's the header of the cleaned dataset:

|   playoffs | league   | side   | position   |   result |   kills |   deaths |   assists |   damagetochampions |   earnedgold |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   deathsat10 |   assistsat10 |   firstblood |   firstdragon |   firstherald |   firstbaron |   firsttower |   turretplates |
|-----------:|:---------|:-------|:-----------|---------:|--------:|---------:|----------:|--------------------:|-------------:|---------------:|-------------:|-------------:|------------:|-------------:|--------------:|-------------:|--------------:|--------------:|-------------:|-------------:|---------------:|
|          0 | LCKC     | Blue   | team       |        0 |       9 |       19 |        19 |               56560 |        28222 |           1523 |          137 |           -8 |           3 |            0 |             5 |            1 |             0 |             1 |            0 |            1 |              5 |
|          0 | LCKC     | Red    | team       |        1 |      19 |        9 |        62 |               79912 |        33769 |          -1523 |         -137 |            8 |           0 |            3 |             0 |            0 |             1 |             0 |            0 |            0 |              0 |
|          0 | LCKC     | Blue   | team       |        0 |       3 |       16 |         7 |               59579 |        34688 |          -1619 |        -1586 |          -27 |           1 |            3 |             1 |            0 |             0 |             1 |            0 |            0 |              2 |
|          0 | LCKC     | Red    | team       |        1 |      16 |        3 |        39 |               74855 |        48063 |           1619 |         1586 |           27 |           3 |            1 |             3 |            1 |             1 |             0 |            1 |            1 |              3 |
|          0 | LCKC     | Blue   | team       |        1 |      14 |        5 |        42 |               67376 |        41372 |           -103 |          813 |           13 |           0 |            1 |             0 |            0 |             1 |             0 |            1 |            1 |              1 |

---

## Baseline Model

For our baseline model, we selected three quantitative variables: `golddiffat10`, `xpdiffat10`, and `csdiffat10`. These features were standardized and used to train a RandomForestClassifier.

### Model Performance

The baseline model performances are as follows:
- Training Accuracy: 1.0
- Testing Accuracy: 0.6636234849677318

### Feature Importances

Here's the list of feature performances for the Baseline model:

<iframe src="assets/basemodel_feature_importance.html" width=800 height=600 frameBorder=0></iframe>

---

## Final Model

To make the prediction model better and avoid overfitting, our final model incorporates additional features and hyperparameter tuning.

### New Features

In the revised version of our model, we incorporated three innovative features:
- `kda_at_10`: This metric is calculated by dividing the sum of a team's total kills and assists at the 10-minute mark by their total deaths at the same time. If the death is 0, then `kda_at_10` is just kill+assists by convention.
- `firstbaron`: If the team got the first baron.
- `firstdragon`: If the team got the first dragon.
- `firstblood`: If the team got the first blood.
- `firstherald`: If the team got the first herald.

The KDA attributes effectively summarize the team and their opponent's kills, deaths, and assists, with higher values being more favorable. Notably, for teams with zero deaths, their KDA is computed as the sum of kills and assists, adhering to a standard set by the League of Legends community. The purpose of this feature is to offer a way to assess a team's comparative performance.

The "firstobjectives" feature is designed to measure the advantage a team gains by securing the initial key objectives, a factor not immediately apparent when examining each objective in isolation.

### Hyperparameter Tuning

For the final model, I opted to use a RandomForestClassifier again, hoping that added complexity and fine-tuning parameters would enhance our predictive accuracy.

Regarding hyperparameter tuning, we aim to identify the optimal number of estimators (n_estimators), the most effective criterion, and the ideal maximum depth for each tree.

The logic behind this is:

n_estimators: Adjusting this parameter helps find the ideal number of trees for the forest, balancing between having too few, which might not capture sufficient information, and too many, which could potentially lead to overfitting.
criterion: This is a gauge of our model's error. We are evaluating between entropy and gini impurity, which differ in their criteria for splitting nodes.
max_depth: This controls the maximum depth of each tree. Deeper trees might overfit on training data and not perform well with new data, while shallower trees might not gather enough relevant information.

Using sklearn's GridSearchCV, we are able to find out the best parameters:
- Criterion: `gini`
- Max Depth: `10`
- Number of Estimators: `100`

### Model Performance

The final model performances are as follows:
- Training Accuracy: 0.8693745361311652
- Testing Accuracy: 0.8364552180072407

### Feature Importances

In the final model, we added one more quantitative variable and four ordinal variables. Here's the list of feature performances for the Baseline model:

<iframe src="assets/finalmodel_feature_importance.html" width=800 height=600 frameBorder=0></iframe>

### Confusion Matrix

Here is a display of the confusion matrix of the final model:

<iframe src="assets/finalmodel_confusion_matrix.html" width=800 height=600 frameBorder=0></iframe>

---

## Fairness Analysis

We performed a fairness analysis to determine if the model's predictions were biased towards the team's side (Red or Blue).

### Permutation Test
- Null Hypothesis: The model's accuracy is the same for both Blue and Red sides.
- Alternative Hypothesis: The model's accuracy differs between the Blue and Red sides.

After conducting 1000 permutations, we get a P-Value of 0.975. Therefore, we reject the null hypothesis at 5% significance level, indicating fairness in the model's performance regardless of the team's side. 


---

## Conclusion

This project provided valuable insights into the factors influencing a team's likelihood of winning in League of Legends matches. The predictive model, after rigorous data cleaning, feature engineering, and hyperparameter tuning, showed improved accuracy and fairness in predictions.

---
