# Sensors

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
    abcase = 0

    if (a == 1 and b == 1):
        abcase = 0
    elif (a == 0 and b == 1):
        abcase = 1
    elif (a == 0 and b == 0):
        abcase = 2
    elif (a == 1 and b == 0):
        abcase = 3

    if (abcase == prevCase):
        continue  # no changes

    # 2. Determine if spinning left or right, and increment accordingly position
    increment = abcase-prevCase
    prevCase = abcase

    if (increment == -3):
        position -= 1  # left
    elif (increment == 3):
        position += 1  # right

    if (position == prevPosition):
        continue  # no changes

    prevPosition = position
    print(position) # printing the result
```

