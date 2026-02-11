
---

# Grid Search CV
- Tries all combinations of parameters specified
- Evaluates each combination using CV
- Selects the best set of hyperparameters
- `n_jobs` specifies number of jobs to run in parallel (-1 means maximum)
```python
from sklearn.model_selection import GridSearchCV

# Parameters specific to the model used
param_grid = {
	'n_estimators': [500, 1000, 1500],
	'min_samples_split': [5, 10, 15],
	'min_samples_leaf': [1, 2, 4],
	'max_depth': [10, 20, 30]
}

search = GridSearchCV(
	estimator=RandomForestClassifier(),
	param_grid=param_grid,
	cv=2,
	scoring='accuracy',
	n_jobs=-1  # Use max processors
)

# Fit
search.fit(X_train, y_train)
```

---

# Randomized Search CV
- Instead of testing all permutations, only test `n_iter` of it
- Cannot find absolute best, but good enough
- Extra parameter `n_iter` and `random_state`, others same
- `param_distributions` instead of `param_grid`
```python
from sklearn.model_selection import RandomizedSearchCV

search = RandomizedSearchCV(
	# In addition to GridSearchCV params
	n_iter=10,
	random_state=11
)
```

---

# Metrics
```python
search.best_params_  # Best params
search.best_score_   # Average CV score with best params
search.best_estimator_

search.score(X_test, y_test)  # Final evaluation
```

---

# Advanced Nesting

## Hyperparameters for Preprocessor and Model
- Left is parent, Right is child (Attribute)
- `animal__dog__pitbull` means pitbull inside dog inside animal
```python
from sklearn.model_selection import GridSearchCV

preprocessor = ColumnTransformer([
    ('num', StandardScaler(), num_cols),
    ('cat', OneHotEncoder(), cat_cols)
])

pipe = Pipeline([
    ('preprocessor', preprocessor),
    ('model', LogisticRegression())
])

# CV
param_grid = {
    'preprocessor__num__with_mean': [True, False],
    'preprocessor__cat__min_frequency': [None, 10],
    'model__C': [0.01, 0.1, 1],
    'model__max_iter': [100, 1000, 10000]
}

search = GridSearchCV(
	estimator=pipe,
	param_grid=param_grid
)
```

## Hyperparameter Includes Different Models
- Sklearn requires an estimator when creating pipeline, thus use dummy as placeholder
```python
from sklearn.model_selection import RandomizedSearchCV
from sklearn.dummy import DummyClassifier

# Column transformer
preprocessor = ColumnTransformer([
    ('num', StandardScaler(), num_cols),
    ('cat', OneHotEncoder(), cat_cols)
])

# Pipeline with dummy classifier
pipe = Pipeline([
    ('proprocessor', preprocessor),
    ('model', DummyClassifier())  # Placeholder
])

# Param distributions
param_distributions = [
    {
        'preprocessor__num': [StandardScaler(), None],
        'preprocessor__cat': [OneHotEncoder(), None],
        'model': [LogisticRegression(max_iter=1000)],
        'model__C': [0.1, 1, 10],
        'model__solver': ['lbfgs']
    },
    {
        'preprocessor__num': [StandardScaler(), None],
        'preprocessor__cat': [OneHotEncoder(), None],
        'model': [SVC()],
        'model__C': [0.1, 1, 10],
        'model__kernel': ['linear', 'rbf']
    }
]

# CV
rs = RandomizedSearchCV(
    pipe,
    param_distributions=param_distributions,
    n_iter=5,
    cv=3,
    scoring='accuracy',
    random_state=42
)
```

