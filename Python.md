![Python](https://cdn.svglogos.dev/logos/python.svg)

---

# Interactive Mode
```bash
python
>>> 10 + 10
>>> _ + 10  # Refers to last printed expression (20)
```

---

# String
```python
# Print quotes
"I'm here"
'I\'m here'
'My name is "Jason"'
"My name is \"Jason\""

# Raw string
print(r'C:\some\name')

# Multi-line
print('''\  # Skip new line
Hello
''')

# String multiplication
print(3 * 'hi' + 'abc')  # hihihiabc

# Length
len(text)

# Concatenate string
text = 'Hello ' + 'World'

# String automatically concatenated
text = ('Hello '
        'World')  # Hello World
```

---

# Indexing
```python
word = 'Python'

word[0]  # First
word[-1]  # Last
```

# Slicing
```python
word[0:2]  # Index 0 (Included) to 2 (Excluded)

word[:2]  # Beginning to index 2 (Excluded)
word[4:]  # Index 4 (Included) to end
word[-3:]  # Third last to end (hon)

# Return same string if same number
word[:2] + word[2:]
word[:4] + word[4:]
```

---

# Lists
- Like strings, can index, slice, concat
```python
res = [1, 2, 3, 4, 5]

res[0]
res[1:3]
merged = res + [6, 7, 8]
len(res)
res.append(6)

# Nested list
res = [[1, 2, 3], [4, 5, 6]]
res[0][1]
```

---

# If Statements
```python
while a < 10:
	a += 1

if x < 0:
	pass
elif x < 10:
	pass
else:
	pass

for p in ['Jason', 'Jack', 'Adam']:
	pass
```




