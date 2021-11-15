# Files

This is an example of how to read and write on a file (`data.csv`). Specifically, this file contains a list of coordinates (row, column) of the pixels of the micro:bit that should be on.

Here an example:

```csv
1,2
3,4
0,0
```

The following code read this file, and display these points. The user can add points by moving the cursor ('A' button) and record the new pixel ('B' button)

```python
from microbit import *

def drawPixels(pixels):
    for (row, col) in pixels:
        display.set_pixel(row, col, 9)

def loadContent(filename):
    content = []
    with open(filename, 'r') as fread:
        data = fread.readline()
        while data != '':
            rowcol = tuple(map(lambda x: int(x.strip()), data.split(',')))
            content.append(rowcol)
            data = fread.readline()
    return content


def saveContent(filename, content):
    with open(filename, 'w') as fwrite:
        for (row, col) in pixels:
            fwrite.write(str(row))
            fwrite.write(',')
            fwrite.write(str(col))
            fwrite.write('\n')


def locationToRowCol(loc):
    row = loc//5
    col = int(loc % 5)
    return (row, col)

# Main
loc = 0

sleep(500)
pixels = loadContent('data.csv')
drawPixels(pixels)

while True:
    if button_a.is_pressed():
        loc = (loc+1) % 25
        sleep(200)
    elif button_b.is_pressed():
        pixels.append(locationToRowCol(loc))
        saveContent('data.csv', pixels)
        sleep(200)

    display.clear()
    drawPixels(pixels)
    drawPixels([locationToRowCol(loc)])
    sleep(50)
```