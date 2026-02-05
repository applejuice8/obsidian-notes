
---

# Polynomial Features
- Only use on linear models (Linear regression, Ridge, Lasso, Elastic Net, Perceptron)
- Models learn nonlinearities automatically, don’t need to use (Decision tree, Random forest, Gradient Boosting, HistGB, AdaBoost, KNN, SVM with poly kernel, MLP)
```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(degree=2, include_bias=False)
X_poly = poly.fit_transform(X)
```

## Interaction Only
- Only cross-features interactions, no self-powers
- Full polynomial (`x1, x2 —> x1, x2, x1x2, x1^2, x2^2`)
- Interaction only (`x1, x2 —> x1, x2, x1x2`)
```python
poly = PolynomialFeatures(degree=2, include_bias=False, interaction_only=True)
```

---

# Polynomial Regression
```python
poly_reg = Pipeline([
	('poly', PolynomialFeatures(degree=2, include_bias=False)),
	('linear', LinearRegression())
])

poly_reg.fit(X_train, y_train)
```
