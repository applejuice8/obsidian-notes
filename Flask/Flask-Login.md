
---

# Installation
```bash
pip install flask-login
```

---

# Set Up
- Need these in `extensions.py` to work
```python
from flask_login import LoginManager

login_manager = LoginManager()  # Initialize in app/__init__.py
login_manager.login_view = 'auth.login'

@login_manager.user_loader
def load_user(user_id):
	from app.models import User
	return User.query.get(int(user_id))
```

---

# User Model
- User need to inherit from `UserMixin` to use Flask-Login
```python
from flask_login import UserMixin
from werkzeug.security import generate_password_hash, check_password_hash
from app.extensions import db

class User(UserMixin, db.Model):
    __tablename__ = 'user'

    id = db.Column(
        db.Integer,
        primary_key=True,
        autoincrement=True
    )
    username = db.Column(
        db.String(100),
        nullable=False,
    )
    email = db.Column(
        db.String(40),
        unique=True,
        nullable=False
    )
    password = db.Column(
        db.String(200),
        nullable=False
    )

	# Overwrite __init__ method to hash password
    def __init__(self, username, email, password):
        self.username = username
        self.email = email
        self.set_password(password)

    def set_password(self, password):
        self.password = generate_password_hash(password)

    def check_password(self, password):
        return check_password_hash(self.password, password)

    def __repr__(self):
        return f'<User id={self.id}, username={self.username} email={self.email}>'
```

---

# Routes

## Login
```python
from flask import Blueprint, render_template, redirect, url_for, flash, request
from flask_login import login_user, logout_user, login_required, current_user
from app.extensions import db
from app.models import User, Cart
from app.forms import LoginForm, SignupForm

bp = Blueprint('auth', __name__)

@bp.route('/login', methods=['GET', 'POST'])
def login():
    if current_user.is_authenticated:
        return redirect(url_for('home.index'))
    
    form = LoginForm()
    if form.validate_on_submit():
        email = form.email.data
        password = form.password.data
        
        user = User.query.filter_by(email=email).first()
        
        if user and user.check_password(password):
            login_user(user, remember=True)
            flash('Logged in successfully!', 'success')
            return redirect(url_for('home.index'))
        flash('Invalid email or password', 'danger')

    return render_template('login.html', form=form)
```

## Signup
```python
@bp.route('/signup', methods=['GET', 'POST'])
def signup():
    if current_user.is_authenticated:
        return redirect(url_for('home.index'))
    
    form = SignupForm()
    if form.validate_on_submit():
        username = form.username.data
        email = form.email.data
        password = form.password.data
        
        # Create user
        user = User(
            username=username,
            email=email,
            password=password
        )
        db.session.add(user)
        db.session.commit()

        flash('Account created!', 'success')
        login_user(user, remember=True)
        return redirect(url_for('home.index'))
    
    return render_template('signup.html', form=form)
```

## Logout
```python
@bp.route('/logout')
@login_required
def logout():
    logout_user()
    flash('You have been logged out.', 'info')
    return redirect(url_for('auth.login'))
```


