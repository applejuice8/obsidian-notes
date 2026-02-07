
---

- Splits data into k folds
- Trains on k-1 folds, Test on that 1 unused fold
- Repeat for each fold
- Returns list of individual scores
- Default cv `KFold` (Regression), `StratifiedKFold` (Classification)
![K Fold](https://media.geeksforgeeks.org/wp-content/uploads/20250912171723027927/train_test_split.webp)
```python
from sklearn.model_selection import cross_val_score, KFold, StratifiedKFold, RepeatedKFold, RepeatedStratifiedKFold

scores = cross_val_score(
	model,
	X,
	y,
	scoring='accuracy',
	cv=10,  # Use default cv
	
	# KFold
	cv = KFold(
	    n_splits=5,
	    shuffle=False,
	    random_state=None
	)
	
	# Repeated
	cv = RepeatedKFold(
	    n_splits=5,
	    n_repeats=10,
	    random_state=42
	)
	
	# Same params, but maintain class proportions (y)
	cv = StratifiedKFold()
	cv = RepeatedStratifiedKFold()
)

# Average
scores.mean()
```

