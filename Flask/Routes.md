
---

# Multiple Same Routes
```python
@app.route('/')
@app.route('/home')
@app.route('/index')
def home():
	return 'Hello World!'
```

---

# Variables
- `str` don't need `<str:>`
- `path` accepts `/` symbol, while `str` doesn't
```python
from flask import render_template

@app.route('/demo/<name>/<int:age>/<float:price>/<uuid:token>')
def demo(name: str, age: int, price: float, token: UUID):
	return render_template('index.html', name=name, age=age)

@app.route('/demo2/<path:filepath>/<any(red,green,blue):color>')
def demo2(filepath: str, color: str):
	return render_template('index.html')
```

---

# Redirect
- Not hardcoded
- Follow function name
- `url_for('func_name')` or `url_for('bp_name.func_name')`
```python
from flask import render_template, redirect, url_for

@app.route('/greet/<name>')
def greet(name):
	return render_template('index.html', name=name)

@app.route('/user/<name>')
def user(name):
	if name =='admin':
		return redirect(url_for('greet', name=name))
```

```jinja2
{# signup() function in auth blueprint #}
<a href="{{ url_for('auth.signup') }}">Sign Up</a>
```

---

# Request Methods
- `PUT` updates completely
- `PATCH` updates partially
```python
@app.route('/', methods=['GET', 'POST', 'PUT', 'DELETE'])
def home():
    if request.method == 'POST':
	    return render_template('success.html')
	return render_template('index.html')
```

---

# Before Request
```python
# Check if user logged in
@app.before_request
def check_login():
    # Skip for login page itself
    if request.endpoint == 'login':
        return

    if 'user' not in session:
        return redirect(url_for('login'))

# Website under maintenance
@app.before_request
def block_site():
    return "Site under maintenance", 503
```