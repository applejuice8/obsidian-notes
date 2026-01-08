![SQLAlchemy](https://quintagroup.com/cms/python/images/sqlalchemy-logo.png/@@images/eca35254-a2db-47a8-850b-2678f7f8bc09.png)

---

# Base
- A special base class knows how to turn classes into databases
- All ORM model classes inherit from Base
- Base collects metadata from all tables
- Later `Base.metadata.create_all(engine)`
```python
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
	pass
```

---

# Tables
- `__repr__` is toString method
```python
from sqlalchemy.orm import Mapped, mapped_column
from sqlalchemy import Integer, SmallInteger, *
from decimal import Decimal
from datetime import date, time, datetime

class User(Base):
    __tablename__ = 'users'
	# Numeric
	id: Mapped[int] = mapped_column(Integer)
	id: Mapped[int] = mapped_column(SmallInteger)
	id: Mapped[float] = mapped_column(Float)
	id: Mapped[Decimal] = mapped_column(Numeric(10, 2))
	
	# String
	id: Mapped[str] = mapped_column(String(100))
	id: Mapped[str] = mapped_column(Text)  # No length limit
	
	# Date Time
	id: Mapped[date] = mapped_column(Date)  # YYYY-MM-DD
	id: Mapped[time] = mapped_column(Time)  # HH:MM:SS
	id: Mapped[datetime] = mapped_column(DateTime)
	
	# Boolean
	id: Mapped[bool] = mapped_column(Boolean)
	id: Mapped[bytes] = mapped_column(LargeBinary)
	
	# JSON
	id: Mapped[dict] = mapped_column(JSON)
	
	def __repr__(self):
		return f'<Student(id={self.id}, name={self.name})>'
```

## Attributes
```python
class User(Base):
    __tablename__ = 'users'

    id: Mapped[int] = mapped_column(Integer,
		primary_key=True,
		nullable=False,
		unique=True,
		autoincrement=True,  # Starts from 1
		default='hello',
		index=True  # Faster lookup, but takes extra space
	)
```

---

# Relationships

## Simple FK
```python
from sqlalchemy.orm import Mapped, mapped_column, relationship
from sqlalchemy import ForeignKey

# ----------------------------
# Parent table: Course
# ----------------------------
class Course(Base):
    __tablename__ = 'course'

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(50))

    students: Mapped[list['Student']] = relationship(
	    back_populates='course'  # Can course.students
    )

# ----------------------------
# Child table: Student
# ----------------------------
class Student(Base):
    __tablename__ = 'student'

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(50))

    course_id: Mapped[int] = mapped_column(
	    ForeignKey('course.id'),
	    nullable=False
	)
    course: Mapped['Course'] = relationship(
	    back_populates='student'  # Can student.course
	)
```

## Junction Table (Many-to-Many) (Need review)
- `ondelete='CASCADE'` means delete rows when student / subject deleted
```python
from sqlalchemy import ForeignKey, Integer, String
from sqlalchemy.orm import Mapped, mapped_column, relationship

# ----------------------------
# Association Table
# ----------------------------
class StudentSubject(Base):
    __tablename__ = 'student_subject'

    student_id: Mapped[int] = mapped_column(
	    ForeignKey('student.id', ondelete='CASCADE'),
	    primary_key=True
	)

    subject_id: Mapped[int] = mapped_column(
	    ForeignKey('subject.id', ondelete='CASCADE'),
	    primary_key=True
    )

    student: Mapped['Student'] = relationship(
	    back_populates='student_subjects'
	)  # Can student.student_subjects

    subject: Mapped['Subject'] = relationship(
	    back_populates='student_subjects'
	)  # Can subject.student_subjects

# ----------------------------
# Student
# ----------------------------
class Student(Base):
    __tablename__ = 'student'

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(50))

    student_subjects: Mapped[list['StudentSubject']] = relationship(
	    back_populates='student',
	    cascade='all, delete orphan'
	)
    subjects: Mapped[list['Subject']] = relationship(
	    secondary='student_subject',
	    back_populates='students',
	    viewonly=True
	)  # Can student.subjects
   
# ----------------------------
# Subject
# ----------------------------
class Subject(Base):
    __tablename__ = 'subject'

    id: Mapped[int] = mapped_column(primary_key=True)
    title: Mapped[str] = mapped_column(String(100))

    student_subjects: Mapped[list['StudentSubject']] = relationship(back_populates='subject', cascade='all, delete orphan')  # Delete rows in student_subject when subject deleted (ORM level)
    students: Mapped[list['Student']] = relationship(secondary='student_subject', back_populates='subjects', viewonly=True)  # Can subject.students
```

---

# Engine
- `echo=True` prints executed SQL queries in terminal
```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

# Create engine
engine = create_engine('sqlite:///school.db', echo=True)

# Create all database tables
from models import Base
Base.metadata.create_all(engine)

# Create session
SessionLocal = sessionmaker(bind=engine)
```

---

# CRUD

## Create
```python
with SessionLocal() as session:
    # Create students
    alice = Student(name='Alice')
    bob = Student(name='Bob')

    # Create subjects
    math = Subject(title='Mathematics')
    physics = Subject(title='Physics')

    # Add to DB
    session.add(alice)
    session.add_all([bob, math, physics])
    session.commit()
```

## Read
```python
with SessionLocal() as session:
	# Student's subjects
    alice = session.query(Student)
				    .filter_by(name='Alice')
				    .one()
    for subject in alice.subjects:
        print(subject.title)

	# Subject's students
    math = session.query(Subject)
				    .filter_by(title='Mathematics')
				    .one()
    for student in math.students:
        print(student.name)
```

## Update
```python
with SessionLocal() as session:
    alice = session.query(Student)
				    .filter_by(name='Alice')
				    .one()
    alice.name = 'Alice Tan'
    session.commit()
```

## Delete
```python
with SessionLocal() as session:
    bob = session.query(Student)
				    .filter_by(name='Bob')
				    .one()
    session.delete(bob)
    session.commit()
```

