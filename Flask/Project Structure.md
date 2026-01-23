
---

# Project Structure
```bash
├── app/
│    ├── __init__.py  # Application factory, Blueprint registration
│    ├── api.py  # API endpoints used by JavaScript
│    ├── auth.py
│    ├── errors.py  # Error handlers
│    ├── extensions.py  # Initializes Flask extensions
│    ├── forms.py  # Flask WTForms
│    ├── home.py
│    ├── models/  # Database models
│    │    ├── __init__.py
│    │    └── user.py
│    ├── static/
│    │    ├── css/
│    │    ├── images/
│    │    └── js/
│    └── templates/
│         ├── base.html
│         ├── index.html
│         └── errors/
├── config.py  # Application configuration
├── instance
│    └── app.db
├── README.md
├── requirements.txt
└── wsgi.py  # WSGI entry point for running the app
```

---

# Individual Files

## config.py
- Application configuration
```python
import os
from dotenv import load_dotenv

load_dotenv()

class Config:
    # General
    FLASK_APP = 'wsgi.py'
    FLASK_DEBUG = True
    SECRET_KEY = os.getenv('SECRET_KEY')

    # Database
    SQLALCHEMY_DATABASE_URI = os.getenv('SQLALCHEMY_DATABASE_URI')
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```

## wsgi.py
- WSGI entry point for running the app
```python
from app import create_app

app = create_app()

if __name__ == '__main__':
	app.run()
```

## extensions.py
- Initiate Flask extensions (SQLAlchemy, Redis, CSRF)
```python
from flask_sqlalchemy import SQLAlchemy
from flask_login import LoginManager
from flask_wtf import CSRFProtect

db = SQLAlchemy()
login_manager = LoginManager()
csrf = CSRFProtect()

login_manager.login_view = 'auth.login'

@login_manager.user_loader
def load_user(user_id):
    from app.models import User
    return User.query.get(int(user_id))
```

## errors.py
- Handle error codes
```python
from flask import render_template
from werkzeug.exceptions import HTTPException

def register_error_handlers(app):
	@app.errorhandler(404)
	def not_found(e):
		return render_template('errors/404.html'), 404
	
	@app.errorhandler(500)
	def internal_error(e):
		return render_template('errors/500.html'), 500
	
	# Others
	@app.errorhandler(HTTPException)
	def handle_http_exception(e):
		return render_template(
			'errors/error.html',
			code=e.code,
			name=e.name,
			description=e.description,
		), e.code
```

## app/`__init__.py`
- Application factory
- Blueprint registration
```python
from flask import Flask
from app.extensions import db, login_manager, csrf
from app.errors import register_error_handlers

def create_app():
	app = Flask(__name__)
	app.config.from_object('config.Config')
	
	# Initialize plugins
	db.init_app(app)
	login_manager.init_app(app)
	csrf.init_app(app)
	
	from app.models import User, Product, Cart, CartItem
	
	with app.app_context():
		# Create db tables
		db.create_all()
		
		# Register blueprints
		from .home import bp as home_bp
		from .auth import bp as auth_bp
		from .cart import bp as cart_bp
		from .api import bp as api_bp
		
		app.register_blueprint(home_bp)
		app.register_blueprint(auth_bp, url_prefix='/auth')
		app.register_blueprint(cart_bp, url_prefix='/cart')
		app.register_blueprint(api_bp, url_prefix='/api')
		
		register_error_handlers(app)
		
		return app
```

## Routes
- More at [[Routes]]
```python
from flask import Blueprint, render_template

bp = Blueprint('home', __name__)

@bp.route('/')
def index():
    return render_template('index.html')
```

## forms.py
- Flask WTForms
- More at [[Flask-WTForms]]
```python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, SubmitField, 

class LoginForm(FlaskForm):
    email = StringField('Email')
    password = PasswordField('Password')
    submit = SubmitField('Log In')
```

## Database Models
- More at [[SQLAlchemy]]
```python
from app.extensions import db

class Cart(db.Model):
    __tablename__ = 'cart'

    id = db.Column(
        db.Integer,
        primary_key=True,
        autoincrement=True
    )
    user_id = db.Column(
        db.Integer,
        db.ForeignKey('user.id'),
        nullable=False
    )

    def __repr__(self):
        return f'<Cart id={self.id}, user_id={self.user_id}>'
```

## models/`__init__.py`
```python
from .user import User
from .product import Product
from .cart import Cart
from .cart_item import CartItem

__all__ = [
    'User',
    'Product',
    'Cart',
    'CartItem'
]
```
