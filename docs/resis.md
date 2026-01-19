# Resistive Sensor Command

Tag `0x80`-`0x8f` command for resistive sensor are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set to 0x00.

> **Output**: Data from user application to sensor.

> **Input**: Data from sensor to user application.

---

### Tag `0x80`

> #### Meaning:

> Set resistive sensor measurement configuration.

> #### Data Format - Output:

> | Byte Length | 1 | 1 | 1 | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | CH0 Gain | CH1 Gain | CH2 Gain | CH3 Gain | CH4 Gain | CH5 Gain |

> **CH Gain:** Code given below. Set the channel is activate or not, and how many times the measured value is amplified.

> | Gain | code |
> | :--- | :--- |
> | OFF | 0xff |
> | x1 | 0x00 |
> | x2 | 0x01 |
> | x4 | 0x02 |
> | x8 | 0x03 |
> | x16 | 0x04 |
> | x32 | 0x05 |
> | x64 | 0x06 |
> | x128 | 0x07 |

> This configuration command have to be called every time before `0x81` start command.

> #### Data Format - Input:

> Same as send format. Same format will acknowledge after command is executed.

---

### Tag `0x81`

> #### Meaning:

> Start resistive sensor measurement. Measurement will continue until `0x82` stop command is called. Data reply with tag `0x86`.

> #### Data Format - Output:

> | Byte Length | 1 | 1 |
> | :--- | :--- | :--- |
> | **Meaning** | IDAC Current | Data Rate |

> | IDAC Current | Code | Data Rate | Code |
> | :--- | :--- | :--- | :--- |
> | 10uA | 0x01 | 2.5Hz | 0x00 |
> | 50uA | 0x02 | 5Hz | 0x01 |
> | 100uA | 0x03 | 10Hz | 0x02 |
> | 250uA | 0x04 | 16.6Hz | 0x03 |
> | 500uA | 0x05 | 20Hz | 0x04 |
> | 750uA | 0x06 | 50Hz | 0x05 |
> | 1000uA | 0x07 | 60Hz | 0x06 |
> | 1500uA | 0x08 | 100Hz | 0x07 |
> | 2000uA | 0x09 | 200Hz | 0x08 |
> | | | 400Hz | 0x09 |
> | | | 800Hz | 0x0a |
> | | | 1000Hz | 0x0b |
> | | | ~~2000Hz~~ | 0x0c |
> | | | ~~4000Hz~~ | 0x0d |

> **IDAC Current** is the current pass through the resistive sensor, the maximum supply voltage is 3.3V, all resistive sensor are in series. Since the internal reference voltage regulation, maximum voltage on each sensor should not exceed 2.5V.

> **Data Rate** is shared between all channel, for example, if 100Hz DR is set and 4 channel is activated, DR on each channel is 25Hz.

> #### Data Format - Input:

> Same as send format. Same format will acknowledge after command is executed.

---

### Tag `0x82`

> #### Meaning:

> Stop resistive sensor measurement.

> #### Data Format - Input:

> None.

> #### Data Format - Output:

> Same as send format. Same format will acknowledge after command is executed.

---

### Tag `0x86`

> #### Meaning:

> Reply

> #### Data Format - Input:

> NA.

> #### Data Format - Output:

> | Byte Length | 2 - 12 |
> | :--- | :--- |
> | **Meaning** | ADC Value (CH0-CH5) |

> **ADC Value:** Two bytes two's complement value. Returned number of values depend on how many channel is activated, smaller channel data go first.







