- Identify a subset of relevant features
- Remove redundant features
- Reduce overfitting, Faster computations

---

# Univariate Selection
- Evaluates each feature individually to determine its importance
- SelectKBest with `c2` (Chi square), `f_classif` (ANOVA F-test)
```python
from sklearn.feature_selection import SelectKBest, chi2

skb = SelectKBest(score_func=chi2, k=2)
importances = skb.fit_transform(X_train, y_train)

X_train.columns[skb.get_support()]
```

---

# Recursive Feature Elimination (RFE)
- Wrapper method
- Recursively removes least important feature until reached desired number of features
```python
from sklearn.feature_selection import RFE
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()

rfe = RFE(model, n_features_to_select=2)
importances = rfe.fit_transform(X_train, y_train)

X_train.columns[rfe.get_support()]
```

---

# Tree-based Models
- Tree-based models can provide feature importance scores
- Decision tree, Random forest
```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier()
model.fit(X_train, y_train)

importances = model.feature_importances_

fi = pd.Series(importances, index=X_train.columns)
```