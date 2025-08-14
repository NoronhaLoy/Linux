# Comprehensive Python Guide for SRE / DevOps / Platform / Cloud Engineers

## 1. Basics

* Installation: `python3` or `python3 -m pip install <package>`
* Script execution: `python3 script.py`
* Comments: `# This is a comment`
* Variables: `x = 10`, dynamic typing
* Data types: int, float, str, bool, list, tuple, dict, set

**Example:**

```python
name = "Server"
print(f"Hello {name}")
```

---

## 2. Operators

* Arithmetic: `+ - * / // % **`
* Comparison: `== != < > <= >=`
* Logical: `and or not`
* Membership: `in not in`
* Identity: `is is not`

**Example:**

```python
x = 5
y = 10
print(x + y)  # 15
print(x < y)   # True
```

---

## 3. Conditionals

```python
if x > 5:
    print("x is greater than 5")
elif x == 5:
    print("x is 5")
else:
    print("x is less than 5")
```

---

## 4. Loops

### For Loop

```python
for i in range(5):
    print(f"Iteration {i}")
```

### While Loop

```python
count = 0
while count < 5:
    print(f"Count: {count}")
    count += 1
```

### Loop with List

```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

---

## 5. Functions

```python
def greet(name):
    return f"Hello {name}"

print(greet("World"))
```

* Default arguments, keyword arguments
* Variable arguments: `*args`, `**kwargs`

---

## 6. Data Structures

### List

```python
lst = [1, 2, 3]
lst.append(4)
lst.pop()
```

### Tuple

```python
t = (1, 2, 3)
```

### Dictionary

```python
d = {"name": "Server", "status": "active"}
print(d["name"])
d["status"] = "inactive"
```

### Set

```python
s = {1, 2, 3}
s.add(4)
s.remove(2)
```

---

## 7. File I/O

```python
with open('file.txt', 'r') as f:
    data = f.read()

with open('file.txt', 'w') as f:
    f.write("Hello World\n")
```

---

## 8. Exception Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
finally:
    print("Execution completed")
```

---

## 9. Modules & Packages

* Import: `import os`, `from sys import exit`
* Install: `pip install requests`
* Use: `import requests; requests.get('https://example.com')`

---

## 10. Virtual Environments

```bash
python3 -m venv env
source env/bin/activate
pip install <package>
deactivate
```

---

## 11. Advanced Topics

* List comprehensions:

```python
squares = [x**2 for x in range(5)]
```

* Dictionary comprehension:

```python
squares_dict = {x: x**2 for x in range(5)}
```

* Lambda functions:

```python
add = lambda x, y: x + y
print(add(2,3))
```

* Map, filter, reduce:

```python
nums = [1,2,3,4]
squared = list(map(lambda x: x**2, nums))
even = list(filter(lambda x: x%2==0, nums))
```

* Generators:

```python
def gen():
    for i in range(5):
        yield i
for val in gen():
    print(val)
```

* Decorators:

```python
def decorator(func):
    def wrapper():
        print("Before")
        func()
        print("After")
    return wrapper

@decorator
def say_hello():
    print("Hello")

say_hello()
```

---

## 12. SRE / DevOps Specific Examples

### Health Check

```python
import subprocess
services = ['nginx', 'mysql']
for svc in services:
    status = subprocess.run(['systemctl', 'is-active', svc], capture_output=True, text=True)
    print(f"{svc}: {status.stdout.strip()}")
```

### Deployment Automation

```python
import shutil
import os
prev_version = '/var/app/releases/v1'
current_version = '/var/app/current'
shutil.copytree(prev_version, current_version, dirs_exist_ok=True)
```

### Log Parsing

```python
with open('/var/log/syslog') as f:
    for line in f:
        if 'ERROR' in line:
            print(line)
```

### Cron Job Script Example

```python
# Python script can be called via cron for backup
import datetime
print(f"Backup started at {datetime.datetime.now()}")
```

### HTTP Check

```python
import requests
response = requests.get('https://example.com')
if response.status_code == 200:
    print("Service OK")
else:
    print("Service Down")
```

---

## 13. Best Practices

* Follow PEP8 style guide
* Use virtual environments
* Catch specific exceptions
* Logging instead of print
* Modular code with functions and classes
* Proper documentation and comments
* Use `__main__` for script entry point

This guide is now a comprehensive reference for Python knowledge applicable to SRE, DevOps, Platform, and Cloud engineers.
