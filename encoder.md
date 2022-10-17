# Encoder

Here how to use an incremental encoder (see slides 9.1 for reference):

```py
from machine import Pin
import time

prevCase = 0

# position is the actual value of the rotation
# positive values are added when spinning right
# negative values are added when spinning left
position = 0
prevPosition = 0


# Setting the pullups
a = Pin(17, Pin.IN, pull=Pin.PULL_UP)
b = Pin(16, Pin.IN, pull=Pin.PULL_UP)


while True:
    # 1. Determine the case in the quadrature
    quadratureCase = 0

    if (a() == 1 and b() == 0):
        quadratureCase = 0  # case 0
    elif (a() == 1 and b() == 1):
        quadratureCase = 1  # case 1
    elif (a() == 0 and b() == 1):
        quadratureCase = 2  # case 2
    elif (a() == 0 and b() == 0):
        quadratureCase = 3  # case 3

    if (quadratureCase == prevCase):
        continue  # no changes


    # 2. Determine if spinning left or right, and increment accordingly position
    increment = quadratureCase-prevCase
    prevCase = quadratureCase

    if (increment == -3):
        position += 1  # right
    elif (increment == 3):
        position -= 1  # left

    if (position == prevPosition):
        continue  # no changes

    prevPosition = position
    print(position) # printing the result
```
