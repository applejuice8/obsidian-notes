
---

```python
from torch.nn.functional import binary_cross_entropy_with_logits

x = torch.tensor([1, 1, 1, 1, 1])  # Input tensor
y = torch.tensor([0, 0, 0])  # Expected output

# Random weights (5 inputs to 3 outputs each)
w = torch.randn(5, 3, requires_grad=True)

# Random biases (Added to output neuron)
b = torch.randn(3, requires_grad=True)

# The formula for model to find weights, biases
z = torch.matmul(x, w) + b

# Loss function
loss = binary_cross_entropy_with_logits(z, y)
```

![Computational Graph](https://docs.pytorch.org/tutorials/_images/comp-graph.png)

## Backpropagation
```python
# Backpropagation
loss.backward()

# Gradients (Derivatives of loss function with respect to parameters)
print(w.grad)  # dloss / dw
print(b.grad)  # dloss / db

'''
tensor([[0.3055, 0.2443, 0.3029],
        [0.3055, 0.2443, 0.3029],
        [0.3055, 0.2443, 0.3029],
        [0.3055, 0.2443, 0.3029],
        [0.3055, 0.2443, 0.3029]])

tensor([0.3055, 0.2443, 0.3029])
'''
```

---

# Disable Gradient Tracking
- Computations on tensors that do not track gradients more efficient
```python
x = torch.rand(3, requires_grad=True)

# Method 1
x.requires_grad_(False)

# Method 2
y = x.detach()

# Method 3
with torch.no_grad():
	y = x + 2  # No grad_fn=<AddBackward0>
```
