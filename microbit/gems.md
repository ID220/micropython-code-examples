# Gems

The following snippets are a random collection of libraries to use Radio, NeoPixels, LCD displays, etc...

**Index**
- [Gems](#gems)
  - [NeoPixels](#neopixels)
  - [LCD](#lcd)
  - [Radio](#radio)
## NeoPixels

Here the example we did in class, where we cycle over different colors for different LEDs (total 8 LEDs) on a strip.

```python
from microbit import *
import neopixel
from random import randint

# Setup the Neopixel strip on pin0 with a length of 8 pixels
np = neopixel.NeoPixel(pin0, 8)

colors = [
    (255, 0, 0),  # red
    (0, 255, 0),  # green
    (0, 0, 255),  # blue
    (255, 255, 0),  # yellow
    (255, 0, 255),  # purple
    (255, 255, 255)  # white
]


while True:
    for color in colors:
        for pixel_id in range(0, len(np)):
            # Assign the current LED a random red, green and blue value between 0 and 60
            np[pixel_id] = color

            # Display the current pixel data on the Neopixel strip
            np.show()
            sleep(100)
```

## LCD

Operating an LCD via I2C. This code uses this [library](https://github.com/rhubarbdog/microbit-LCD-driver).

```python
from microbit import *
import microbit_i2c_lcd as lcd

display = lcd.lcd(i2c)

display.lcd_display_string("mic hello world this is cool and fun", 1)
```

Make sure that in the `microbit_i2c_lcd.py` file you have set the correct address of the LCD slave, like for example this way:

```python
# LCD Address, could be  0x27
ADDRESS = 0x27
```

## Radio

Here an example of how to use the radio, inspired by [this](https://microbit-micropython.readthedocs.io/en/v1.0.1/tutorials/radio.html) tutorial:

```python
from microbit import display, button_a, sleep
import radio

# The radio won't work unless it's switched on.
radio.on()

counter = 0
while True:
    if button_a.was_pressed():
        radio.send('Send '+str(counter)) 
        counter += 1

    incoming = radio.receive()
    if incoming != None:
        print(incoming)
    sleep(500)
```