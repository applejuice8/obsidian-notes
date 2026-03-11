
---

# Initialize Tensors

## Preset Values
```python
# Hardcode
x = torch.tensor([[1, 2],[3, 4]])

# Specified shape
shape = (2, 3)  # 2x3
x = torch.zeros(5, 3)
x = torch.rand(shape)
x = torch.ones(shape)
x = torch.zeros(shape)
```

## Random Tensors
```python
torch.manual_seed(1729)
r1 = torch.rand(2, 2)
r2 = torch.rand(2, 2)

# Same as r1
torch.manual_seed(1729)
r3 = torch.rand(2, 2)
```

## From Another Tensor
- Retains shape, data type
```python
x = torch.empty_like(x)
x = torch.zeros_like(x)
x = torch.ones_like(x)
x = torch.rand_like(x)

# Overrides data type
x = torch.rand_like(x, dtype=torch.float)
```

## To / From Numpy
- Tensors on CPU, NumPy arrays share underlying memory locations
- Changing 1 changes both
```python
# Tensor to numpy
t = torch.ones(5)
n = t.numpy()

# Numpy to tensor
n = np.ones(5)
t = torch.from_numpy(n)

# Changes both
t.add_(1)
```

---

# Data Types
- Default `torch.float32`
```python
# Default float32
x = torch.zeros((5, 3), dtype=torch.int16)
```

---

# Mathematical Operations

## Element-wise Arithmetic
- Only if tensors same size
```python
ones = torch.ones(2, 3)
twos = ones * 2
threes = twos + 1
```

## Tensor Broadcasting
- Align dimensions from right side (Not left)
- `torch.ones(4, 3, 2)` aligned with `torch.ones(3, 2)` (Same as `torch.ones(0, 3, 2)`)
![Tensor Broadcasting](https://jidindinesh.github.io/img/broadcasting.png)
```python
rand = torch.rand(2, 4)
doubled = rand * (torch.ones(1, 4) * 2)
```

## Mathematical Operations
```python
# Absolute value
torch.abs(x)

# Standard deviation, mean
torch.std(x)
torch.mean(x)
torch.std_mean(x)  # Tuple (std, mean)

# Maximum
torch.max(x)

# Round up
torch.ceil(x)
torch.floor(x)

# Equal
torch.eq(x, y)

# Determinant
torch.det(x)

# Singular value decomposition
torch.svd(x)
```

## Inplace
```python
x = torch.det(x)

# Inplace of current tensor
x.add_(y)

# Inplace of another existing tensor
torch.add(x, y, out=z)
x.add(y, out=z)
```

---

# Copy Tensor
```python
# Shallow copy
y = x

# Copy
y = x.clone()

# Copy without autograde
y = x.detach().clone()
```

---

# Tensor Attributes
```python
x = torch.rand(3, 4)

x.shape   # torch.Size([3, 4])
x.dtype   # torch.float32
x.device  # 'cpu'

x.item()  # If only 1 element, get absolute value
```

---

# Move Tensors to Accelerator (GPU)
```python
# Create tensor in GPU
my_device = torch.accelerator.current_accelerator() if torch.accelerator.is_available() else torch.device('cpu')

x = torch.rand(2, 2, device=my_device)

# Move tensor to GPU
if torch.accelerator.is_available():
    tensor = tensor.to(
	    torch.accelerator.current_accelerator()
	)
```

---

# Change Number of Dimensions

## Squeezing, Unsqueezing
- Singletons (Dimensions of size 1)
```python
# Remove all singletons
x = torch.squeeze(x, dim=None)
# (2, 1, 3, 1) -> (2, 3)

# Remove that dimension 1 if it is singleton
x = torch.squeeze(x, dim=1)
# (2, 1, 1) -> (2, 1)
# (1, 2, 3) -> (1, 2, 3)

# Add singleton
x = torch.unsqueeze(x, 0)
# (4, 3, 4) -> (1, 4, 3, 4)

# Add singleton at specific dimension
x = torch.unsqueeze(x, 2)
# (4, 3, 4) -> (4, 3, 1, 4)

# Inplace versions
torch.squeeze_(x, dim=1)
torch.unsqueeze(x, 2)
```

## Reshape
```python
output3d = torch.rand(6, 20, 20)

# Method 1
input1d = output3d.reshape(6 * 20 * 20)

# Method 2
input1d = torch.reshape(output3d, (6 * 20 * 20,))
```

---

# Concatenate Tensors
```python
y = torch.cat([x, x, x], dim=1)

# Before
x = tensor([[1., 0., 1.],
	        [1., 0., 1.],
	        [1., 0., 1.],
	        [1., 0., 1.]])

# After
y = tensor([[1., 0., 1., 1., 0., 1., 1., 0., 1.],
	        [1., 0., 1., 1., 0., 1., 1., 0., 1.],
	        [1., 0., 1., 1., 0., 1., 1., 0., 1.],
	        [1., 0., 1., 1., 0., 1., 1., 0., 1.]])
```