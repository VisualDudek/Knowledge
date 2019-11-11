# 100DaysOfWeb in Python

## Flask

### doc

[http://flask.palletsprojects.com/en/1.1.x/](http://flask.palletsprojects.com/en/1.1.x/)

### A minimal App

`$ pipenv install pytlint --dev`

{% hint style="info" %}
Remember to set up env for flask app:

1. `$ export FLASK_APP=<app_file>`
2. Optional for dev `$ export FLASK_ENV=development` it activates the debugger and automatic reloader.
{% endhint %}

{% hint style="info" %}
`favicon.ico`
{% endhint %}

```markup
<!-- Adding a favicon -->
<link rel="shortcut icon" href="{{ url_for('static', filename='favicon.ico') }}">
```

[gauger.io/fonticon](https://gauger.io/fonticon) - create beautiful favicons with ease

```python
#demo.py

from flask import Flask, redirect, url_for

app = Flask(__name__)

@app.route('/')
def home():
    return f'Hello World'
    
@app.route('/user/<user>')
def user(user):
    return f'Hello {user}!'
    
@app.route('/admin')
def admin():
    return redirect(url_for('home'))
    
if __name__ = '__main__':
    app.run()
```

### Templates

```python
# program/
# |
# |-templates/
#   |-index.html
```

```python
# routes.py
from program import app
from flask import render_template

@app.route('/')
@app.route('/index')
def index():
    return render_template('index.html')
```

[https://fontawesome.com/start](https://fontawesome.com/start) - all kind of icons

```python
# 1-flask/
#   |
#   |---demo.py
#   |---program/
#       |---__init__.py
#       |---routes.py
```

