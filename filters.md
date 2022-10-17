# Software Filters

Here an example of a simple low pass filter.

```py
from machine import Pin, ADC
import time

FILTER_LENGTH= 10
MIN = 0
MAX= int(2**16)
lp= [0] * FILTER_LENGTH
index= 0

a0 = ADC(Pin(26))

while True:
    lp[index]= a0.read_u16()
    index = (index + 1) % len(lp)
    avg= sum(lp)/len(lp)
    print (MIN, avg, MAX)
    time.sleep_ms(50)
```
