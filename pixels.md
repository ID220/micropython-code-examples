# Pixels

These are examples of showing different patterns of light on the Raspberry PI Pico with the 8x8 pixels [BlinkPico](https://github.com/ID220/BlinkPico) shield.

**Index**

- [Pixels](#pixels)
  - [Matrix LED blinking](#matrix-led-blinking)
    - [All LED](#all-led)
    - [Turning on or off a single LED](#turning-on-or-off-a-single-led)
    - [Blink one LED](#blink-one-led)
    - [X pattern](#x-pattern)
    - [Other options](#other-options)

---

## Matrix LED blinking

Various examples of how to blink the "pixels" of the _BlinkPico_ matrix..
All the examples assume you import the blinkpico library and other dependencies:

```python
from pyblinkpico import *
from machine import Pin
import time
```

### All LED

Clear all the LEDs

```python
display.clear()
# or
display.fill(0)
```

or set them all on

```python
display.fill(1)
```

### Turning on or off a single LED

Set on or off one LED at location `[row, column]`. The max size for rows and columns is 8.

```python
display[0,0]= 0 # off
display.show()

display[0,0]= 1 # on
display.show()

display[7,7] = 1 # right corner on
display.show()
```

You can make the call to `display.show()` directly or make it automatic by setting the option _auto_show_ as `true`

```python
display.auto_show(True)
```

> All following examples will assume that the option _auto_show_ is `true`

### Blink one LED

```python
while True:
    display[0,0]= 0 # off
    time.sleep_ms(1000)
    display[0,0]= 1 # on
    time.sleep_ms(1000)
```

or even better

```python
while True:
    display[0,0] = not display[0,0]
    time.sleep_ms(1000)
```

### X pattern

```python
SIZE = 8

def xcross(value):
    for r in range (0,SIZE):
        for c in range (0,SIZE):
            if (r==c or r+c==SIZE-1):
                display[r,c]= value
            else:
                display[r,c]= not value

while True:
    xcross(1)
    time.sleep_ms(1000)
    xcross(0)
    time.sleep_ms(1000)
```

### Other options

You can set the blinking rate and brightness for all LEDs in the matrix.

```
display.brightness(50)
display.blink_rate(Matrix.NO_BLINK)
```

Please refer to [this page](https://github.com/ID220/BlinkPico/blob/main/library/README.md).
