
---

# Gradient Boosting
1. Start with a model to predict
2. Measure error between actual, predicted
3. Train new model to predict the error
4. Add new model to improve prediction
5. Repeat
![Gradient Boosting](https://www.ibm.com/content/adobe-cms/us/en/think/topics/gradient-boosting/jcr:content/root/table_of_contents/body-article-8/image_180162990.coreimg.png/1763387523973/ensemble-learning-boosting.png)
```python
from sklearn.ensemble import GradientBoostingClassifier, GradientBoostingRegressor

gbc = GradientBoostingClassifier(
	n_estimators=100,
	learning_rate=0.1,
	random_state=42
)

gbr = GradientBoostingRegressor()
```

---

# Histogram-Based Gradient Boosting (HistGB)



