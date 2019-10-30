# 100DaysOfWeb in Python

## Flask

```python
# 1-flask/
#   |
#   |---demo.py
#   |---program/
#       |---__init__.py
#       |---routes.py
```

`$ pipenv install pytlint --dev`

`$ export FALSK_APP=demo.py`

{% hint style="info" %}
`favicon.ico`
{% endhint %}

```markup
<!-- Adding a favicon -->
<link rel="shortcut icon" href="{{ url_for('static', filename='favicon.ico') }}">
```

[gauger.io/fonticon](https://gauger.io/fonticon) - create beautiful favicons with ease

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

