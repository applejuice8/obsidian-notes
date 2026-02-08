
---

# Classification
- Logistic regression

# Regression
- Linear regression
- Lasso
- Ridge
- ElasticNet

---

# Logistic Regression
![Logistic Regression](https://almablog-media.s3.ap-south-1.amazonaws.com/image_15_ea63b72d9e.png)
```python
from sklearn.linear_model import LogisticRegression

lr = LogisticRegression(
	# Penalty
	penalty='none',
	penalty='l1',  # Lasso
	penalty='l2',  # Ridge (Default)
	penalty='elasticnet',  # Mix L1, L2
	
	# Elastic-Net mixing parameter
	# 1 (Pure L1), 0 (Pure L2)
	l1_ratio=0.5,
	
	# Smaller = Stronger regularization
	# Larger (Overfit)
	C=1.0,
	
	max_iter=1000,
	random_state=42
)
```

---

# Linear Regression
- `alpha` is regularization strength (L1 for Lasso, L2 for Ridge)
```python
from sklearn.linear_model import LinearRegression, Lasso, Ridge, ElasticNet

lr = LinearRegression()
l = Lasso(alpha=1.0, max_iter=1000)
r = Ridge(alpha=0.1)

en = ElasticNet(
    alpha=1.0,  # 0.0 (Ridge), 1.0 (Lasso)
    l1_ratio=0.5,
    max_iter=1000
)
```

## Ridge Regression
```python
from sklearn.linear_model import Ridge

```