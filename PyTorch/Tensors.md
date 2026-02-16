
---

# Initialize Tensors

## Preset Values
```python
# Hardcode
x = torch.tensor([[1, 2],[3, 4]])

# Default float32
x = torch.zeros(5, 3)
x = torch.zeros((5, 3), dtype=torch.int16)



# Specified shape
shape = (2, 3)  # 2x3
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

## Element-wise Arithmetic
```python

```

## From Another Tensor
- Retains shape, data type
```python
x = torch.ones_like(x)

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
if torch.accelerator.is_available():
    tensor = tensor.to(
	    torch.accelerator.current_accelerator()
	)
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