
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

# Training Loop, Test Loop
```python
def train_loop(dataloader, model, loss_fn, optimizer):
    size = len(dataloader.dataset)
    model.train()
    
    for batch, (X, y) in enumerate(dataloader):
        # Compute prediction, loss
        pred = model(X)
        loss = loss_fn(pred, y)

        # Backpropagation
        loss.backward()
        optimizer.step()
        optimizer.zero_grad()

		# Formatted print every 100 batch
        if batch % 100 == 0:
            loss, current = loss.item(), batch * batch_size + len(X)
            print(f'loss: {loss:>7f}  [{current:>5d}/{size:>5d}]')

def test_loop(dataloader, model, loss_fn):
    # Set model to evaluation mode (Important for batch normalization, dropout layers)
    model.eval()
    size = len(dataloader.dataset)
    num_batches = len(dataloader)
    test_loss, correct = 0, 0

    # Ensures no gradients are computed during test mode
    # Reduce unnecessary gradient computations, memory usage for tensors with `requires_grad=True`
    with torch.no_grad():
        for X, y in dataloader:
            pred = model(X)
            test_loss += loss_fn(pred, y).item()
            correct += (pred.argmax(1) == y).type(torch.float).sum().item()

    test_loss /= num_batches
    correct /= size
    print(f'Test Error: \n Accuracy: {(100*correct):>0.1f}%, Avg loss: {test_loss:>8f} \n')
```

## Usage
```python
loss_fn = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)

epochs = 10
for t in range(epochs):
    print(f'Epoch {t + 1}\n-------------------------------')
    train_loop(train_dataloader, model, loss_fn, optimizer)
    test_loop(test_dataloader, model, loss_fn)
```