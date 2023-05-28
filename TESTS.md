# normal air pressure measurement test
## setup
Connect to b018 via I2C connector. Run the following micropython code:
```python
import machine
import utime
sda = machine.Pin(18)
scl = machine.Pin(19)
i2c=machine.I2C(1,sda=sda, scl=scl, freq=400000)
devices = i2c.scan()
if devices:
    device = devices[0]
    print("device found at address:",hex(device))

    # Select Control register, 0x20
    # Active mode, Continous update 0x90
    i2c.writeto(device,b'\x20\x90')

    utime.sleep(0.1)

    data = i2c.readfrom_mem(device,0xA8,3)

    pressure = (data[2] * 65536 + data[1] * 256 + data[0]) / 4096.0

    print(f'Barometric Pressure is : {pressure:.2f} hPa')
    
```

## results
I got a measurement of 1020.07 hPa which is plausible and the value changed (slightly) when I moved the sensor up and down in the room. 
