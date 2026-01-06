![Jinja](https://www.vectorlogo.zone/logos/pocoo_jinja/pocoo_jinja-icon.svg)

---

# Comments
```html
{# Comment #}
```

---

# Template Inheritance

## base.html
```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Site{% endblock %}</title>
</head>

<body>
    {% block body %}Empty{% endblock %}
</body>
</html>
```

## index.html
```html
{% extends 'base.html' %}

{% block title %}Home{% endblock %}

{% block body %}
    <p>Hello, {{ name }}!</p>
{% endblock %}
```

---

# Inject Code Snippet

## header.html
```html
<header>
    <h1>My Site</h1>
</header>
```

## index.html
```html
{% include 'header.html' %}
<p>Welcome to the homepage!</p>
```

---

# Simple Variable
```python
@app.route('/')
def index():
    return render_template('index.html', name='Adam')
```

```html
<h1>Hello, {{ name }}!</h1>
```

# Lists
```python
@app.route('/')
def index():
    return render_template('index.html', users=['Adam', 'Alex', 'Joe'])
```

```html
<ul>
    {% for user in users %}
        <li>{{ user }}</li>
    {% endfor %}
</ul>
```

# Conditionals
```html
{% if age >= 18 %}
    <p>You are an adult.</p>
{% else %}
    <p>You are a minor.</p>
{% endif %}
```

# Loops
```html
{% for user in users %}
    <li class="{{ loop.cycle('odd', 'even') }}">
        {{ loop.index }}. {{ user.name }}
        {% if loop.first %} (First) {% endif %}
        {% if loop.last %} (Last) {% endif %}
    </li>
{% endfor %}

<!-- Loop over dict -->
{% for key, value in user.items() %}
    <p>{{ key }}: {{ value }}</p>
{% endfor %}

<!-- Attributes of loop -->
{{ loop.index }}  <!-- 1-based -->
{{ loop.index0 }}  <!-- 0-based -->
{{ loop.first }}
{{ loop.last }}
{{ loop.length }}
```

# Filters
```html
<p>Uppercase: {{ name|upper }}</p>
<p>Lowercase: {{ name|lower }}</p>
<p>Title case: {{ name|title }}</p>
<p>Length: {{ name|length }}</p>
<p>Default: {{ name|default('None') }}</p>

<!-- Can chain -->
{{ name|upper|length|default('None') }}
```

---

# Set Variables
```html
{% set name = user.first_name + ' ' + user.last_name %}
<p>{{ name }}</p>
```

---

# Macros
- Reusable code snippets
```html
{% macro user_card(user) %}
	<div class="card">
	    <h3>{{ user.name|title }}</h3>
	    <p>Age: {{ user.age }}</p>
	</div>
{% endmacro %}

{% for user in users %}
    {{ user_card(user) }}
{% endfor %}
```



