# node-ina219-async
Node.js Driver for INA219 current sensing module.
Returns promises for easier operation chaining.

Based on the [ina219 node module](https://www.npmjs.com/package/ina219), which in turn is based on [Adafruit's INA219 library](https://github.com/adafruit/Adafruit_INA219).


# Install

````bash
$ npm install ina219-async
````

##Usage
```javascript

  var ina219 = require('ina219-async')();
  await ina219.calibrate32V2A();
  const volts = await ina219.getBusVoltage_V();
  console.log("Voltage: " + volts);
  const current = await ina219.getCurrent_mA();
  console.log("Current (mA): " + current);
```


## Methods
  * [()](#init)
  * [(address, device)](#init)
  * [async .calibrate32V1A()](#calibrate32V1A())
  * [async .calibrate32V2A()](#calibrate32V2A())
  * [async .getBusVoltage_V()](#getBusVoltage_V())
  * [async .getShuntVoltage_mV()](#getShuntVoltage_mV())
  * [async .getCurrent_mA()](#getCurrent_mA())


### init
The initialization function is the single export from this Node module.
Takes (address, device) as parameters.
If no parameters are passed, address `0x40` and device `1` is assumed.
Called to initilize the INA219 board, you call calibrate after this.

Returns the ina219 object used for querying readings.

### calibrate32V1A()
Configures to INA219 to be able to measure up to 32V and 1A of current.
 Each unit of current corresponds to 40uA, and each unit of power corresponds
 to 800mW. Counter overflow occurs at 1.3A.
 Note: These calculations assume a 0.1 ohm shunt resistor is used.

 Returns a promise which is completed upon successfully configuring the sensor.

### calibrate32V2A()
Configures to INA219 to be able to measure up to 32V and 2A of current.

Returns a promise which is completed upon successfully configuring the sensor.

### getBusVoltage_V()
Gets the bus voltage in volts. Returns a promise.

### getShuntVoltage_mV()
Gets the shunt voltage in mV (so +-327mV). Returns a promise.

### getCurrent_mA()
Gets the current value in mA, taking into account the config settings and current LSB.
Returns a promise.
