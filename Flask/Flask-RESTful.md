- Automatically `jsonify` the dictionaries

---

# Installation
```bash
pip install flask-restful
```

---

# Project Structure
```bash
flask_restful_app/
├── app/
│   ├── __init__.py  # create_app()
│   ├── models.py
│   ├── resources.py  # Flask-RESTful resources
│   └── extensions.py  # db, api initialization
├── run.py  # Entry point
└── requirements.txt
```

## run.py
```python
from app import create_app

app = create_app()

if __name__ == '__main__':
    app.run(debug=True)
```

## extensions.py
```python
from flask_sqlalchemy import SQLAlchemy
from flask_restful import Api

db = SQLAlchemy()
api = Api()
```

## models.py
```python
from .extensions import db

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), nullable=False)
    age = db.Column(db.Integer, nullable=False)

    def to_dict(self):
        return {'id': self.id, 'name': self.name, 'age': self.age}
```

## `__init__.py`
```python
from flask import Flask
from .extensions import db, api
from .resources import UserResource, UserListResource

def create_app():
    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

    # Initialize extensions
    db.init_app(app)

    # Register resources
    from .resources import bp as api_bp
    app.register_blueprint(api_bp, url_prefix='/api')

    # Create tables
    with app.app_context():
        db.create_all()

    return app
```

## resources.py
- If don't want to use `reqparse`, can use `request.get_json()`
```python
from flask import request, Blueprint
from flask_restful import Resource, Api, reqparse
from .models import User
from .extensions import db

bp = Blueprint('api', __name__)
api = Api(bp)

# Request parser for User
user_parser = reqparse.RequestParser()
user_parser.add_argument(
	'name',
	type=str,
	required=True,
	help='Name is required'
)
user_parser.add_argument(
	'age',
	type=int,
	required=True,
	help='Age is required'
)

# Single user (GET, PUT, DELETE)
class UserResource(Resource):
    def get(self, user_id: int):
        user = User.query.get(user_id)
        if not user:
            return {'message': 'User not found'}, 404
        return user.to_dict()

    def put(self, user_id: int):
        user = User.query.get(user_id)
        if not user:
            return {'message': 'User not found'}, 404
        args = user_parser.parse_args()
        user.name = args.get('name')
        user.age = args.get('age')
        db.session.commit()
        return user.to_dict()

    def delete(self, user_id: int):
        user = User.query.get(user_id)
        if not user:
            return {'message': 'User not found'}, 404
        db.session.delete(user)
        db.session.commit()
        return '', 204

# List of users (GET all, POST)
class UserListResource(Resource):
    def get(self):
        users = User.query.all()
        return [user.to_dict() for user in users]

    def post(self):
        args = user_parser.parse_args()
        user = User(name=args.get('name'), age=args.get('age'))
        db.session.add(user)
        db.session.commit()
        return user.to_dict(), 201
        
# Register resources with API
api.add_resource(UserListResource, '/users')
api.add_resource(UserResource, '/users/<int:user_id>')
```
