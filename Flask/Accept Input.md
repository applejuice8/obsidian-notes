
---

# URL Path Parameters (GET)
- http://127.0.0.1:5500/ `/users/14`
```python
@app.route('/users/<int:user_id>')
def get_user(user_id: int):
	return
```

# Query Parameters (GET)
- http://127.0.0.1:5500 `/search?q=flask&page=2`
```python
from flask import request

@app.route('/search')
def search():
    q = request.args.get('q')
    page = request.args.get('page', default=1, type=int)
```

# Form Data (POST)
- Follows `name` attribute
```python
from flask import request

@app.route('/login', methods=['POST'])
def login():
    username = request.form.get('username')
    password = request.form.get('password')
```

```html
<form method="POST" action="/login">
	<input name="username">
	<input name="password" type="password">
</form>
```

# request.values (GET + POST)
- Combine `request.args` + `request.form`
```python
from flask import request

@app.route('/submit', methods=['GET', 'POST'])
def submit():
    value = request.values.get('value')
```

# JSON Body
- Called by JavaScript
- Frontend to Backend communication
```python
from flask import request, jsonify

@app.route('/api/users', methods=['POST'])
def create_user():
    data = request.get_json()
    username = data.get('username')
```

```javascript
fetch('/api/users', {
	method: 'POST',
	headers: { 'Content-Type': 'application/json' },
	body: JSON.stringify({ username: 'adam' })
});
```

# File Uploads
```python
from flask import request

@app.route('/upload', methods=['POST'])
def upload():
    f = request.files.get('file')
    f.save(f'./uploads/{f.filename}')
```

```html
<form method="POST" enctype="multipart/form-data">
	<input type="file" name="file">
</form>
```

---

# Less Common

## Headers
```python
from flask import request

@app.route('/agent')
def agent():
    user_agent = request.headers.get('User-Agent')
```

## Cookies
```python
from flask import request

@app.route('/theme')
def theme():
    theme = request.cookies.get('theme', 'light')
```