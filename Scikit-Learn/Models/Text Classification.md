
---

# Count Vectorizer
- Builds a vocabulary of unique words, count how many times each word appear in each text
- OHE for words, with counts instead of just binary
- Vocabulary example = {free, money, now, win, prize}
- Frequent words (’the’, ‘is’, ‘to’) appear everywhere, dominate the counts but don’t help classification, thus need TF-IDF
![Count Vectorizer](https://cdn.hashnode.com/res/hashnode/image/upload/v1696827074664/cd9af0b6-594f-49db-8f2a-a41d8bb53dde.png)

---

# TF-IDF (Term Frequency – Inverse Document Frequency)
- Improves on raw counts by giving importance scores to words
- TF (Words that appear a lot in a document are important)
- IDF (Words that appear in every document probably not useful)
- Word ‘free’ appear in both document, less useful
![TF-IDF](https://assets.zilliz.com/TF_IDF_table_62a64f4dc1.png)

---

# Code
- `ngram_range` allows detection of multi-words phrases like 'free money'
- `min_df` drops rare tokens, since they are likely typos
- `max_df` drops overly common words
- `stop_words='english'` removes standardized list of common words (this, is, the)
- `binary=True` only checks for presence / absence instead of count (Good for spam)
- `sublinear_tf=True` uses `1 + log(tf)` instead of raw count (Prevent long spam messages dominating)
- `norm='l2'` normalizes document vectors, works well with linear models
```python
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer

# Count
CountVectorizer(
    ngram_range=(1, 1),  # Unigrams only
    ngram_range=(1, 2),  # Unigrams, Bigrams
    
    min_df=2,
    max_df=0.95,
    
    stop_words='english',
    binary=True
)

# TF-IDF
TfidfVectorizer(
	# Other params same
    sublinear_tf=True,  # Use 1 + log(tf) 
    
    norm='l2',
    norm=None
)
```

---

# Good Models for Text Classification
```python
MultinomialNB(alpha=1.0)
LogisticRegression(max_iter=1000)
```

---

# Print Out Vectors
```python
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer

# Example dataset
texts = [
	'free money now',
	'win free prize',
	'hello friend'
]

# Count Vectorizer
cv = CountVectorizer()
X_counts = cv.fit_transform(texts)
print('Vocabulary: ', cv.get_feature_names_out())
print('Vectors:\n ', X_counts.toarray())

# TF-IDF Vectorizer (TF-IDF)
tfidf = TfidfVectorizer()
X_tfidf = tfidf.fit_transform(texts)
print('Vocabulary: ', tfidf.get_feature_names_out())
print('Vectors:\n ', X_tfidf.toarray())
```