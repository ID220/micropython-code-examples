# NeoPixels

An example of how to use the neopixels for a string of 8 LEDs.
Reference can be found [here](https://docs.micropython.org/en/latest/rp2/quickref.html#neopixel-and-apa106-driver).

```py
from machine import Pin
from neopixel import NeoPixel
import time

pin = Pin(0, Pin.OUT)   # set GPIO0 to output to drive NeoPixels
np = NeoPixel(pin, 8)   # create NeoPixel driver on GPIO0 for 8 pixels

# Only one red
np[0] = (255, 0, 0)
np.write()
r, g, b = np[0]
print (r,g,b)
time.sleep(1)

# All Red
np.fill((255, 0, 0)) # all red
np.write()
time.sleep(1)

# ... or (all green)
for i in range (0, len(np)):
    np[i] =(0, 255, 0)
    np.write()
    time.sleep(1)

# All off
np.fill((0, 0, 0)) # all off
np.write()
```
