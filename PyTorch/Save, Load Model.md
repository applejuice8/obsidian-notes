
---

# Model's State Dict
- All weights, biases
```python
class SimpleModel(nn.Module):
    def __init__(self):
        super().__init__()
        self.linear1 = nn.Linear(2, 8)
        self.relu = nn.ReLU()
        self.linear2 = nn.Linear(8, 1)

    def forward(self, x):
        x = self.linear1(x)
        x = self.relu(x)
        x = self.linear2(x)
        return x
```

```python
linear1.weight torch.Size([8, 2])
linear1.bias torch.Size([8])
linear2.weight torch.Size([1, 8])
linear2.bias torch.Size([1])
```

## Method 1 (Only Save State Dict)
- Call `model.eval()` because only use for test, no need to train anymore
```python
model = SimpleModel()

# Save
torch.save(model.state_dict(), 'model.pth')

# Load (Need recreate architecture)
model = SimpleModel()
model.load_state_dict(torch.load('model.pth', weights_only=True))
model.eval()

# Only load matching params
model.load_state_dict(
	torch.load('model.pth', weights_only=True),
	strict=False
)
```

## Method 2 (Save Entire Model)
- Uses Python's `pickle` module
```python
model = SimpleModel()

# Save
torch.save(model, 'model.pth')

# Load
model = torch.load('model.pth', weights_only=False)
model.eval()
```

## Method 3 (Saving Checkpoint)
- Continue training next time
```python
# Save
torch.save({
	'epoch': epoch,
	'model_state_dict': model.state_dict(),
	'optimizer_state_dict': optimizer.state_dict(),
	'loss': loss
}, 'model.pth')

# Load
model = SimpleModel()
checkpoint = torch.load('model.pth', weights_only=True)
model.load_state_dict(checkpoint['model_state_dict'])
optimizer.load_state_dict(checkpoint['optimizer_state_dict'])
epoch = checkpoint['epoch']
loss = checkpoint['loss']

# Train or test
model.eval()
model.train()
```

---

# Interchange Save, Load on CPU, GPU
- `torch.save(model.state_dict(), 'model.pth')` same for all
```python
# Save on GPU, Load on GPU
device = torch.device('cuda')
model = SimpleModel()
model.load_state_dict(torch.load('model.pth', weights_only=True))
model.to(device)

# Save on GPU, Load on CPU
device = torch.device('cpu')
model = SimpleModel()
model.load_state_dict(torch.load(
	'model.pth',
	map_location=device,
	weights_only=True
))

# Save on CPU, Load on GPU
device = torch.device('cuda')
model = SimpleModel()
model.load_state_dict(torch.load(
	'model.pth',
	weights_only=True,
	map_location='cuda:0'
))
model.to(device)
```


