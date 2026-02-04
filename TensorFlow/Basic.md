
---

```python
import tensorflow as tf
```

---

# Tensors
```python
import tensorflow as tf 

# Create tensor
tensor = tf.constant([[1, 2, 3], [4, 5, 6]])

# Shape
tensor.shape  # (2, 3)

# Size (Total elements)
tf.size(tensor)  # tf.Tensor(6, shape=(), dtype=int32)
```

## Tensor Rank
```python
# Rank 0 tensor (Scalar)
scalar = tf.constant(5)

# Rank 1 tensor (Vector)
vector = tf.constant([1, 2, 3])

# Rank 2 tensor (Matrix)
matrix = tf.constant([[1, 2], [3, 4]])  

# Rank 3 tensor (3D)
tensor_3d = tf.constant([
	[[1, 2], [3, 4]],
	[[5, 6], [7, 8]]
])

tf.rank(scalar)
tf.rank(vector)
tf.rank(matrix)
tf.rank(tensor_3d)
```

---

# Tensor Indexing
