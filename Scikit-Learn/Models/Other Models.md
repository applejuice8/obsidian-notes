
---

- Other than Naive Bayes, models have 2 types (Similar parameters)
- Classifier (Discrete data), Regressor (Continuous data)

---

# Naive Bayes Classifier
- GaussianNB (Features continuous, normally distributed)
- MultinomialNB (Counts, Frequencies)
- CategoricalNB (Features not numeric, More than 2 categories)
- BernoulliNB (Features are binary)
![Naive Bayes Classifier](https://miro.medium.com/0*J5MLEJwQqhW41wg6.png)
```python
from sklearn.naive_bayes import GaussianNB, MultinomialNB, CategoricalNB, BernoulliNB

gnb = GaussianNB()
mnb = MultinomialNB(alpha=1.0)
cnb = CategoricalNB(alpha=1.0)
bnb = BernoulliNB(alpha=1.0)
```

## Laplace Smoothing (Alpha)
- Avoids 0 probabilities when feature doesn’t appear in training data
- Prevent model from assigning 0 probability to unseen events
- Even if word never appeared, it gets small probability (Default = `1e-9`)

---

# SVM (Support Vector Machine)
- Find the optimal hyperplane / decision boundary
- That maximizes the margin between classes
- C is regularization parameter (High means small margin, may overfit)
![Support Vector Machine](https://d2a032ejo53cab.cloudfront.net/Glossary/jP0OYWuU/SVM2.JPG)
```python
from sklearn.svm import SVC, SVR

svc = SVC(
	kernel='linear',
	kernel='rbf',  # Nonlinear
	kernel='poly',
	kernel='sigmoid'
	
	C=1.0,
	random_state=42
)

svr = SVR()
```

## Kernel Trick
- Enables SVM to classify non-linear data by implicitly mapping input data into a higher-dimensional feature space
![Kernel Trick](https://cml.rhul.ac.uk/images/projects/mapping.png)

---

# KNN (K Nearest Neighbors)
![K Nearest Neighbors](https://cdn.educba.com/academy/wp-content/uploads/2020/01/Nearest-Neighbors-Algorithm-2.png)
```python
from sklearn.neighbors import KNeighborsClassifier, KNeighborsRegressor

knc = KNeighborsClassifier(n_neighbors=3)  # Default = 5
knr = KNeighborsRegressorr()
```

---

# Decision Tree
- By default, no maximum (Tree expands until all leaves pure)
- Setting limit avoid overfitting
![Decision Tree](https://www.geo.fu-berlin.de/en/v/geo-it/gee/3-classification/3-1-methodical-background/3-1-1-cart/dectree.png?width=1000)
```python
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor

dtc = DecisionTreeClassifier(
	max_depth=None,
	random_state=42
)

dtr = DecisionTreeRegressor()
```

---

# Random Forest
- Ensemble of decision trees
![Random Forest](https://media.geeksforgeeks.org/wp-content/uploads/20251216121929631349/random_forest_algorithm.webp)
```python
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor

rfc = RandomForestClassifier(
	n_estimators=100,  # Number of decision trees
	max_depth=None,
	random_state=42
)

rfr = RandomForestRegressor()
```

---

# MLP (Multi-Layer Perceptron)
![Multi-Layer Perceptron](https://cdn-images-1.medium.com/max/800/0*eaw1POHESc--l5yR.png)
```python
from sklearn.neural_network import MLPClassifier

model = MLPClassifier(
	# Number of neurons in each hidden layer
    hidden_layer_sizes=(100,),
    hidden_layer_sizes=(100, 50),
    hidden_layer_sizes=(100, 64, 32),
    
    activation='relu',
    
    # Optimizer
    solver='adam',
    solver='sgd',
    solver='lbfgs',  # For small data
    
    learning_rate_init=0.001,
    alpha=0.001  # L2 regularization strength
    max_iter=200,
    random_state=42,
    verbose=True  # Prints progress to terminal
)
```

## Plot Loss Curve
```python
fig, axes = plt.subplots(1, 1)
axes.plot(mlp.loss_curve_, 'o-')
axes.set_xlabel("number of iteration")
axes.set_ylabel("loss")
plt.show()
```

