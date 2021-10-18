# enum

## summary

* fixes: str representation which is to broad, e.g. role atribute

## usage

```python
from enum import Enum, auto
from abs import ABC

class Role(Enum):
    """Employee roles."""
    
    PRESIDENT = auto()
    VICEPRESIDENT = auto()
    MANAGER = auto()
    LEADER = auto()
    WORKER = auto()
    INTERN = auto()
    
    
@dataclass
class Employee(ABC)
    """Basic representation of an employee at the company"""
    
    name: str
    role: Role
    vacation_days: int = 25
```
