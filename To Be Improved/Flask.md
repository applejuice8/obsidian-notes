![Flask](https://www.vectorlogo.zone/logos/palletsprojects_flask/palletsprojects_flask-icon~v2.svg)

---
[Reference](https://hackersandslackers.com/series/build-flask-apps/)


---

---


---

# Request Object
```python
from flask import request
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
        self.password = generate_password_hash(password)

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
