![Flask](https://www.vectorlogo.zone/logos/palletsprojects_flask/palletsprojects_flask-icon~v2.svg)

---

# Commands
```bash
pip install flask

# Run
flask run
```

---

# Simple
```python
from flask import Flask


app = Flask(__name__)

@app.route('/')
def hello_world():
	return 'Hello World'
	
if __name__ == '__main__':
	app.run(debug=True)
```

## Default Arguments
```python
app = Flask(
    __name__,
    instance_relative_config=False,
    template_folder='templates',
    static_folder='static'
)
```

---

# API Response
```python
from flask import make_response, jsonify

@app.route('/')
def hello():
	# Without header
	return make_response('Malformed request', 400)
	
	# With header
	headers = {"Content-Type": "application/json"}
    return make_response('it worked!', 200, headers)
    
    # Jsonify for dict
    my_dict = {'key': 'dictionary value'}
    headers = {"Content-Type": "application/json"}
    return make_response(jsonify(my_dict), 200, headers)
```

---

# Routes

## Multiple Routes
```python
@app.route('/')
@app.route('/home')
@app.route('/index')
def home():
	return 'Hello World!'
```

## Variables
- `str` don't need `<str:>`
- `path` accepts `/`, while `str` doesn't
```python
from flask import render_template

@app.route('/demo/<name>/<int:age>/<float:price>/<uuid:token>')
def demo(name: str, age: int, price: float, token: UUID):
	return render_template('index.html', name=name, age=age)

@app.route('/demo2/<path:filepath>/<any(red,green,blue):color>')
def demo2(filepath: str, color: str):
	return render_template('index.html')
```

## Redirect
- Not hardcoded
- Follow function name
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

---

# Request Object
```python
from flask import request
```

## request.method
- `PUT` updates completely
- `PATCH` updates partially
```python
@app.route('/', methods=['GET', 'POST', 'PUT', 'DELETE'])
def home():
    if request.method == 'POST':
	    return render_template('success.html')
	return render_template('index.html')
```

## request.args (Query Params GET)
- https...`/search?q=python&page=3`
```python
@app.route('/search')
def search():
    q = request.args.get('q', '')
    page = request.args.get('page', 1)
```

## request.form (Form Data POST)
```python
@app.route('/login', methods=['POST'])
def login():
    username = request.form['username']
    password = request.form.get('password')  # Safer
```

## request.values (GET & POST)
- Works for both GET, POST
- GET takes precedence over POST if same key
```python
@app.route('/demo', methods=['GET','POST'])
def demo():
    name = request.values.get('name')
    return f"Hello {name}"
```

## request.files
```python
@app.route('/upload', methods=['POST'])
def upload():
    file = request.files.get('photo')
    file.save(f'uploads/{file.filename}')
```

## request.json
```python
@app.route('/api/user', methods=['POST'])
def api_user():
    data = request.json
    name = data.get('name')
    age = data.get('age')
```

## request.path, full_path, url
- Path (`/search`)
- Full path (`/search?q=flask`)
- url (`https://localhost:5000/search?q=flask`)
```python
@app.route('/path')
def path():
    p = request.path
    full_p = request.full_path
    url = request.url
```

## request.headers, cookies, remote_addr (Infos)
- Headers (HTTP headers)
- Cookies / Sessions
- remote_addr (Client IP)
```python
@app.route('/info')
def info():
    ua = request.headers.get('User-Agent')
    session = request.cookies.get('session')
    ip = request.remote_addr
```

---

# Decorators

## Before Request
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

## Handle HTTP Error Codes
```python
# 404 Not found
@app.errorhandler(404)
def not_found():
    return make_response(
	    render_template('404.html'),
	    404
	)

# 400 Bad request
@app.errorhandler(400)
def bad_request():
    """Bad request."""
    return make_response(
        render_template('400.html'),
        400
    )

# 500 Internal server error
@app.errorhandler(500)
def server_error():
    return make_response(
        render_template('500.html'),
        500
    )
```

---

# Config
```bash
# .env
ENVIRONMENT = 'development'
FLASK_APP = 'my-app'
FLASK_DEBUG = True
SECRET_KEY = 'GDtfD^&$%@^8tgYjD'
```

```python
# config.py
import os
from dotenv import load_dotenv

load_dotenv()

class Config:
	ENVIRONMENT = os.get_env('ENVIRONMENT')
	FLASK_APP = 'my-app'
	FLASK_DEBUG = True
	SECRET_KEY = 'GDtfD^&$%@^8tgYjD'
```

```python
# app.py
from flask import Flask

app = Flask(__name__)
app.config.from_object('config.Config')
```














---

# Flask WTForms
- Prevent CSRF attacks
```bash
pip install flask-wtf
```

```python
from flask import Flask
from flask_wtf import CSRFProtect


app = Flask(__name__)
app.config['SECRET_KEY'] = 'dev-secret-key'
csrf = CSRFProtect(app)
```

## Form
```python
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired, Length

class LoginForm(FlaskForm):
    username = StringField(
        'Username',
        validators=[DataRequired(), Length(min=3, max=20)]
    )
    submit = SubmitField('Login')
```

## Form in Routes
- Change `methods` to handle form
- `validate_on_submit()` same as `if request.method == 'POST' and form.validate()`
```python
@app.route('/login', methods=['GET', 'POST'])
def login():
    form = LoginForm()

    if form.validate_on_submit():
        username = form.username.data
        return redirect(url_for('index'), username=username)
    return render_template('login.html', form=form)
```

```jinja2
<form method="POST">
    {{ form.hidden_tag }}  {# form.csrf_token #}

	{{ form.username.label }}
	{{ form.username() }}

    {{ form.submit() }}
</form>
```

## Form Fields
- `(value, display)` such as `('IN', 'India')`
- User clicks `'India'`, `'IN'` sent to server
- Can add `message='error message'` to all
- `RecaptchaField` need `RECAPTCHA_PUBLIC_KEY` and `RECAPTCHA_PRIVATE_KEY` from Google API docs
```python
from flask_wtf import FlaskForm, RecaptchaField
from wtforms import StringField, PasswordField, BooleanField, *

class LoginForm(FlaskForm):
    # Text inputs
    name = StringField('Name', message='Name is required')
    username = StringField('Username')
    password = PasswordField('Password')
    email = EmailField('Email')
    website = URLField('Website')
    search = SearchField('Search')
    phone = TelField('Phone')
    message = TextAreaField('Message')

    # Numbers
    age = IntegerField('Age')
    height = FloatField('Height')
    salary = DecimalField('Salary')

    # Boolean
    remember_me = BooleanField('Remember Me')

    # Choice fields
    gender = RadioField(
        'Gender',
        choices=[
	        ('male', 'Male'),
	        ('female', 'Female')
		]
    )
    country = SelectField(
        'Country',
        choices=[
            ('IN', 'India'),
            ('US', 'United States'),
            ('UK', 'United Kingdom')
        ]
    )
    hobbies = SelectMultipleField(
        'Hobbies',
        choices=[
            ('sports', 'Sports'),
            ('music', 'Music'),
            ('coding', 'Coding')
        ]
    )

    # Date, time
    birth_date = DateField('Birth Date')
    meeting_time = TimeField('Meeting Time')
    appointment = DateTimeField('Appointment')

    # Files
    photo = FileField('Photo')
    documents = MultipleFileField('Documents')

    # Hidden
    next_page = HiddenField()
    
    # reCAPTCHA
    recaptcha = RecaptchaField(message='Human check failed')

    # Submit
    submit = SubmitField('Submit')
```

## Usage in Routes
```python
@app.route('/login', methods=['GET', 'POST'])
def login():
    form = LoginForm()
    if form.validate_on_submit():
	    name = form.name.data
        password = form.password.data
        return f'Hi {name}'
    return render_template('index.html', form=form)
```

## Usage in HTML
```jinja2
<form method="post" action="/">
	{{ form.hidden_tag() }}  {# form.csrf_token #}
	
	{{ form.name.label }}
	{{ form.name() }}
	<br>
	{{ form.password.label }}
	{{ form.password() }}
	<br>
	{{ form.submit }}
</form>
```

## Validators
- `DataRequired()` for most fields
- `InputRequired()` when falsy values are valid (0, False)
```python
from wtforms.validators import Optional, DataRequired, *
from flask_wtf.file import FileRequired, FileAllowed

class LoginForm(FlaskForm):
	StringField(
		'Name', 
		validators=[
			Optional(),  # Must first
			DataRequired(),
			InputRequired(),
			Length(min=3, max=20),
			NumberRange(min=18, max=65),
			Email(),
			URL(),
			Regexp(r'^[a-zA-Z0-9_]+$'),
			AnyOf(['US', 'UK']),
			NoneOf(['admin', 'root'])
		]
	)

	# Files
	photo = FileField(
        'Photo',
        validators=[
            FileRequired(),
            FileAllowed(['jpg', 'png'])
        ]
    )

	# Check equal
	password = PasswordField('Password')

    confirm = PasswordField(
        'Confirm Password',
        validators=[EqualTo('password')]
    )
```

## Custom Validators
```python
class LoginForm(FlaskForm):
    username = StringField('Username')
    password = PasswordField('Password')
    confirm = PasswordField('Confirm Password')
    submit = SubmitField('Submit')

    # Field validator
    def validate_username(self, field):
        if field.data.lower() == 'admin':
            raise ValidationError('Username "admin" is reserved')

    # Form-level validator
    def validate(self, extra_validators=None):
	    # Run field validator first
        if not super().validate(extra_validators):
            return False

        if self.password.data != self.confirm.data:
            self.confirm.errors.append('Passwords do not match')
            return False
        return True
```

## Render Errors
```jinja2
<form method="POST">
	{{ form.hidden_tag() }}
	
	<div>
		{{ form.username.label }}<br>
		{{ form.username() }}
		
		{# Render errors for specific field #}
		{% for error in form.username.errors %}
			<span>{{ error }}</span>
		{% endfor %}
	</div>

	{# Render errors for all fields #}
	<ul>
	    {% for field, errors in form.errors.items() %}
	        {% for error in errors %}
	            <li>{{ field }}: {{ error }}</li>
	        {% endfor %}
	    {% endfor %}
	</ul>
	
	{# Render form-level errors #}
	<ul>
		{% for error in form.errors.get('__all__', []) %}
			<li>Form error: {{ error }}</li>
		{% endfor %}
	</ul>

	{{ form.submit() }}
</form>
```

---


