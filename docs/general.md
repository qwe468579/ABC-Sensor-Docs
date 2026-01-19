# General Command

Tag `0x00`-`0x2f` command for general use, battery and LED are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set as 0x00.

---

### Tag `0x00`

> **Meaning:**	
> Get/reply product ID and firmware version.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 1 | 1 |
> | :--- | :--- | :--- |
> | **Meaning** | Product ID | Firmware Version |

> **Product ID**: Different product ID represent following sensor series.

> | Sensor Series | Product ID |
> | :--- | :--- |
> | A Series | 0x41 |
> | B Series | 0x42 |

> **Firmware Version**: Integer that represent the firmware version.

---

### Tag `0x01`

> **Meaning:**	
> Ping message, sensor will reply a ping message if it get ping.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> NA

---

### Tag `0x02`

> **Meaning:**	
> Get the sensor modules availability.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 2 |
> | :--- | :--- |
> | **Meaning** | Modules availability flags |

> **Modules availability flags**: Each bit represent that sensor module availability, 1 is function normally, 0 is disabled.

> | Bit Order | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
> | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
> | **Sensor Module** | BMS | LED | EEPROM | IMU | MAG | Air Pressure | EMG | TOF | SD Card |

---

### Tag `0x03`

> **Meaning:**	
> Error, sensor will send this message when any error happened.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | Error Code |

> Different error code have the following meaning:

> | Error Code | Error Meaning |
> | :--- | :--- |
> | 0x00 | General Command Format Error |
> | 0x01 | SPI Error |
> | 0x02 | I2C Error |
> | 0x03 | IMU Error |
> | 0x04 | LED Error |
> | 0x05 | Magnetometer Error |

---

### Tag `0x04`

> **Meaning:**  
> For development use.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> None

---

### Tag `0x05`

> **Meaning:**	
> Request/Report the BLE connection parameters.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Conn Interval Min | Conn Interval Max | Slave Latency | Supervision Timeout |

> **Connection Interval Min**: Range from 0x01(7.5ms) ... 0xff(1912.5ms), default is 30ms.

> **Connection Interval Max**: Range from 0x01(7.5ms) ... 0xff(1912.5ms), default is 45ms.

> **Slave Latency**: Latency in the number of connection event, 0x00 ... 0xff, default is 0.

> **Supervision Timeout**: Range from 0x01(100ms) ... 0xff(25500ms), default is 5000ms.

> On Android System parameters should be able to set directly from the client, this command is not required.

> These parameters are used to request from client, actual set parameters might change depend on system.

> **Data format - app receive:**  

> Same as the send format. Sensor will response the actual set parameters.

---

### Tag `0x09`

> **Meaning:**  
> Enter shipment mode, with very low current consume, wake on charging.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> None.

---

### `Tag 0x10`

> **Meaning:**	
> ask/reply current battery level.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | battery level(%) |

> **battery level**: unsigned integer, only value between 0% to 100%.

---











