![Flask](https://www.vectorlogo.zone/logos/palletsprojects_flask/palletsprojects_flask-icon~v2.svg)

---
[Reference](https://hackersandslackers.com/series/build-flask-apps/)

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
	# General
	ENVIRONMENT = os.getenv('ENVIRONMENT')
	FLASK_APP = os.getenv('FLASK_APP')
	FLASK_DEBUG = os.getenv('FLASK_DEBUG')
	SECRET_KEY = os.getenv('SECRET_KEY')
	
	# Folders
	STATIC_FOLDER = 'static'
    TEMPLATES_FOLDER = 'templates'

    # Database
    SQLALCHEMY_DATABASE_URI = os.getenv('SQLALCHEMY_DATABASE_URI')
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```

```python
# wsgi.py
from flask import Flask

app = Flask(__name__)
app.config.from_object('config.Config')
```

---

# Application Factory
- Instead of 1 `app.py`, entire `application/` folder is app
```bash
/app
├── /application
│   ├── /static
│   └── /templates
│   ├── __init__.py  # Creation of app
│   ├── auth.py
│   ├── forms.py
│   ├── models.py
│   └── routes.py
├── config.py
└── wsgi.py  # App gateway
```

## application/`__init__.py`
```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_redis import FlaskRedis

db = SQLAlchemy()
r = FlaskRedis()

def init_app():
    app = Flask(__name__)
    app.config.from_object('config.Config')

    # Initialize plugins
    db.init_app(app)
    r.init_app(app)

    with app.app_context():
        # Include routes
        from . import routes
        
        # Create db tables
        db.create_all()

        # Register blueprints
        app.register_blueprint(auth.auth_bp)
        app.register_blueprint(admin.admin_bp)

        return app
```

## wsgi.py (App's Entry Point)
```python
from application import init_app

app = init_app()

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```

---

# Blueprints
```python
from flask import Blueprint

home_bp = Blueprint(
    'home_bp', __name__,
    template_folder='templates',
    static_folder='static'
)

@home_bp.route('/')
def home():
	return
```

## Register Blueprints in `__init__.py`
```python
from flask import Flask

def init_app():
    app = Flask(__name__)
    app.config.from_object('config.Config')

    with app.app_context():
        # Register blueprints
        app.register_blueprint(auth.auth_bp, prefix='/auth')
        app.register_blueprint(admin.admin_bp, prefix='/admin')

        return app
```

## In Jinja
```jinja2
<a href="{{ url_for('home_bp.home') }}">Home</a>
```

---

# SQLAlchemy
```python
# models.py
from . import db

class User(db.Model):
	__tablename__ = 'user'
    id = db.Column(
        db.Integer,
        primary_key=True
    )
    
    # Different with non-flask SQLAlchemy
	id: Mapped[int] = mapped_column(Integer)
```

---

# Auth

## Models
- Models which inherit UserMixin gains access to 4 methods `.is_authenticated`, `is_active`, `is_anonymous`, `get_id`
```python
from . import db
from flask_login import UserMixin
from werkzeug.security import generate_password_hash, check_password_hash

class User(UserMixin, db.Model):
	__tablename__ = 'user'
    id = db.Column(
        db.Integer,
        primary_key=True
    )
    
	def set_password(self, password):
        self.password = generate_password_hash(
            password,
            method='sha256'
        )

    def check_password(self, password):
        return check_password_hash(self.password, password)
```

## Signup
```python
from flask import flash, request, session, url_for
from flask_login import login_required, logout_user, current_user, login_user

# Signup
@auth_bp.route('/signup', methods=['GET', 'POST'])
def signup():
    form = SignupForm()
    if form.validate_on_submit():
        existing_user = User.query
						    .filter_by(email=form.email.data)
						    .first()
        if existing_user is None:
            user = User(
                name=form.name.data,
                email=form.email.data,
                website=form.website.data
            )
            user.set_password(form.password.data)
            db.session.add(user)
            db.session.commit()
            login_user(user)
            return redirect(url_for('main_bp.dashboard'))
        flash('A user already exists with that email address.')

    return render_template('signup.jinja2', form=form)
```

## Login
```python
@auth_bp.route('/login', methods=['GET', 'POST'])
def login():
    # If user is logged in
    if current_user.is_authenticated:
        return redirect(url_for('main_bp.dashboard'))

	# Validate login attempt
    form = LoginForm()
    if form.validate_on_submit():
        user = User.query
			        .filter_by(email=form.email.data)
			        .first()
        if user and user.check_password(form.password.data):
            login_user(user)
            next_page = request.args.get('next')
            return redirect(
	            next_page or url_for('main_bp.dashboard')
			)
        flash('Invalid username/password combination')
        return redirect(url_for('auth_bp.login'))
    
    return render_template('login.jinja2', form=form)
```

## Login Helpers
```python
# Check if user is logged-in on every page load
@login_manager.user_loader
def load_user(user_id):
    if user_id is not None:
        return User.query.get(user_id)
    return None

# Redirect unauthorized
@login_manager.unauthorized_handler
def unauthorized():
    flash('You must be logged in to view that page.')
    return redirect(url_for('auth_bp.login'))
```

## Login Required
```python
from flask_login import current_user, login_required

@main_bp.route('/', methods=['GET'])
@login_required
def dashboard():
	return
```

## Logout
```python
@main_bp.route("/logout")
@login_required
def logout():
    logout_user()
    return redirect(url_for('auth_bp.login'))
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
	{{ form.username(placeholder='Adam') }}  {# Attributes #}
	
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

# Flask RESTful
```python
from flask import Flask
from flask_restful import Api, Resource

app = Flask(__name__)
api = Api(app)

users = {
    1: {'name': 'Alice'},
    2: {'name': 'Bob'}
}

class User(Resource):
    def get(self, user_id):
        return users.get(user_id, 'User not found'), 200

    def put(self, user_id):
        users[user_id] = {'name': 'Updated'}
        return {'message': 'User updated'}

    def delete(self, user_id):
        users.pop(user_id, None)
        return {'message': 'User deleted'}

api.add_resource(User, '/users/<int:user_id>')

if __name__ == '__main__':
    app.run(debug=True)
```
