# CAEN - Nuclear Instruments A7585X/NIPM12 Python Control Library


The NIPM-12 SiPM Power Module is a compact and integrated solution to provide stable and noiseless power supply for single and array / matrix SiPM detectors.
High resolution Output Voltage and Output Current measurements enable the NIPM-12 to be used for I-V detector characterization.
Digital (UART, I2C and USB with adapter) and analog control interface are runtime selectable by a single pin or a digital command.
The module integrates a temperature HV loop that regulates the SiPM output voltage as a programmable function of the SiPM temperature coefficient.

This library is a python A7585X/NIPM12 module. It allows to control the module using the UART/USB interface. 

## Features
- 20-85V Output Voltage
- 10mA Output Current
- 1mV Output Voltage step
- Less than 300uV rms noise
- User Selectable Digital / Analog output voltage control
- Automatic temperature feedback on the output voltage
- Multi device support using I2C
- Fully working examples designed to control the module with keypad and display or monitor multiple devices using serial port
- Module compatible with ZEUS software for stand alone usage

## Pinout of the module

![pinout of the moduke](https://github.com/NuclearInstruments/a7585d/blob/master/images/img2.png?raw=true)

## Python installation

In order to install the A7585D python Library, just run the following command:

```bash
pip install a7585d
```

## Usage

### Module user manual

The user manual can be downloaded from the CAEN website:
[https://www.caen.it/products/a7585/](https://www.caen.it/products/a7585/)

### Import the library

Import the library in your python script:

```python
from a7585d.a7585d import A7585D 
from a7585d.a7585d import A7585D_REG
```

### Create the A7585D object

Create the A7585D object and connect to the module serial port:

```python
hv = A7585D()

# open serial port
hv.open("COM3") # Windows
# hv.open("/dev/ttyUSB0") # Linux USB
# hv.open("/dev/ttyS0") # Linux UART
```

### Full code example

Create the A7585D object and connect to the module serial port:

```python
from a7585d.a7585d import A7585D 
from a7585d.a7585d import A7585D_REG
import time


hv = A7585D()

# open serial port
hv.open("COM3") # Windows
# hv.open("/dev/ttyUSB0") # Linux USB
# hv.open("/dev/ttyS0") # Linux UART

# configure parameters HV

# set control output control loop mode
# 0 Digital mode (output voltage is vtarget)
# 1 Analog mode (output voltage is proportional to vref)
# 2 Thermal compensation (output voltage is vtarget - Tcoef * (T - 25)
hv.set_parameter(A7585D_REG.CNTRL_MODE, 0)  

# set voltage target to 40V
hv.set_parameter(A7585D_REG.V_TARGET, 40)

# set max voltage 1mA
hv.set_parameter(A7585D_REG.MAX_I, 1)

# set max voltage (compliance) to 50V
hv.set_parameter(A7585D_REG.MAX_V, 50)

# configure SiPM temperature compensation coefficient
hv.set_parameter(A7585D_REG.T_COEF_SIPM, -35)

# configure ramp speed to 5V/s
hv.set_parameter(A7585D_REG.RAMP, 5)

# set current monitor range
# 0 low range
# 1 high range
# 2 auto select
hv.set_parameter(A7585D_REG.CURRENT_RANGE, 2)


# control pi controller (1 enable)
hv.set_parameter(A7585D_REG.ENABLE_PI, 0)

# enable hv
hv.set_parameter(A7585D_REG.HV_ENABLE,1)

while True:
    print("HV V_OUT: " + str(hv.get_parameter(A7585D_REG.MON_VOUT)))
    print("HV I_OUT: " + str(hv.get_parameter(A7585D_REG.MON_IOUT)))
    time.sleep(0.1)
```