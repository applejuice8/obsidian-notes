
![Scaling](https://blog.alliedoffsets.com/hubfs/1_hP0vIkTucGjspPootFrS_w.webp)

---

# Min-Max Scaling
- Scale values to fixed range [0, 1]
- Preserves distribution shape
![Formula](https://miro.medium.com/v2/resize:fit:888/1*ye1I00S61GqpR34ABZZFLQ.png)
![Min-Max Scaling](https://static-assets.codecademy.com/Paths/data-science-career-path/MachineLearning/outlier.png)
```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler(feature_range=(0,1))
X_scaled = scaler.fit_transform(X)
```

---

# Max-Abs Scaling
- Scale values to [-1, 1]
![Formula](https://miro.medium.com/v2/resize:fit:1400/1*FGviCA_gRRNljXCeHexncg.png)
```python
from sklearn.preprocessing import MaxAbsScaler

scaler = MaxAbsScaler()
X_scaled = scaler.fit_transform(X)
```

---

# Standardization
- Transform data to have mean = 0, standard deviation = 1
![Formula](https://miro.medium.com/v2/resize:fit:740/1*Nlgc_wq2b-VfdawWX9MLWA.png)
![Standardization](https://miro.medium.com/v2/resize:fit:1204/0*BGymFtZoq3DamXYD)
```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

---

# Robust Scaling
- Similar to standardization
- Uses median, IQR (Instead of mean, standard deviation)
- Works well with outliers
![Formula](https://miro.medium.com/v2/resize:fit:1400/0*1lDcAWaRG0wkfBQF.png)
```python
from sklearn.preprocessing import RobustScaler

scaler = RobustScaler()
X_scaled = scaler.fit_transform(X)
```

