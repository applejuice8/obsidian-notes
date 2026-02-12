
---

- `Dataset` stores samples, their corresponding labels
- `DataLoader` wraps an iterable around `Dataset` to enable easy access
```python
from torch.util.data import Dataset, DataLoader
```

---

# FashionMNIST Dataset
- `root` is file path
- `download=True` downloads file if not downloaded
- `transform`, `target_transform` specify feature, label transformations
```python
from torch.utils.data import Dataset
from torchvision import datasets
from torchvision.transforms import ToTensor

training_data = datasets.FashionMNIST(
    root='data',
    train=True,
    download=True,
    transform=ToTensor()
)

test_data = datasets.FashionMNIST(
    root='data',
    train=False,
    download=True,
    transform=ToTensor()
)

# Can index
img, label = training_data[i]
```

---

# Create Custom Dataset
- Must implement 3 functions (`__init__`, `__len__`, `__getitem__`)
- `__len__` returns number of samples
- `decode_image()` converts images to tensors
```python
from torchvision.io import decode_image

class CustomImageDataset(Dataset):
    def __init__(self, annotations_file, img_dir, transform=None, target_transform=None):
        self.img_labels = pd.read_csv(annotations_file)
        self.img_dir = img_dir
        self.transform = transform
        self.target_transform = target_transform

    def __len__(self):
        return len(self.img_labels)

    def __getitem__(self, idx):
        img_path = os.path.join(self.img_dir, self.img_labels.iloc[idx, 0])
        image = decode_image(img_path)
        label = self.img_labels.iloc[idx, 1]
        if self.transform:
            image = self.transform(image)
        if self.target_transform:
            label = self.target_transform(label)
        return image, label
```

---

# DataLoaders
- `Dataset` retrieves 1 sample at a time
- `DataLoader` pass samples in mini batches, reshuffle at each epoch
```python
from torch.utils.data import DataLoader

train_dataloader = DataLoader(
	training_data, batch_size=64, shuffle=True
)
test_dataloader = DataLoader(
	test_data, batch_size=64, shuffle=True
)
```

---

# Transforms
- All TorchVision datasets have 2 parameters `transform`, `target_transform`
- `transform` modifies features, `target_transform` modify labels
- `ToTensor()` converts PIL image or NumPy array into FloatTensor
- `ToTensor()` also scales image pixel intensity values in range of [0., 1.]
```python
import torch.nn.functional as F
from torchvision import datasets
from torchvision.transforms import ToTensor, Lambda

ds = datasets.FashionMNIST(
    root='data',
    train=True,
    download=True,
    transform=ToTensor(),
    target_transform=Lambda(
        lambda y: F.one_hot(torch.tensor(y), num_classes=10).float()
    )
)
```