
---

```python
# Empty tensors (Garbage values)
torch.empty(2)
torch.empty(3, 2, 4)  # 4 inside 2 inside 3

# Random (0 to 1)
torch.rand(3, 2, 4)

# Zero
torch.zeros(2, 3)

# From list
torch.tensor([1, 2, 3])
```

# Data Types
- Default `float64`
```python
x = torch.ones(2, 2, dtype=torch.int)
print(x.dtype)
```

# Methods
```python
x.dtype  # torch.float32
x.size()  # torch.Size([2, 3])

# To, from numpy
y = x.numpy()
x = torch.from_numpy(y)
```

# Arithmetic
```python
z = x + y
z = torch.add(x, y)
z = torch.sub(x, y)
z = torch.mul(x, y)
z = torch.div(x, y)

# Underscore behind means inplace
y.add_(x)
```

# Tensor Indexing
```python
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

# If only 1 item, use .item() for value
t[1, 1].item()
```

# Reshape
- Total number must same
```python
x = torch.rand(4, 4)
y = x.view(16)

# -1 PyTorch determine how many
y = x.view(-1)
y = x.view(-1, 3)  # 3 cols, PyTorch decides how many rows
y = x.view(3, -1)  # 3 rows, PyTorch decides how many cols
```