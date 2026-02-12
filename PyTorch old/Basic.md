
---

```python
import torch
```

---

# Cuda (GPU)
```python
if torch.cuda.is_available():
	device = torch.device('cuda')
	
	# Create in GPU
	x = torch.ones(5, device=device)
	
	# Move to GPU
	x = torch.ones(5)
	x = x.to(device)
	
	# Move back to CPU
	x = x.to('cpu')
```