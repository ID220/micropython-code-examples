# Shift Register

These are examples of showing different ways to send data to a 8-bit shift register (74HC595).

## Using SPI library

You can use the built in SPI library with the SPI-enabled pins.

```python
data = pin15
latch = pin2
clock = pin13

spi.init(10000, 8, 0,
         sclk=clock, mosi=data)

while True:
    latch.write_digital(0)
    
    # Send a single byte containing the number 7
    spi.write(b'\x07')

    # Send two bytes with the number 7 and 1
    buffer = bytes([7, 1])
    spi.write(buffer)

    latch.write_digital(1)
    sleep(100)
```


## Using a custom function shiftOut

You can also use a [shiftOut](https://www.arduino.cc/reference/en/language/functions/advanced-io/shiftout/) function.

```python
def shifOut(dataPin, clockPin, dataOut):
    i = 0
    pinState = 0

    dataPin.write_digital(1)
    clockPin.write_digital(0)

    for i in range(8, 0, -1):
        clockPin.write_digital(0)

        if dataOut & (1 << (i-1)):
            pinState = 1
        else:
            pinState = 0

        dataPin.write_digital(pinState)
        clockPin.write_digital(1)

    dataPin.write_digital(1)
    clockPin.write_digital(0)
```

You can then use the _shiftOut_ function this way

```python
# let's send a byte with the number 7

data = pin15
latch = pin2
clock = pin13

latch.write_digital(0)
shifOut(data, clock, 0x07)
latch.write_digital(1)

```