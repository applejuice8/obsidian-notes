
---

# K Means Clustering
- Sensitive to outliers
- n_clusters = Number of clusters to find

## Steps
1. Initialize k centroids
2. Assign each point to the nearest centroid
3. Recompute centroids as the mean of assigned points
4. Repeat until centroids stabilize
![K Means Clustering](https://miro.medium.com/1*fz-rjYPPRlGEMdTI-RLbDg.png)
```python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=3, random_state=42)
```

---

# Agglomerative Clustering
- Bottom-up hierarchical clustering

## Steps
1. Each point start as its own cluster
2. Merge closest cluster until enough n_clusters
![Agglomerative Clustering](https://media.geeksforgeeks.org/wp-content/uploads/20251117144305409428/agglomerative_clustering.webp)
```python
from sklearn.cluster import AgglomerativeClustering

agg = AgglomerativeClustering(n_clusters=3)
```

---

# DBSCAN (Density-Based Spatial Clustering of Applications with Noise)
- Density based clustering
- No need to specify number of clusters
- Can find arbitrarily shaped clusters (No restrictions)
- Handles outliers (Noise)
- `eps` (Maximum distance between points to consider them in same neighbourhood)
- `min_samples` (Minimum number of points required to form a core point)

## Steps
1. For each point, check its neighbourhood within eps radius
2. If a point has `≥ min_samples` neighbours, it’s a core point
3. Core points close to each other form clusters, Points not reachable are noise
![DBSCAN](https://media.geeksforgeeks.org/wp-content/uploads/20250912174514326431/4.webp)
![Arbitrarily shaped clusters](https://miro.medium.com/0*z09DnDHJPkMKkz70)
```python
from sklearn.cluster import DBSCAN

dbscan = DBSCAN(eps=0.5, min_samples=5)
```

---

# Mean Shift Clustering
- No need to specify number of clusters
- Can find irregular shape clusters (Non standard shape, but usually still compact)

## Steps
1. Initialize centroids at every point / subset of points
2. Shift each centroid to the mean of points within a window (Bandwidth)
3. Merge centroids that converge to the same location into clusters
![Mean Shift](https://media.geeksforgeeks.org/wp-content/uploads/20190429213154/1354.png)
```python
from sklearn.cluster import MeanShift

meanshift = MeanShift()
```

---

# Gaussian Mixture Clustering
- Produces soft clusters (Points have probabilities of belonging to each cluster)
- `n_components` (Number of Gaussian distributions)
- Each component models 1 cluster in the mixture
![Gaussian Mixture](https://media.geeksforgeeks.org/wp-content/uploads/20250905101108541893/GMM.webp)
```python
from sklearn.mixture import GaussianMixture

gmm = GaussianMixture(n_components=3, random_state=42)
```