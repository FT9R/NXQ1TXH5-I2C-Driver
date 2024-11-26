# NXQ1TXH5 I2C driver
Platform-independent I2C driver for the NXQ1TXH5 controller and driver IC for a 5 V Qi-certified/compliant low-power wireless charger. 
This driver facilitates communication between a microcontroller and the NXQ1TXH5, allowing developers to configure and monitor the IC's functionality easily.

## Features
* Charge process ON/OFF control
* Partially possible to get the current device operation
* Digital supply (Vddd) voltage level control
* Temperature readings from the internal device sensor
* Vdd readings
* NTC pin voltage readings

## Notes
Majority of device parameters are valid only if device is in charge mode (either Qi receiver is placed to the coil or device is in digital ping mode)

## Quick start
* Mention the header:
```C
#include "nxq1txh5.h"
```
* Declare the device handle:
```C
nxq1txh5_t nxq;
```
* Initialize I2C interface:
```C
I2Cx_Init();
```
* Provide platform depended implementations for functions below in the `nxq1txh5_ifc.c`:
```C
nxq1txh5_status_t NXQ1TXH5_I2Cx_Receive(uint8_t regAddress, uint16_t *data);
nxq1txh5_status_t NXQ1TXH5_I2Cx_Transmit(uint8_t regAddress, uint16_t *data);
```
* Link the functions above to a device handle:
```C
NXQ1TXH5_Link(&nxq, NXQ1TXH5_I2Cx_Receive, NXQ1TXH5_I2Cx_Transmit);
```

## TODO
According to UM11152 it is also possible to request the following parameters from device:
* `freq` - Operating frequency (Hz) of the charger.
* `dut` - Duty cycle (%) of the full bridge in; generally, it is at 50 %.
* `Icrms` - RMS current (mA) through the coil. The calculation of this current is based on the
voltage that is measured on pin VSEN.
* `ptx` - Input power (mW) of the full bridge. It excludes the dissipation in LEDs.
* `prx` - Received power (mW) that the receiver reports. This power is not the power received in
the load, but the power received in the magnetic field.
* `ppad` - Calculated loss in the charger (mW). ppad = FOD_E * Icrms2
 / 1000. When the value of FOD_E is not correct, the calculated loss is wrong.
* `pfor` - Calculated foreign power (mW). pfor = ptx − ppad − prx.
* `qidata` - \<headerid> \<data> \<checksum>

## Examples
* [STM32](platform/STM32F103C8T6/Core/Src/main.c)
