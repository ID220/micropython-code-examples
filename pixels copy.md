# Buttons

These are examples of showing how to use the buttons with [BlinkPico](https://github.com/ID220/BlinkPico) shield.

**Index**

- [Buttons](#buttons)
  - [Presses](#presses)
  - [Clicks](#clicks)

---

## Presses

```python
from pyblinkpico import *
import time

while True:
    print(button_a.is_pressed())
    print(button_b.is_pressed())
    print(button_c.is_pressed())

    print (button_a.get_presses()) # how many times it was pressed?
    print (button_a.was_pressed()) # was it ever clicked
  time.sleep(2)
```

See more details [here](https://github.com/ID220/BlinkPico/blob/main/library/README.md).

## Clicks

Here how you can detect a click, instead of a press

```python
from pyblinkpico import *
import time

pressed= False

while True:
    if pressed and not button_a.is_pressed():
        print ("click")
        pressed= False
    elif not pressed and button_a.is_pressed():
        pressed= True

```
