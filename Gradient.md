
---

```python
x = torch.rand(3, requires_grad=True)
# tensor([0.1, 0.2, 0.3], requires_grad=True)

y = x + 2
# tensor([2.1, 2.2, 2.3], grad_fn=<AddBackward0>)

z = y * y
# tensor([4.41, 4.84, 5.29], grad_fn=<MulBackward0>)

z = z.mean()
# tensor(4.8467, grad_fn=<MeanBackward0>)

# dz/dx (Gradient of z with respect to x)
z.backward()
x.grad  # tensor([0.0160, 3.3650, 4.5153])
```