![FastAPI](https://cdn.svglogos.dev/logos/fastapi-icon.svg)

---

# Installation
```bash
pip install "fastapi[standard]"
```

---

# FastAPI Project Structure
```bash
course-enrollment/
├── app/
│   ├── core/
│   │   ├── config.py  # Config variables (DB URL, secrets)
│   │   └── security.py  # Authentication, password hashing
│   ├── db/
│   │   ├── database.db
│   │   └── database.py  # Engine, SessionLocal creation
│   ├── models/  # SQLAlchemy ORM table models
│   │   ├── course.py
│   │   ├── enrollment.py
│   │   └── student.py
│   ├── routers/  # API endpoints
│   │   ├── course.py
│   │   ├── enrollment.py
│   │   └── student.py
│   ├── schemas/  # Pydantic models for validation
│   │   ├── course.py
│   │   ├── enrollment.py
│   │   └── student.py
│   ├── services/  # Business logic, CRUD operations
│   │   ├── student_service.py
│   │   ├── course_service.py
│   │   └── enrollment_service.py
│   ├── dependencies.py  # FastAPI dependencies (get_db)
│   └── main.py   # FastAPI app instance, routers registration
├── tests/  # Unit, integration tests
│   ├── test_course.py
│   ├── test_enrollment.py
│   ├── test_main.py
│   └── test_student.py
├── .env
├── .gitignore
├── fast_commit.bash
├── README.md
└── requirements.txt
```
[Structuring a FastAPI Project: Best Practices](https://dev.to/mohammad222pr/structuring-a-fastapi-project-best-practices-53l6)

---

# Order of Operations
1. `.env`
2. core/`config.py` & `security.py`
3. db/`database.py`
4. `dependencies.py`
5. Models
6. Schemas
7. Services
8. Routers
9. Main
10. db/`init_db.py` (Only run once)
11. Tests

## Request Data Flow
1. Client makes a request to API (JSON body)
2. `FastAPI + Pydantic` validates input, produces `StudentCreate schema`
3. `Router` calls `create_student() service`, passing `StudentCreate schema` into it
4. `Service` converts schema to SQLAlchemy ORM model (`db_course=Course(...)`), then performs database operations
5. `Pydantic` converts ORM to `StudentGet schema` (This line of code `from_attributes=True` in GET schemas triggered by FastAPI `response_model=StudentGet` in routers)
6. `FastAPI` converts Schema to JSON (Using JSON encoder), then returns to client

---

# Run Live Server
```bash
uvicorn app.main:app --reload

python -m app.db.init_db  # Run specific file only
```

---

# 1. .env
- Store environment variables (DB URL, API keys)
```bash
DATABASE_URL='sqlite:///./app/db/database.db'
```

# 2a. core/config.py
- Load configuration from `.env`
```python
from pydantic import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str
    debug: bool = False

    class Config:
        env_file = '.env'

settings = Settings()
```

## Manual Non-Pydantic Approach
```python
import os
from dotenv import load_dotenv

load_dotenv()
DATABASE_URL = os.getenv('DATABASE_URL')
```

---

# 2b. core/security.py
- Hash / Verify passwords
- Generate / Verify JWT tokens
```python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=['argon2'], deprecated='auto')

def hash_pw(pw: str) -> str:
	return pwd_context.hash(pw)

def verify_pw(plain_pw: str, hashed_pw: str) -> bool:
	return pwd_context.verify(plain_pw, hashed_pw)
```

---

# 3. db/database.py
- Setup SQLAlchemy engine, session
- `echo=True` SQL query logging (Prints every SQL statement executed by SQLAlchemy to console)
- `autocommit=False` need to manually call `db.commit()`
- `autoflush=False` If true, can query after adding, even if haven't commit
```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, DeclarativeBase
from app.core.config import settings

engine = create_engine(settings.database_url, echo=True)

SessionLocal = sessionmaker(
	autocommit=False,
	autoflush=False,
	bind=engine
)

class Base(DeclarativeBase):
	pass
```

# 4. dependencies.py
- To be used in `Depends(get_db)` in routers
- All shared dependencies centralized, keep routes clean
```python
from app.db.database import SessionLocal

def get_db():
	db = SessionLocal()
	try:
		yield db
	finally:
		db.close()
```

---

# 5. Models
- Define database tables using SQLAlchemy ORM
- Tables, Columns, Data types, Relationships
```python
from sqlalchemy import Integer, String
from app.db.database import Base
from sqlalchemy.orm import Mapped, mapped_column

class Course(Base):
	__tablename__ = 'course'
	
	id: Mapped[int] = mapped_column(
		primary_key=True,
		autoincrement=True
	)
	code: Mapped[str] = mapped_column(
		String(10),
		unique=True,
		nullable=False
	)
	
	# toString method
	def __repr__(self):
		return f'<Course(id={self.id}, code={self.code}, name={self.name}, credit_hours={self.credit_hours})>'
```

# 6. Schemas
- Data validation, serialization using Pydantic models
- Ensure correct fields, data types
- Sent by users to backend (Create, Update, Delete)
- Sent by backend to users (Get) need `model_config = ConfigDict(from_attributes=True)`
```python
from pydantic import BaseModel, ConfigDict, Field

class CourseBase(BaseModel):
	code: str = Field(
		...,
		title='Course Code',
		pattern='^[A-Za-z]{3}[0-9]{4}$',
		example='FEL1234'
	)
	
	name: str = Field(
		...,
		title='Course Name',
		min_length=10,
		max_length=50,
		example='Functional Programming Principles'
	)
	
	credit_hours: int = Field(
		...,
		title='Credit Hours',
		description='Number of credit hours per week',
		gt=0,
		lt=10,
		example=4
	)

class CourseCreate(CourseBase):
	pass

class CourseGet(CourseBase):
	id: int = Field(
		...,
		title='Unique primary key for each course',
		description='Unique primary key for each course auto-generated by database',
		gt=0,
		example=3
	)
	model_config = ConfigDict(from_attributes=True)

class CourseUpdate(BaseModel):
	code: str | None = Field(
		None,
		title='Course Code',
		pattern='^[A-Za-z]{3}[0-9]{4}$',
		example='FEL1234'
	)
```

# 7. Services
- Business logic
- Database CRUD operations
```python
from http.client import HTTPException
from sqlalchemy.orm import Session
from app.models.course import Course
from app.schemas.course import CourseCreate, CourseUpdate

def create_course(db: Session, course: CourseCreate):
	db_course = Course(
		code=course.code,
		name=course.name,
		credit_hours=course.credit_hours
	)
	db.add(db_course)
	db.commit()
	db.refresh(db_course)
	return db_course
	
def get_course(db: Session, course_id: int = None):
	if course_id:
		db_course = db.query(Course).filter_by(id=course_id).first()
		if not db_course:
			raise HTTPException(status_code=404, detail='Course not found')
		return db_course
	return db.query(Course).all()

def update_course(
	db: Session,
	course_id: int,
	course: CourseUpdate
):
	db_course = db.query(Course).filter_by(id=course_id).first()
	if not db_course:
		raise HTTPException(status_code=404, detail='Course not found')
	
	if course.code is not None:
		db_course.code = course.code
	if course.name is not None:
		db_course.name = course.name
	if course.credit_hours is not None:
		db_course.credit_hours = course.credit_hours
	
	db.commit()
	db.refresh(db_course)
	return db_course

def delete_course(db: Session, course_id: int):
	db_course = db.query(Course).filter_by(id=course_id).first()
	if not db_course:
		raise HTTPException(status_code=404, detail='Course not found')
	db.delete(db_course)
	db.commit()
```

# 8. Routers
- Define API endpoints (URL paths)
```python
from fastapi import APIRouter, Depends, Path
from sqlalchemy.orm import Session
from app.schemas.course import CourseCreate, CourseGet, CourseUpdate
from app.services import course_service
from app.dependencies import get_db

router = APIRouter(prefix='/courses', tags=['courses'])

# Custom validation
def validate_course_id(
	course_id: int = Path(..., gt=0, description='Course ID must be positive')
) -> int:
	return course_id

# Create
@router.post('/', response_model=CourseGet)
def create_course(
	course: CourseCreate,
	db: Session = Depends(get_db)
):
	return course_service.create_course(db, course)

# Read
@router.get('/', response_model=list[CourseGet])
def get_courses(db: Session = Depends(get_db)):
	return course_service.get_course(db)

@router.get('/{course_id}', response_model=CourseGet)
def get_course(
	course_id: int = Depends(validate_course_id),
	db: Session = Depends(get_db)
):
	return course_service.get_course(db, course_id)

# Update
@router.patch('/{course_id}', response_model=CourseGet)
def update_course(
	course: CourseUpdate,
	course_id: int = Depends(validate_course_id),
	db: Session = Depends(get_db)
):
	return course_service.update_course(db, course_id, course)

# Delete
@router.delete('/{course_id}', status_code=204)
def delete_course(
	course_id: int = Depends(validate_course_id),
	db: Session = Depends(get_db)
):
	course_service.delete_course(db, course_id)
```

---

# 9. Main
- Creates the FastAPI application
- Connects all routes into app
```python
from fastapi import FastAPI
from app.routers import course, enrollment, student

app = FastAPI()

app.include_router(course.router)
app.include_router(enrollment.router)
app.include_router(student.router)
```

# 10. db/init_db.py (Only Run Once)
- Only run once to create all tables in database
- If `database.db` doesn't exist, will create
- `Base.metadata` keeps registry of all ORM models that inherit from `Base`
- `Base.metadata.create_all()` looks through that registry, generates the corresponding tables in database
- Even though `app.models` imports not used, need to import to run the code defining the table classes, to register them with `Base`
```python
from app.db.database import Base, engine
from app.models import student, course, enrollment

def init_db():
	print('Creating database tables...')
	Base.metadata.create_all(bind=engine)
	print('Done')

if __name__ == '__main__':
	init_db()
```

---

# 11. Tests
```python
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

# Create
def test_create_course():
	payload = {
		'code': 'FEL1847',
		'name': 'Database Fundamentals',
		'credit_hours': 4
	}
	response = client.post('/courses/', json=payload)
	data = response.json()
	
	assert response.status_code == 200
	for key, value in payload.items():
		assert data[key] == value
	
	client.delete(f'/courses/{data['id']}')  # Delete after test

# Read
def test_get_courses():
	response = client.get('/courses/')
	data = response.json()
	
	assert response.status_code == 200
	assert isinstance(data, list)
	assert len(data) > 0
	
	for course in data:
		assert set(course.keys()) == {'id', 'code', 'name', 'credit_hours'}

def test_get_course():
	response = client.get('/courses/1')
	data = response.json()
	
	assert response.status_code == 200
	assert isinstance(data, dict)
	assert set(data.keys()) == {'id', 'code', 'name', 'credit_hours'}

# Update

# Delete
```

---

# Pydantic Models
## Data Types
```python
from app.db.database import Base
from sqlalchemy import String, Text, Integer, Boolean, DateTime, Date, Float, Numeric, JSON
from sqlalchemy.orm import Mapped, mapped_column
from datetime import datetime, date
from decimal import Decimal

class Course(Base):
	__tablename__ = 'course'

	i: Mapped[int] = mapped_column(Integer)
	s: Mapped[str] = mapped_column(String(100))
	t: Mapped[str] = mapped_column(Text)  # No length limit
	f: Mapped[float] = mapped_column(Float)
	d: Mapped[Decimal] = mapped_column(Numeric(5, 2))
	b: Mapped[bool] = mapped_column(Boolean)
	dt: Mapped[datetime] = mapped_column(DateTime)
	d: Mapped[date] = mapped_column(Date)
	j: Mapped[dict] = mapped_column(JSON)

	# Optional fields
	o: Mapped[int | None] = mapped_column(Integer)
```

## Common Arguments
- `default` for when inserting with Python, `server_default` for when inserting with raw SQL
```python
id: Mapped[int] = mapped_column(
        Integer,
        primary_key=True,  # Automatically applies nullable=False
        autoincrement=True,
        nullable=False,
        unique=True,
        index=True,  # Create index (Faster lookup)
        default=1,  # Python-side default
        server_default=text('1'),  # Database-side default
        onupdate=datetime.now,  # Time of latest update
        comment='Primary key identifier',  # Description
        name='course_id',  # Name in database
    )
```

## Relationship
### Simple Foreign Key (1 User, Many Posts)
```python
from sqlalchemy import ForeignKey
from sqlalchemy.orm import Mapped, mapped_column, relationship

class User(Base):
    __tablename__ = 'user'
    
    id: Mapped[int] = mapped_column(Integer, primary_key=True)

    posts: Mapped[list['Post']] = relationship(
	    back_populates='user'
	)

class Post(Base):
    __tablename__ = 'post'

    id: Mapped[int] = mapped_column(Integer, primary_key=True)
    
    user_id: Mapped[int] = mapped_column(
	    ForeignKey('user.id')
	)
    user: Mapped['User'] = relationship(
	    back_populates='posts'
	)
```

### Self-referential Relationship (Relationship within Same Table)
- Without `remote_side`, SQLAlchemy don't know which is child, which is parent
```python
class Student(Base):
    __tablename__ = 'student'
    
    id: Mapped[int] = mapped_column(Integer, primary_key=True)

    mentor_id: Mapped[int | None] = mapped_column(
	    ForeignKey('student.id')
	)
    mentor: Mapped['Student | None'] = relationship(
        remote_side=[id],  # id column in this same table
        back_populates='mentees'
    )
    mentees: Mapped[list['Student']] = relationship(
        back_populates='mentor'
    )
```

### back_populates
- Automatic synchronization
```python
class User(Base):
    posts: Mapped[list['Post']] = relationship(
	    back_populates='user'
	)

class Post(Base):
    user: Mapped['User'] = relationship(
	    back_populates='posts'
	)

user = User()
post = Post()

# Method 1
user.posts.append(post)
print(post.user)  # Automatically set

# Method 2
post.user = user
print(user.posts)  # Automatically set

# If no back_populates, need to add both sides
user.posts.append(post)
post.user = user
```

---

# Path Param, Query Param
- `https://.../items/123?q=hello&size=5`
- Path param (`items`)
- Query param (`q='hello'`, `size=5`)

## New Style (Using Annotated)
- With `alias`, instead of searching for `user_name`, searches for `user-name` instead
- Because cannot use `-` in python
```python
from typing import Annotated
from fastapi import Path, Query

@app.get('/users/{username}')
def get_user(
    username: Annotated[
        str,
        Path(
	        min_length=3,
	        max_length=20,
	        regex='^[a-zA-Z0-9_]+$',
	
	        gt=0,
	        ge=1,
	        lt=100,
	        le=99,
	        multiple_of=2,
	
	        title='Username',
	        description='Path parameter example',
	        example='john_doe',
	        deprecated=False,
	        alias='user-name'
	    )
	],

    score: Annotated[
        int | None,
        Query(
            gt=0,
            ge=1
        )
	] = None  # Default value
):
    return {'username': username, 'score': score}
```

## Old Style
```python
from fastapi import Path, Query

@app.get('/users/{username}')
def get_user(
    username: str = Path(
        ...,
        min_length=3,
        max_length=20,
    ),

    score: int | None = Query(
		None,  # Default value
        gt=0,
        ge=1
    )
):
    return {'username': username, 'score': score}
```

## Custom Validation
```python
from typing import Annotated
from pydantic import AfterValidator

# Validation function
def must_be_positive(value: int):
    if value <= 0:
        raise ValueError('Value must be positive')
    return value

@app.get('/square/')
async def square_number(
    x: Annotated[int, AfterValidator(must_be_positive)]
):
    return {'x': x, 'square': x ** 2}
```

---

# Enums
```python
from enum import Enum

class CoffeeSize(str, Enum):
    small = 'small'
    medium = 'medium'
    large = 'large'

@app.get('/order/{size}')
async def order_coffee(size: CoffeeSize):
    if size is CoffeeSize.small:
        return {'size': size, 'message': 'A small coffee'}
    
    if size is CoffeeSize.large:
        return {'size': size, 'message': 'A large coffee'}
    return {'size': size, 'message': 'A medium coffee'}
```

