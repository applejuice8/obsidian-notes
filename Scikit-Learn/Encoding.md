
---

# Label Encoder
- Unranked data
- Convert each category into a unique integer
- Even though no ranking, models may interpret as rankings, safer to use OHE on X
- Only use on target labels (y)
![Label Encoder](https://media.geeksforgeeks.org/wp-content/uploads/20250724112756132446/categorical_data_encoding_2.webp)
```python
from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()
df['Color'] = le.fit_transform(df['Color'])
```

---

# One Hot Encoder
- Separate column for each value
![One Hot Encoder](https://miro.medium.com/v2/resize:fit:1400/1*cnmpSdK-6hAQJdTUBFxcnA.png)
```python
from sklearn.preprocessing import OneHotEncoder

ohe = OneHotEncoder(
	handle_unknown='ignore',
	sparse_output=False  # Doesn't store 0 (Save memory)
).set_output(transform='pandas')

encoded = ohe.fit_transform(df[['Color']])

# Drop original, add encoded
df = df.drop(columns=['Color']).join(encoded)
```

---

# Ordinal Encoder
- Data with order
- Assign numbers alphabetically by default (’L’ = 0, ‘M’ = 1, ‘S’ = 2)
- Specify order (`categories=[['Small', 'Medium', 'Large']]`)
![Ordinal Encoder](https://substackcdn.com/image/fetch/$s_!hXQt!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4752d819-8d28-4f62-a2f8-ab07af9ed48e_1431x702.jpeg)
```python
from sklearn.preprocessing import OrdinalEncoder

# Get all sizes
sizes = df['size'].unique()

# Determine the order
oe = OrdinalEncoder(categories=[['Small', 'Medium', 'Large']])
df['Size'] = oe.fit_transform(df[['Size']])
```