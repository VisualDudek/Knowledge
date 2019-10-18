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



