# ALS Command

Tag `0x50`-`0x5f` command for ambient light sensor(ALS), used to detect light intensity.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set as 0x00.

---

### Tag `0x50`

> **Meaning:**  
> Configure and start ALS with parameters. Use this command before getting data from the ALS.

> **Data format - app send:**  

> | Byte Length | 1 | 2 | 1 | 4 |
> | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | A Time | W Time | A Gain | GA |

> **A Time**: ALS integration time. Options refer to code below.  
> **W Time**: ALS wait time, time between each integration. Options refer to code below.  
> **A Gain**: Gain of the output data, used at low light intensity environment. Options refer to code below.  
> **GA**: Glass attenuation. Used to compensate for attenuation, for a device in open air with no aperture or glass/plastic above the device, GA = 1. This is a float number.  

> One measurement cycle ~= (A Time + W Time)

> Data will not stream to the application automatically, command tag `0x52` need to be used to get the ALS illuminance data.

> | A Time | A Time Code | W Time | W Time Code | A Gain | A Gain Code |
> | :--- | :--- | :--- | :--- | :--- | :--- |
> | 50ms  | 0xee | 0ms | 0xffff | x0.16 | 0x04 |
> | 100ms | 0xdb | 10ms | 0x00fc | x1 | 0x00 |
> | 150ms | 0xc9 | 20ms | 0x00f9 | x8 | 0x01 |
> | 200ms | 0xb7 | 50ms | 0x00ee | x16 | 0x02 |
> | 250ms | 0xa4 | 100ms | 0x00db | x120 | 0x03 |
> | 300ms | 0x92 | 200ms | 0x00b7 |
> | 350ms | 0x80 | 500ms | 0x0049 |
> | 400ms | 0x6d | 1000ms | 0x01e1 |
> | 450ms | 0x5b | 2000ms | 0x01c3 |
> | 500ms | 0x49 | 5000ms | 0x0167 |
> | 550ms | 0x37 |
> | 600ms | 0x24 |
> | 650ms | 0x12 |
> | 700ms | 0x00 |

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge to the user after command is executed.

---

### Tag `0x51`

> **Meaning:**  
> Stop and power down ALS. This command will also execute by the sensor when disconnected.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge to the user after command is executed.

---

### Tag `0x52`

> **Meaning:**  
> Get/reply ALS illuminance data, in unit of lux.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 4 |
> | :--- | :--- |
> | **Meaning** | Lux |

> The returned lux is a float value.







