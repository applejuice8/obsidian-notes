
---

# Column Transformer vs Pipeline
![Column Transformer vs Pipeline](https://miro.medium.com/max/1190/1*I0F-ALOL8J8f6V33CDKyrA.png)

## Column Transformer
- Applies different transformations to different columns in parallel
- When preprocessing numeric, categorical features differently

## Pipeline
- Chains together multiple steps to be applied sequentially
- Each step (Except last) must be a transformer (Has `.fit()`, `.transform()` methods)
- Final step can be a predictor (Has `.fit()`, `.predict()` methods)

---

# Column Transformer
- Scale / Fill missing value for 1 or more columns with different operations
![Column Transformer](https://miro.medium.com/1*ckcmcUm9CcJh9LNQMo3alA.png)

## ColumnTransformer Object
```python
from sklearn.compose import ColumnTransformer

ct = ColumnTransformer([
    ('ohe', OneHotEncoder(sparse_output=False), ['city']),
    ('imp_mean', SimpleImputer(strategy='mean'), ['age']),
    ('pt', 'passthrough', ['time'])
], remainder='drop').set_output(transform='pandas')

modified = ct.fit_transform(df)
```

## make_column_transformer() Method
```python
from sklearn.compose import make_column_transformer

ct = make_column_transformer(
    (OneHotEncoder(sparse_output=False), ['city']),
    (SimpleImputer(strategy='mean'), ['age']),
    ('passthrough', ['time']),
    remainder='drop'
).set_output(transform='pandas')

modified = ct.fit_transform(df)
```

---

# Pipeline
- Group operations to be performed sequentially
![Pipeline](https://miro.medium.com/v2/resize:fit:1400/0*mEO_PWFmOG_POqpf.png)

## Pipeline Object
```python
from sklearn.pipeline import Pipeline

pipe = Pipeline([
	('imputer', SimpleImputer(strategy='mean')),
    ('scaler', StandardScaler()), 
    ('lr', LogisticRegression())
])

pipe.fit(X_train, y_train)
```

## make_pipeline() Method
```python
from sklearn.pipeline import make_pipeline

pipe = make_pipeline(
	SimpleImputer(strategy='mean'),
    StandardScaler(),
    LogisticRegression()
)

pipe.fit(X_train, y_train)
```

---

# Combine Column Transformer, Pipeline

## Column Transformer Inside Pipeline
```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer

# Split into numerical, categorical data
num_cols = ['age', 'salary']
cat_cols = ['city']

# Combine transformers into a ColumnTransformer
preprocessor = ColumnTransformer([
    ('num', StandardScaler(), num_cols),
    ('cat', OneHotEncoder(handle_unknown='ignore'), cat_cols)
])

# Pipeline
pipe = Pipeline([
    ('preprocessor', preprocessor),
    ('scaler', StandardScaler()), 
    ('model', LogisticRegression())
])

# Fit pipeline
pipe.fit(X_train, y_train)
```

## Pipelines Inside Column Transformer
```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer

# Split into numerical, categorical data
num_cols = ['age', 'salary']
cat_cols = ['city']

# Individual pipeline for each data type
num_pipe = Pipeline([
	('imputer', SimpleImputer(strategy='mean')),
	('scaler', StandardScaler())
])

cat_pipe = Pipeline([
	('encoder', OneHotEncoder(handle_unknown='ignore')),
	('scaler', StandardScaler())
])

# Column transformer contains 2 pipelines
preprocessor = ColumnTransformer([
	('num pipe', num_pipe, num_cols),
	('cat pipe', cat_pipe, cat_cols)
], remainder='drop')

# Final step must pipeline
pipe = Pipeline([
    ('prep', preprocessor),
    ('model', LogisticRegression())
])
pipe.fit(X_train, y_train)
```



