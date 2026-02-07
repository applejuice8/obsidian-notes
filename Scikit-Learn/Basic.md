
---

# Assign X, y
```python
import pandas as pd

# Read file
df = pd.read_csv('file.csv')

# Drop unnecessary cols
df = df.drop(columns=['Name', 'Age'])

# Assign X, y
X = df.drop(columns=['Price', 'Date'])  # All except these 2
y = df['Last change']
```

---

# Split Training, Testing
- Training size = 1 - Test size
- `stratify=y` maintains same ratio of y in training, testing
- Example (3000 data, only 20 are True, others False). `stratify=y` ensures 10 Trues are in each
```python
from sklearn.model_selection import train_test_split

# 80% to train, 20% to test
X_train, X_test, y_train, y_test = train_test_split(
	X, y, test_size=0.2, random_state=11, stratify=y
)
```

---

# Fit vs Fit Transform
- `fit()` computes mean, standard deviation
- `transform()` uses the mean, standard deviation calculated previously for the test data
- `fit_transform()` does both
- Using `fit_transform()` on testing data is cheating (Because it 'sees' the testing data)
- Use `fit_transform()` on training data, Use `transform()` on testing data
- Pipeline, Column transformer automatically ensures `fit_transform()` applied on training data, `transform()` applied on testing data
```python
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

---

# Predict
```python
y_pred = model.predict(X_test)
y_pred = pipe.predict(X_test)
```

---

# Save, Load Model
- With Joblib
```python
import joblib

# Save model
joblib.dump(pipe, 'my_model.joblib')

# Load model
joblib.load('my_model.joblib')
```