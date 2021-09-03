# logging

## Summary

* Log event information is passed between loggers, handlers, filters and formatters in a `LogRecord` instance.

## Q

* jak getLogger\(\) dodaje kelejne loggery w hierarchi że możliwe jest dot.referencja `logger.child` ?

## basic summary

* plain `logging.warning(str)` refers to the sigleton-root logger
* if you call message on root-looger it will check `if len(root.handlers) == 0` and do the `basicConfig()`
* aha moment: there are two messaging methods/function \(1\) function of logging module e.g. `logging.warning()` and \(2\) method of class `Logger` ; fist one calls the second checking if root has a handler

```python
import logging

format = "%(asctime)s - %(levelname)s - %(message)s"
datefmt = "%d-%b-%y %H:%M:%S"
logging.basicConfig(level=logging.DEBUG, format=format, datefmt=datefmt)
# logging.warning("Start program")
# logging.debug("this is debug lvl")

# BETTER Create child logger of root-logger
logger = logging.getLogger("Test")

# BETTER: Name logger witn filename 
logger = logging.getLogger(__name__)

c_handler = logging.StreamHandler()
c_handler.setLevel(logging.DEBUG)
logger.addHandler(c_handler)

logger.warning("Hello")
```

## parsowanie --log=INFO

{% hint style="info" %}
bardzo ciekawy przykład i template do wykorzystania
{% endhint %}

`getattr(logging, loglevel.upper())`

### level arg validation

* takeaway: `getattr` also has a default value, same as get method for dict\(\)

```python
# assuming loglevel is bound to the string value obtained from the
# command line argument. Convert to upper case to allow the user to
# specify --log=DEBUG or --log=debug
numeric_level = getattr(logging, loglevel.upper(), None)
if not isinstance(numeric_level, int):
    raise ValueError('Invalid log level: %s' % loglevel)
logging.basicConfig(level=numeric_level, ...)
```

