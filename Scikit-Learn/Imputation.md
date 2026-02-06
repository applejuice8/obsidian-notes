- Fill missing data

---

# Count Missing Cells in Each Column
```python
df.isnull().sum()
```

---

# Panda's fillna()
```python
# Fill with constant
df['name'].fillna('Unknown', inplace=True)
df['age'].fillna(0, inplace=True)

# Fill with mean, median, mode
df['age'].fillna(df['age'].mean(), inplace=True)     # Mean
df['age'].fillna(df['age'].median(), inplace=True)   # Median
df['age'].fillna(df['age'].mode()[0], inplace=True)  # Mode

# [10, None, None, 30, None, 50]
# Forward Fill (Follow value on left)
df_ffill = df.fillna(method='ffill')  # [10, 10, 10, 30, 30, 50]

# Backward Fill (Follow value on right)
df_bfill = df.fillna(method='bfill')  # [10, 30, 30, 30, 50, 50]

# Drop
df = df.dropna()  # Drop rows where any column is missing
df = df.dropna(subset=['name', 'age'])  # Specific columns
```

---

# Simple Imputer
- Default strategy is mean
```python
from sklearn.impute import SimpleImputer

# Fill with constant
imp_constant = SimpleImputer(strategy='constant', fill_value=13)
df[['Score'] = imp_constant.fit_transform(df[['Score']])

# Mean (Default)
imp_mean = SimpleImputer(strategy='mean')
imp_mean = SimpleImputer()
df[['Score'] = imp_mean.fit_transform(df[['Score']])

# Median
imp_median = SimpleImputer(strategy='median')
df[['Score'] = imp_median.fit_transform(df[['Score']])

# Mode
imp_mode = SimpleImputer(strategy='most_frequent')
df[['Score'] = imp_mode.fit_transform(df[['Score']])
```