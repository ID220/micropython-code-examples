# I2C

Here an example of how to use the **MPU6050** IMU with I2C.

```py
from machine import Pin, I2C
import time

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


# Main

i2c = I2C(0, scl=Pin(17), sda=Pin(16), freq=400_000)
ids = i2c.scan()
MPU = ids[0]

i2c.writeto(MPU, bytearray([0x6B, 1])) # device reset
time.sleep_ms(100)
i2c.writeto(MPU, bytearray([0x68, 7]))  # gyro reset
time.sleep(1)



while True:
    i2c.writeto(MPU, bytearray([0x3B]))

    raw = i2c.readfrom(MPU, 14)
    AcX = toGforce(bytes_toint(raw[0], raw[1]))
    AcY = toGforce(bytes_toint(raw[2], raw[3]))
    AcZ = toGforce(bytes_toint(raw[4], raw[5]))
    Tmp = toCelisus(bytes_toint(raw[6], raw[7]))
    GcX = toDegSec(bytes_toint(raw[8], raw[9]))
    GcY = toDegSec(bytes_toint(raw[10], raw[11]))
    GcZ = toDegSec(bytes_toint(raw[12], raw[13]))

    # Print the result
    print(AcX, AcY, AcZ)
    print(Tmp)
    print(GcX, GcY, GcZ)
    time.sleep_ms(500)
```

Here the same code using a [library](https://github.com/micropython-IMU/micropython-mpu9x50) and following [this example](https://microcontrollerslab.com/mpu6050-raspberry-pi-pico-micropython-tutorial/):

```py
from imu import MPU6050
import time, math
from machine import Pin, I2C
i2c = I2C(0, scl=Pin(17), sda=Pin(16), freq=400_000)
imu = MPU6050(i2c)

print("Temperature: ", round(imu.temperature,2), "°C")
while True:
    ax=round(imu.accel.x,2)
    ay=round(imu.accel.y,2)
    az=round(imu.accel.z,2)
    gx=round(imu.gyro.x)
    gy=round(imu.gyro.y)
    gz=round(imu.gyro.z)
    tem=round(imu.temperature,2)
    print("ax",ax,"\t","ay",ay,"\t","az",az,"\t","gx",gx,"\t","gy",gy,"\t","gz",gz,"\t","Temperature",tem,"        ",end="\r")
    time.sleep(0.2)
```
