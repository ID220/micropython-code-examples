# Sensors

**Index**
- [Sensors](#sensors)
  - [Optical encoder](#optical-encoder)
  - [Compass](#compass)
  - [Acceleroemter](#acceleroemter)
  - [Temperature](#temperature)

## Optical encoder

Here the full code example of using an optical encoder.

```python
from microbit import *

prevCase = 0

# position is the actual value of the rotation
# positive values are added when spinning right
# negative values are added when spinning left
position = 0
prevPosition = 0


# Setting the pullups
pin0.set_pull(pin0.PULL_UP)
pin1.set_pull(pin1.PULL_UP)


while True:
    # 1. Determine the case in the quadrature
    a = pin0.read_digital()
    b = pin1.read_digital()
    quadratureCase = 0

    if (a == 1 and b == 0):
        quadratureCase = 0  # case 0
    elif (a == 1 and b == 1):
        quadratureCase = 1  # case 1
    elif (a == 0 and b == 1):
        quadratureCase = 2  # case 2
    elif (a == 0 and b == 0):
        quadratureCase = 3  # case 3

    if (quadratureCase == prevCase):
        continue  # no changes

    # 2. Determine if spinning left or right, and increment accordingly position
    increment = quadratureCase-prevCase
    prevCase = quadratureCase

    if (increment == -3):
        position -= 1  # left
    elif (increment == 3):
        position += 1  # right

    if (position == prevPosition):
        continue  # no changes

    prevPosition = position
    print(position) # printing the result
```

## Compass

Using the built-in compass in micro:bit.

```python
from microbit import *


# Start calibrating
compass.calibrate()

# Try to keep the needle pointed in (roughly) the correct direction
while True:
    sleep(100)
    needle = ((15 - compass.heading()) // 30) % 12
    display.show(Image.ALL_CLOCKS[needle])
    
```


## Acceleroemter

Detect shaking with the default threshold, using the built-in accelerometer:

```python
from microbit import *

while True:
    if accelerometer.was_gesture('shake'):
        display.scroll('Shake')
        sleep(1000)
        display.clear()
    sleep(10)
```

or using a custom threshold

```python
from microbit import *
from math import *

while True:
    xyz = accelerometer.get_values()
    x = xyz[0]
    y = xyz[1]
    z = xyz[2]
    '''
    # or
    x = accelerometer.get_x()
    y = accelerometer.get_y()
    z = accelerometer.get_z()
    '''

    shake = sqrt(int(x*x + y*y + z*z))
    if shake > 3000:  # 3000 is a custom threshold
        display.scroll('Shake')
        sleep(1000)
        display.clear()
    sleep(10)
```

## Temperature

Get the temperature in Celsius of the board

```python
from microbit import *

while True:
    print (temperature())
    sleep(1000)
```