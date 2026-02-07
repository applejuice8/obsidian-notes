
---

# Fit vs Fit Transform
- `.fit()` learns parameters from data
- `.transform()` apply the learned parameters
- `.fit_transform()` 2 in 1

---

# Fit
- `StandardScaler` learns mean, variance to be used to transform data later
- `OneHotEncoder` learns what unique categories are there
- Models memorizes weights, biases (Or trees for decision tree)

---

# Use .fit() on Training Data, .transform() on Test Data
- Using `fit_transform()` on testing data is cheating (Because it ‘sees’ the testing data)
- Use `fit_transform()` on training data, Use `transform()` on testing data
```python
# Wrong
X_scaled = scaler.fit_transform(X)

# Correct
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

## Pipeline
- Pipeline automatically ensures `fit_transform()` applied on training data, `transform()` applied on testing data
```python
pipe.fit(X_train, y_train)
```
