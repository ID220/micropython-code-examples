# Python basics

This pages contains very simple code examples you should probably be already familiar with. For further documentation of how to use MicroPython with the micro:bit please refer to [these amazing tutorials](https://microbit-micropython.readthedocs.io/en/v1.0.1/).

**Index**

- [Python basics](#python-basics)
  - [Types and convertions](#types-and-convertions)
    - [Numbers (unmutable)](#numbers-unmutable)
    - [Strings (unmutable)](#strings-unmutable)
    - [Booleans (unmutable)](#booleans-unmutable)
    - [Lists (mutable)](#lists-mutable)
  - [Functions](#functions)
    - [Declaring and using a function](#declaring-and-using-a-function)
    - [Passing parameters](#passing-parameters)
    - [Functions and globals](#functions-and-globals)
    - [Higher order functions](#higher-order-functions)

---

## Types and convertions

### Numbers (unmutable)

```python
x = 3   # integer
y = 5.2   # floating point

x + y       # 8.2
x - y       # -2.2
x * y       # 15.6
10 / x      # 3.3333
10 // x     # 3
10 % x      # 1
x ** 3      # 27
abs(-2)     # 2
int(y)      # 5
```

### Strings (unmutable)

```python
str1 = "A string"
str2 = 'Another'

str1 + " " + str2   # "A string Another"
str1 * 2            # "A stringA string"
len(str1)           # 8
str1[0]             # 'A'
str1[0:4]           # 'A st'
str1.index('s')     # 2
str1.split(' ')     # ['A', 'string']
"1" + str(2)        # '12'
int("1") + 1        # 2 (number)
```

### Booleans (unmutable)

```python
True
False

True or False       # True
True and False      # False
not True            # False

5 < 10              # True
```

### Lists (mutable)

```python
ls = []
other = [1,2,3]

ls.append(4)        # ls=> [4]
other.pop()         # other => [1,2]
ls.extend(other)    # ls => [4, 1, 2]
len(ls)             # 3
ls[0]               # 4
ls[0:2]             # [4, 1]
ls[0]= 1            # ls => [1, 1, 2]
ls.copy()           # [1, 1, 2]
ls.clear()          # ls = []
```

> **NOTE** Python has several other [buil-in types](https://docs.python.org/3/library/stdtypes.html). Worth noticing are dictionaries (`dict`), `tuple`, `range`, and `set`.

---

## Functions

### Declaring and using a function

```python
# definition
def sum(a,b):
    return a+b

# function call
sum(1,2)        # 3
```

### Passing parameters

All parameters are passed by reference, but mutable and immutable types behaves differently

**Immutable types**

```python
def changeNum(n):
    n= 0

x= 5
changeNum (x)
print (x)       # x is still 5
```

**Mutable types**

```python
def changeList(n):
    n[0] = 0

x= [5]
changeList (x)
print (x)    # The list changed to [0]
```

### Functions and globals

```python
x=1

# this won't work!!
def increment():
    x+= 1       # error, x is local but not initialized

# this is correct
def increment():
    global x
    x+= 1   # x is a global, so it works
```

### Higher order functions

> This is an advanced topic and we will not cover this in class. If you are curious, however, I am happy to explain more.

In python all function are first-order which means they can be passed assigned to variables, passed as parameters and returned by functions.

Here an example of a function passed as parameter to called later

```python
from microbit import *

def sayHello(): display.show('Hello')

def callMeAfter(msTime, callBackFunction):
    sleep(msTime)
    callBackFunction()

callMeAfter(4000, sayHello)
# the function sayHello is called after 4 seconds
```

Here an example of a function which partially applied

```python
def sum(a,b): return a+b

# add2 is a partial application of sum
def add2(x):
    return sum(x, 2)

add2(10)        # 12
```
