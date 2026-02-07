
---

# Logistic Regression
![Logistic Regression](https://almablog-media.s3.ap-south-1.amazonaws.com/image_15_ea63b72d9e.png)
```python
from sklearn.linear_model import LogisticRegression

lr = LogisticRegression(
	# Penalty
	penalty='none',
	penalty='l1',  # Lasso
	penalty='l2',  # Ridge (Default)
	penalty='elasticnet',  # Mix L1, L2
	
	# Elastic-Net mixing parameter
	# 1 (Pure L1), 0 (Pure L2)
	l1_ratio=0.5,
	
	# Smaller = Stronger regularization
	# Larger (Overfit)
	c=1.0,
	
	max_iter=1000,
	random_state=42
)
```

---

# SVM (Support Vector Machine)
- Find the optimal hyperplane / decision boundary
- That maximizes the margin between classes
![Support Vector Machine](https://d2a032ejo53cab.cloudfront.net/Glossary/jP0OYWuU/SVM2.JPG)
```python
from sklearn.svm import SVC

svc = SVC(
	kernel='linear',
	kernel='rbf',  # Nonlinear
	kernel='poly',
	kernel='sigmoid'
	
	# Regularization parameter
	# High (Small margin, may overfit)
	C=1.0,
	
	random_state=42
)
```

## Kernel Trick
- Enables SVM to classify non-linear data by implicitly mapping input data into a higher-dimensional feature space
![Kernel Trick](https://cml.rhul.ac.uk/images/projects/mapping.png)

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

# KNN (K Nearest Neighbors)
![K Nearest Neighbors](https://cdn.educba.com/academy/wp-content/uploads/2020/01/Nearest-Neighbors-Algorithm-2.png)
```python
from sklearn.neighbors import KNeighborsClassifier

knc = KNeighborsClassifier(n_neighbors=3)  # Default = 5
```

---

# Decision Tree
- By default, no maximum (Tree expands until all leaves pure)
- Setting limit avoid overfitting
![Decision Tree](https://www.geo.fu-berlin.de/en/v/geo-it/gee/3-classification/3-1-methodical-background/3-1-1-cart/dectree.png?width=1000)
```python
from sklearn.tree import DecisionTreeClassifier

dtc = DecisionTreeClassifier(
	max_depth=None,
	random_state=42
)
```

---

# Random Forest
- Ensemble of decision trees
![Random Forest](https://media.geeksforgeeks.org/wp-content/uploads/20251216121929631349/random_forest_algorithm.webp)
```python
from sklearn.ensemble import RandomForestClassifier

rfc = RandomForestClassifier(
	n_estimators=100,  # Number of decision trees
	max_depth=None,
	random_state=42
)
```

---

# Gradient Boosting
1. Start with a model to predict
2. Measure error between actual, predicted
3. Train new model to predict the error
4. Add new model to improve prediction
5. Repeat
![Gradient Boosting](https://www.ibm.com/content/adobe-cms/us/en/think/topics/gradient-boosting/jcr:content/root/table_of_contents/body-article-8/image_180162990.coreimg.png/1763387523973/ensemble-learning-boosting.png)
```python
from sklearn.ensemble import GradientBoostingClassifier

gbc = GradientBoostingClassifier(
	n_estimators=100,
	learning_rate=0.1,
	random_state=42
)
```