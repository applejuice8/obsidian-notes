
---

# Installation
```bash
pip install flask-wtf email-validator
```

---

# Using WTForms
- `validate_on_submit()` same as `if request.method == 'POST'` + `form.validate()`
```python
@app.route('/login', methods=['GET', 'POST'])
def login():
    form = LoginForm()
    if form.validate_on_submit():
	    name = form.name.data
        password = form.password.data
        return
    return render_template('index.html', form=form)
```

```jinja2
<form method="post" action="{{ url_for('auth.login') }}">
	{{ form.hidden_tag() }}
	
	{{ form.name.label }}
	{{ form.name() }}
	<br>
	{{ form.password.label }}
	{{ form.password() }}
	<br>
	{{ form.submit }}
</form>
```

---

# Form Fields
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

---

# Validators
- `DataRequired()` for most fields
- `InputRequired()` when falsy values are valid (0, False)
```python
from flask_wtf import FlaskForm
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

---

# Render Errors
```jinja2
<form action="{{ url_for('auth.login') }}" method="POST">
	{{ form.hidden_tag() }}
	
	{# Email #}
	<div class="mb-3">
		{{ form.email.label }}
		{{ form.email() }}
		{% for error in form.email.errors %}
			<div class="text-danger">{{ error }}</div>
		{% endfor %}
	</div>
	
	{# Password #}
	<div class="mb-3">
		{{ form.password.label }}
		{{ form.password() }}
		{% for error in form.password.errors %}
			<div class="text-danger">{{ error }}</div>
		{% endfor %}
	</div>
	
	{# Form level errors #}
	<div class="mb-3">
		{% for error in form.errors.get('__all__', []) %}
			<div class="text-danger">{{ error }}</div>
		{% endfor %}
	</div>
	
	{# Submit #}
	{{ form.submit() }}
</form>
```