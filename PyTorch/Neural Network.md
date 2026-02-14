
---

# Train Model on Accelerator
```python
device = torch.accelerator.current_accelerator().type
		if torch.accelerator.is_available()
		else 'cpu'

# Move model to GPU
model = NeuralNetwork().to(device)
```

---

# Define Model
- `model(X)` automatically calls `.forward()` method, don't call directly
```python
from torch import nn

class NeuralNetwork(nn.Module):
    def __init__(self):
        super().__init__()
        self.flatten = nn.Flatten()
        self.linear_relu_stack = nn.Sequential(
            nn.Linear(28*28, 512),
            nn.ReLU(),
            nn.Linear(512, 512),
            nn.ReLU(),
            nn.Linear(512, 10),
        )

    def forward(self, x):
        x = self.flatten(x)
        logits = self.linear_relu_stack(x)
        return logits
```

---

# Layers
```python
seq_modules = nn.Sequential(
    nn.Flatten(),  # [3, 28, 28] -> [3, 784]
    nn.Linear(in_features=28*28, out_features=20),
    nn.ReLU(),
    nn.Linear(20, 10)
)

# 3 images of size 28x28
input_image = torch.rand(3, 28, 28)
logits = seq_modules(input_image)
nn.Softmax(dim=1)(logits)  # Scale values to [0, 1]
```

---

# Parameters (Weights, Biases)
```python
print(f'Model structure: {model}')

for name, param in model.named_parameters():
    print(f'Layer: {name}
		    Size: {param.size()}
		    Values : {param[:2]}')
```

## Example
```python
Model structure: NeuralNetwork(
  (flatten): Flatten(start_dim=1, end_dim=-1)
  (linear_relu_stack): Sequential(
    (0): Linear(in_features=784, out_features=512, bias=True)
    (1): ReLU()
    (2): Linear(in_features=512, out_features=512, bias=True)
    (3): ReLU()
    (4): Linear(in_features=512, out_features=10, bias=True)
  )
)

Layer: linear_relu_stack.0.weight | Size: torch.Size([512, 784]) | Values : tensor([[ 0.0039,  0.0304, -0.0297,  ..., -0.0167, -0.0301,  0.0256],
        [-0.0337,  0.0312,  0.0150,  ..., -0.0112,  0.0306, -0.0156]],
       device='cuda:0', grad_fn=<SliceBackward0>)

Layer: linear_relu_stack.0.bias | Size: torch.Size([512]) | Values : tensor([-0.0265,  0.0143], device='cuda:0', grad_fn=<SliceBackward0>)

Layer: linear_relu_stack.2.weight | Size: torch.Size([512, 512]) | Values : tensor([[ 0.0385, -0.0276, -0.0122,  ..., -0.0349, -0.0117, -0.0272],
        [ 0.0094, -0.0210, -0.0223,  ...,  0.0071, -0.0212, -0.0390]],
       device='cuda:0', grad_fn=<SliceBackward0>)

Layer: linear_relu_stack.2.bias | Size: torch.Size([512]) | Values : tensor([-0.0125,  0.0110], device='cuda:0', grad_fn=<SliceBackward0>)

Layer: linear_relu_stack.4.weight | Size: torch.Size([10, 512]) | Values : tensor([[ 0.0434,  0.0026, -0.0181,  ..., -0.0277, -0.0019,  0.0109],
        [ 0.0429, -0.0313,  0.0167,  ..., -0.0130, -0.0136,  0.0196]],
       device='cuda:0', grad_fn=<SliceBackward0>)

Layer: linear_relu_stack.4.bias | Size: torch.Size([10]) | Values : tensor([0.0172, 0.0092], device='cuda:0', grad_fn=<SliceBackward0>)
```