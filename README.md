# node-ina219-async
Node.js Driver for INA219 current sensing module.
Returns promises for easier operation chaining.

This library supports having multiple sensors connected by passing in i2c bus number and i2c device address.

Based on the [ina219 node module](https://www.npmjs.com/package/ina219), which in turn is based on [Adafruit's INA219 library](https://github.com/adafruit/Adafruit_INA219).


# Install

````bash
$ npm install ina219-async
````

##Usage

Take a single reading, chained with `then`:
```javascript

  const Ina219Board = require('ina219-async');
  
  const ina219 = Ina219Board();
  ina219.calibrate32V2A()
    .then(() => ina219.getBusVoltage_V())
    .then(volts => console.log("Voltage: " + volts))
    .then(() => ina219.getCurrent_mA())
    .then(current => console.log("Current (mA): " + current))
    .then(() => ina219.closeSync());
```

Async example:

```javascript
  const Ina219Board = require('ina219-async');
  
  const ina219 = Ina219Board();
  
  // Inside an async function:
  await ina219.calibrate32V2A();
  const volts = await ina219.getBusVoltage_V();
  console.log("Voltage: " + volts);
  const current = await ina219.getCurrent_mA();
  console.log("Current (mA): " + current);
  
  // Make sure to call closeSync() to close the undelying i2c bus reference
  // to free resources when the ina219 object is no longer needed
  ina219.closeSync();
```

Multiple sensors:
```javascript

  const Ina219Board = require('ina219-async');

  const bus1 = Ina219Board(0x40, 1);
  await bus1.calibrate32V2A();

  const bus2 = Ina219Board(0x42, 1);
  await bus2.calibrate32V1A();

  const volts1 = await bus1.getBusVoltage_V();
  const volts2 = await bus2.getBusVoltage_V();
  console.log("Voltage:", volts1, volts2);
```


## Module export
The `ina219-async` library exports a single function to be used as a constructor.

```javascript
  const Ina219Board = require('ina219-async');
  
  const ina219 = Ina219Board(i2cAddress = 0x40, i2cBus = 1);
```

This function takes 2 parameters - i2c address of the ina219 device and i2c bus where the device is connected.
If not specified, address `0x40` and device `1` are used by default.

Make sure to call one of the calibrate methods before reading voltage and current values.

## Methods
The `Ina219Board` object created above has the following methods:
  * [async .calibrate32V1A()](#calibrate32V1A())
  * [async .calibrate32V2A()](#calibrate32V2A())
  * [async .getBusVoltage_V()](#getBusVoltage_V())
  * [async .getShuntVoltage_mV()](#getShuntVoltage_mV())
  * [async .getCurrent_mA()](#getCurrent_mA())
  * [closeSync()](#closeSync())


### calibrate32V1A()
Configures the INA219 to be able to measure up to 32V and 1A of current.
 Each unit of current corresponds to 40uA, and each unit of power corresponds
 to 800mW. Counter overflow occurs at 1.3A.
 Note: These calculations assume a 0.1 ohm shunt resistor is used.

 Returns a promise which is completed upon successfully configuring the sensor.

### calibrate32V2A()
Configures the INA219 to be able to measure up to 32V and 2A of current.

Returns a promise which is completed upon successfully configuring the sensor.

### getBusVoltage_V()
Gets the bus voltage in volts. Returns a promise.

### getShuntVoltage_mV()
Gets the shunt voltage in mV (so +-327mV). Returns a promise.

### getCurrent_mA()
Gets the current value in mA, taking into account the config settings and current LSB.
Returns a promise.

### closeSync()
Closes the underlying i2c bus object used for communicating with the INA219 device to free resources.
