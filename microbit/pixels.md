# Pixels

These are examples of showing different patterns of light on the micro:bit built-in LED matrix display.

**Index**

- [Pixels](#pixels)
  - [Matrix LED blinking](#matrix-led-blinking)
    - [One LED](#one-led)
    - [All](#all)
    - [X-Cross](#x-cross)
    - [Blinking two LEDs (blocking)](#blinking-two-leds-blocking)
    - [Blinking two LEDs (non-blocking)](#blinking-two-leds-non-blocking)

---

## Matrix LED blinking

Various examples of how to blink the "pixels" (the LEDs of the micro:bit matrix).
All the examples assume you import the microbit library: `from microbit import *`

### One LED

```python
while True:
    display.set_pixel(0,0,9)
    sleep(1000)
    display.set_pixel(0,0,0)
    sleep(1000)
```

or even better

```python
def pixelToggle(row, col):
    curr = display.get_pixel(row,col)
    if curr: display.set_pixel(row,col,0)
    else: display.set_pixel(row,col,9)

while True:
    pixelToggle(0,0)
    sleep(1000)
```

### All

```python
def all (value):
    for row in range (0,5):
        for col in range (0,5):
            display.set_pixel(row,col,value)

while True:
    all(9)
    sleep(1000)
    all(0)
    sleep(1000)
```

### X-Cross

```python
def xcross (value):
    for row in range (0,5):
        for col in range (0,5):
            if (row==col or row+col==4):
                display.set_pixel(row,col,value)

while True:
    xcross(9)
    sleep(1000)
    xcross(0)
    sleep(1000)
```

### Blinking two LEDs (blocking)

```python
delay1= 1000
delay2= 100
counter = 0

# pixelToggle is defined in the examples above

while True:
    pixelToggle (1,1) # Fast LED (1,1)

    # we reached 10
    if (counter %10 == 0):
        pixelToggle(0,0) # Slow LED (0,0)

    sleep (delay2) # delay2 is 1/10 of delay1
    counter += 1
```

### Blinking two LEDs (non-blocking)

```python
import utime

delay1= 1000
delay2= 100
prevTime1 = 0
prevTime2 = 0

# pixelToggle is defined in the examples above

while True:
    currTime = utime.ticks_ms()
    delta1= utime.ticks_diff(currTime, prevTime1)
    delta2= utime.ticks_diff(currTime, prevTime2)

    if (delta1 >= delay1):
        pixelToggle(0,0) # Slow LED (0,0)
        prevTime1= currTime

    if (delta2 >= delay2):
        pixelToggle(1,1) # Fast LED (1,1)
        prevTime2= currTime
```

or even better

```python
import utime
prevTime = {}

def tick(ms, callback):
    if (ms not in prevTime):
        prevTime[ms] = 0
    if utime.ticks_diff(utime.ticks_ms(), prevTime[ms]) > ms:
        callback()
        prevTime[ms] = utime.ticks_ms()

delay1= 1000
delay2= 100

# pixelToggle is defined in the examples above
while True:
    # Slow LED (0,0)
    tick(delay1, lambda: pixelToggle(0,0))
    # Fast LED (1,1)
    tick(delay2, lambda: pixelToggle(1,1))
```
