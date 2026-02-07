
---

# Accuracy
```python
model.score(X_test, y_test)  # 0.79
```

---

# Confusion Matrix
![Confusion Matrix](https://i0.wp.com/glassboxmedicine.com/wp-content/uploads/2019/02/confusion-matrix.png?fit=1200%2C675&ssl=1)
```python
from sklearn.metrics import confusion_matrix

y_pred = model.predict(X_test)
cm = confusion_matrix(y_test, y_pred)
```

---

# Classification Report
![Classification Report](https://substackcdn.com/image/fetch/$s_!hfB2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe9ff0a9d-b70d-4abe-bbf3-8e06d4eb13c1_3084x784.png)
```python
from sklearn.metrics import classification_report

y_pred = model.predict(X_test)
cr = classification_report(y_test, y_pred)
```