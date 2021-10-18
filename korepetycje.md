---
description: all notes made during tutoring
---

# Korepetycje

### week 41

* **Read python docs!**
* `unittest.TestCase.setUp()`
* Good example for above [link](https://www.codingame.com/playgrounds/10614/python-unit-test-with-unittest) @CodinGame
* ```python
  ### Check also:
  @clssmethod
  def setUpClass(cls):
      ...
  ```
* `unittest.mock.MagicMock()`
* Mock `request.get`
  * mock only `.get` method, not all request
  * ```python
    request.get = MagicMock()
    request.get.return_value = 'result'
    ```
* `... import X, Y` use comas in import statement
*   ```python
    class MockRequest:
        def json(self)
    ```

    ???
* `assert_called_with()` and `assert_called_once_with() test for valid url build`
* ```python
  class InvalidPhoneNumber(ValueError):
      pass
  ```
* `def phonenumbervalidation() -> bool:` do not raise exception inside method, rasie it on class lvl BC U want to use method e.g. on web service lvl.
* check what can API SerwerSMS return after U hit with `post`
* PyCharm advantages:
  * auto completion
  * test running
  * auto import
  * auto completion in class syntax e.g. self
* `@classmethod` vs `@staticmethod`
* Data Classes `@dataclass`
* Git: 
  * `$ git diff [commit] [commit]` read manpage  in zsh alias `gd`
  * `$ git diff HEAD~1`
  * try with `TAB` check if only in zsh `$ git diff` and `TAB` should show all branches and commits
* `assertTure` and `asssertEqual` or `assertFalse` check other
* check if VSCode have auto import similar to PyCharm

#### LINUX SETUP

* snippet in `.bashrc` or `.zshrc` to check if in dir `READ.ME` exist, if yes than print out first 20 lines  

### week 42

* `Ctrl + H`
* Logging, Python lib
* Class która obsługuje informacje zwrotną z SendSMS

```python
class Status:
    def isSuccesful():
```

* dodaj w katalogu test katalog dataset dla danych testowych
* `main()` wszedzie gdzie jest to mozliwe, przykład Konrada z `pip`
* exclude all VSCode settings in `.gitignore`
* przećwicz `git diff HEAD~1` (zsh) `gd` 
* `HEAD~3` nanosi trzy ostatnie commity na siebie
* git submodule vs pip install from git repo?

#### rozliczenie week 42-43

* DevOps/SysOps Meetup
  * architektura hiperkonwergentna
  * protokol wifi bez zamykania access pointow
  * AWS Cloud
* Building Quantum Computing Algorithms In Python - Episode 235 @Python \__init\_\_
* CVE sudo minus 1
* logging
* flask templates



