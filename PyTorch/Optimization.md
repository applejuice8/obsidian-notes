
---

# Optimizer
```python
optimizer = torch.optim.SGD(
	model.parameters(),
	lr=learning_rate
)
```

---

# Hyperparameters
- Epochs (Number of times to iterate over dataset)
- Batch size (Number of data samples propagated through the network before parameters are updated)
- Learning rate (How much to update parameters at each batch / epoch)

---

# Optimization Loop
- Each iteration of optimization loop called epoch

## Each epoch consists of 2 main parts
- Train loop (Iterate over training dataset, try to converge to optimal parameters)
- Validation / Test loop (Iterate over test dataset, check if model performance improving)

## Inside training loop, optimization happens in 3 steps
- `optimizer.zero_grad()` resets gradients of model parameters
- By default, gradients add up (To prevent double-counting, we explicitly zero them at each iteration)
- `loss.backward()` backpropagates the prediction loss (PyTorch deposits the gradients of the loss with respect to each parameter)
- Once we have our gradients, `optimizer.step()` adjust the parameters by the gradients collected in the backward pass

---

# Loss Function
- Measures the degree of dissimilarity of obtained result to the target value
- We want to minimize loss function during training
- Compare prediction against true data label value

---
