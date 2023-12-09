# League of Legends Competitive Matches Prediction
## by Your Name

---

## Introduction

This project is a continuation of an exploratory data analysis of competitive League of Legends matches. The goal is to predict whether a League of Legends team will win a game, a crucial factor in determining a team's success and revenue generation. We will use a binary classification algorithm and evaluate our model with accuracy, as there is no significant class imbalance between the Red and Blue sides.

_Source: [Oracle's Elixir Dataset](link to the dataset)_

---

## Cleaning the Data

The data cleaning process involved selecting relevant columns and handling missing values. We focused on columns that could be obtained well before the end of the game, considering the minimum game duration is about 15 minutes.

The following are the key columns after cleaning:

- `playoffs`: Indicates if a team made the playoffs.
- `league`: Identifier of the tier and region the game was played in.
- `side`: The side of the map the team played on, Blue or Red.
- `position`: The position a champion played.
- `champion`: The champion played.
- `win`: Whether the team won.
- `kills`, `deaths`, `assists`: In-game statistics for each team.
- `damagetochampions`, `earnedgold`: Damage to champions and gold earned.
- `golddiffat10`, `xpdiffat10`, `csdiffat10`: Gold, experience, and creep score differences at 10 minutes.
- `first_`: Indicates if the team achieved the first objective (e.g., tower, dragon, baron).

_(Graph: Overview of cleaned data)_

---

## Baseline Model

For our baseline model, we selected three quantitative variables: `golddiffat10`, `xpdiffat10`, and `csdiffat10`. These features were standardized and used to train a RandomForestClassifier.

### Model Performance
- Training Accuracy: _[Insert Training Accuracy]_
- Testing Accuracy: _[Insert Testing Accuracy]_

### Feature Importances
_(Graph: Feature importances of the baseline model)_

---

## Final Model

The final model incorporates additional features and hyperparameter tuning.

### New Features
- `kda_at_10`: Kill-death-assist ratio at 10 minutes.
- `opp_kda_at_10`: Opponent's KDA at 10 minutes.
- `firstobjectives`: Product of first objectives achieved.

### Hyperparameter Tuning
Best parameters:
- Criterion: _[Insert Criterion]_
- Max Depth: _[Insert Max Depth]_
- Number of Estimators: _[Insert Number of Estimators]_

### Model Performance
- Training Accuracy: _[Insert Training Accuracy]_
- Testing Accuracy: _[Insert Testing Accuracy]_

### Feature Importances
_(Graph: Feature importances of the final model)_

### Confusion Matrix
_(Graph: Confusion matrix of the final model)_

---

## Fairness Analysis

We performed a fairness analysis to determine if the model's predictions were biased towards the team's side (Red or Blue).

### Permutation Test
- Null Hypothesis: The model's accuracy is the same for both Blue and Red sides.
- Alternative Hypothesis: The model's accuracy differs between the Blue and Red sides.

P-Value: _[Insert P-Value]_

### Conclusion
_(Graph: Visualization of the permutation test)_

Based on our analysis, we fail to reject the null hypothesis, indicating fairness in the model's performance regardless of the team's side.

---

## Conclusion

This project provided valuable insights into the factors influencing a team's likelihood of winning in League of Legends matches. The predictive model, after rigorous data cleaning, feature engineering, and hyperparameter tuning, showed improved accuracy and fairness in predictions.

---
