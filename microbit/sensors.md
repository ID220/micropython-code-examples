# Sensors

**Index**
- [Sensors](#sensors)
  - [Optical encoder](#optical-encoder)
  - [Compass](#compass)
  - [Acceleroemter](#acceleroemter)
  - [Temperature](#temperature)
  - [I2C with MPU6050](#i2c-with-mpu6050)

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


## I2C with MPU6050

```python
from microbit import *
from math import *

MPU = 0x68
rotation = 0


def bytes_toint(firstbyte, secondbyte):
    if not firstbyte & 0x80:
        return firstbyte << 8 | secondbyte
    return - (((firstbyte ^ 255) << 8) | (secondbyte ^ 255) + 1)


def toDegSec(value):
    return value / 131


def toGforce(value):
    return value / 16384


def toCelisus(value):
    return value / 340.00 + 36.53


i2c.init(freq=100000, sda=pin20, scl=pin19) # starts I2C

ids = i2c.scan()
i2c.write(MPU, bytearray([0x6B, 0]), repeat=False)  # reset
i2c.write(MPU, bytearray([0x68, 7]), repeat=False)  # calibrate
sleep(1000)


while True:
    i2c.write(MPU, bytearray([0x3B]), repeat=False)
    raw = []
    raw = i2c.read(MPU, 14, repeat=True)
    AcX = toGforce(bytes_toint(raw[0], raw[1]))
    AcY = toGforce(bytes_toint(raw[2], raw[3]))
    AcZ = toGforce(bytes_toint(raw[4], raw[5]))
    Tmp = toCelisus(bytes_toint(raw[6], raw[7]))
    GcX = toDegSec(bytes_toint(raw[8], raw[9]))
    GcY = toDegSec(bytes_toint(raw[10], raw[11]))
    GcZ = toDegSec(bytes_toint(raw[12], raw[13]))

    print(AcX, AcY, AcZ)
    print(Tmp)
    print(GcX, GcY, GcZ)
```