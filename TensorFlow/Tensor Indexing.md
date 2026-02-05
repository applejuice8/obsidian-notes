
---

```python
t = tf.constant([
    [10, 20, 30],
    [40, 50, 60],
    [70, 80, 90]
])

# 3rd row, 2nd col
t[2, 1]

# Entire row
t[1]
t[1, :]

# Entire col
t[:, 2]
t[:, 2:4]

# Row 0 to 1, Col 1 to 2
t[0:2, 1:3]
```

# tf.gather()
```python
# [10, 30, 50]
t = tf.constant([10, 20, 30, 40, 50])
tf.gather(t, [0, 2, 4])

t = tf.constant([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

tf.gather(t, [0, 2], axis=0)  # Rows
tf.gather(t, [0, 2], axis=1)  # Cols


```

## batch_dims, gather_nd
```python
t = tf.constant([
    [10, 11, 12],
    [20, 21, 22],
    [30, 31, 32]
])

# batch_dims
indices = tf.constant([
    [0, 2],  # Row 1 (Index 0 and 2)
    [1, 1],  # Row 2 (2 Index 1)
    [2, 0]   # Row 3 (Index 2 and 0)
])
tf.gather(t, indices, axis=1, batch_dims=1)

# gather_nd
indices = tf.constant([
    [0, 2],  # t[0][2] = 12
    [1, 1],  # t[1][1] = 21
    [2, 0]   # t[2][0] = 30
])
tf.gather_nd(t, indices)

# 2 approaches same
indices = [
	[2],
	[1]
]
tf.gather(t, indices, axis=1, batch_dims=1)

indices = [
    [0, 2],
    [1, 1]
]
tf.gather_nd(t, indices)
```

---

# Boolean Mask
- Filter elements in tensor

## Filter
```python
t = tf.constant([10, 20, 30, 40])

# [30, 40]
tf.boolean_mask(t, t > 20)
```

## Filter and Flatten
```python
t = tf.constant([
    [1, 2, 3, 0],
    [4, 5, 0, 0]
])

# [1, 2, 3, 4, 5]
tf.boolean_mask(t, t != 0)
```

## Filter Without Flatten
```python
t = tf.constant([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

# First and third rows
tf.boolean_mask(t, [True, False, True])
```

---

# tf.where()
- Replace elements in tensor based on condition
- `tf.where(condition, if_true, if_false)`

## Replace Elements
```python
t = tf.constant([
    [1, 4, 3],
    [4, 5, 6]
])

tf.where(t > 3, t, 0)
# [[0, 0, 0],
#  [4, 5, 6]]
```

## Return Indices Where True
```python
x = tf.constant([
    [1, 4],
    [5, 2]
])

tf.where(x > 3)
# [[0, 1],
#  [1, 0]]
```

---

# tf.scatter_nd()

## 1D
```python
indices = [[0], [2], [4]]
updates = [10, 20, 30]

tf.scatter_nd(indices, updates, shape=[6])
# Start with 6 zeros
# Add 10 to index 0, 20 to index 2

# [10, 0, 20, 0, 30, 0]
```

## 2D
```python
indices = [
    [0, 1],
    [1, 0],
    [1, 2]
]
updates = [5, 6, 7]

tf.scatter_nd(indices, updates, shape=[3, 3])
```

---

# Reshape

