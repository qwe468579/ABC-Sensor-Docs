# General Command

Tag `0x00`-`0x2f` command for general use, battery and LED are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set as 0x00.

---

### Tag `0x00`

> **Meaning:**	
> Get/reply Product ID.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | Product ID |

> Different sensor have the following product ID:

> | Sensor Series | Product ID |
> | :--- | :--- |
> | A Series | 0x41 |

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
> Ping reply, sensor will reply to a ping with this tag.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  
> None

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
> Hello message, for some specific function only.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> None

---

### Tag `0x05`

> **Meaning:**	
> SPI read/reply, do a SPI read at the sensor.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | SPI bus ID and CS number | register address | read length |

> For the first data byte, upper 4 bits is SPI bus ID and Lower 4 bits is CS number, it is for choosing which SPI bus and SPI device is going to read.

> Length of the data read should be less than 20 bytes each time.

> **Data format - app receive:**  

> | Byte Length | depend |
> | :--- | :--- |
> | **Meaning** | data read from SPI |

---

### Tag `0x06`

> **Meaning:**	
> SPI write/reply, do a SPI write at the sensor.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | depend |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | SPI bus ID and CS number | register address | data to write |

> For the first data byte, upper 4 bits is SPI bus ID and Lower 4 bits is CS number, it is for choosing which SPI bus and SPI device is going to write.

> Length of the data write should be less than 20 bytes each time.

> **Data format - app receive:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | data written length |

---

### Tag `0x07`

> **Meaning:**	
> I2C read/reply, do a I2C read at the sensor.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | I2C bus ID | I2C slave address | register address | read length |

> I2C bus ID choose which I2C bus is using, if there are more than one I2C bus one the controller. If there are only one I2C bus, use 0x00 as the bus ID.

> I2C slave address is a 7 bits address specified by the I2C device

> Please also include the last read/write bit as 0 when sending this command. For example, if the slave address is 0b0100101, the I2C slave address byte should be 0b01001010, which is 0x4a.

> Length of the data read should be less than 20 bytes each time.

> **Data format - app receive:**  

> | Byte Length | depend |
> | :--- | :--- |
> | **Meaning** | data read from I2C |

---

### Tag `0x08`

> **Meaning:**	
> I2C write/reply, do a I2C write at the sensor.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | 1 | depend |
> | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | I2C bus ID | I2C slave address | register address | data to write |

> I2C bus ID choose which I2C bus is using, if there are more than one I2C bus one the controller. If there are only one I2C bus, use 0x00 as the bus ID.

> I2C slave address is a 7 bits address specified by the I2C device

> Please also include the last read/write bit as 0 when sending this command. For example, if the slave address is 0b0100101, the I2C slave address byte should be 0b01001010, which is 0x4a.

> Length of the data write should be less than 20 bytes each time.

> **Data format - app receive:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | data written length |

---

### Tag `0x09`

> **Meaning:**  
> Enter ship mode, wake on charging.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> None.

---

### `Tag 0x10`

> **Meaning:**	
> ask/reply current battery charging status.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | charging status |

> | charging status | Meaning |
> | :--- | :--- |
> | 0 | not under charging |
> | 1 | charging |

---

### `Tag 0x11`

> **Meaning:**	
> ask/reply current battery level.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 4 |
> | :--- | :--- |
> | **Meaning** | battery level (%) |

> Battery level returned in float, unit is %.

---

### Tag `0x12`

> **Meaning:**	
> ask/reply battery voltage.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 4 |
> | :--- | :--- |
> | **Meaning** | battery voltage (V) |

> Battery voltage returned in float, unit is V.

---

### Tag `0x13`

> **Meaning:**	
> ask/reply battery current.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 4 |
> | :--- | :--- |
> | **Meaning** | battery current (A) |

> Battery current returned in float, unit is A.

---

### `Tag 0x14`

> **Meaning:**	
> ask/reply battery capacity.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 4 |
> | :--- | :--- |
> | **Meaning** | battery capacity (mAh) |

> Battery capacity returned in float, unit is mAh.

---

### `Tag 0x15`

> **Meaning:**	
> ask/reply battery estimated time to empty for the application under present load conditions.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 4 |
> | :--- | :--- |
> | **Meaning** | estimated time to empty (s) |

> Estimated time to empty returned in float, unit is second(s). Maximum value is 368634.24s.

---

### `Tag 0x16`

> **Meaning:**	
> ask/reply battery estimated time to full for the application under present charge conditions.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 4 |
> | :--- | :--- |
> | **Meaning** | estimated time to full (s) |

> Estimated time to full returned in float, unit is second(s). Maximum value is 368634.24s.

---

### Tag `0x20`

> **Meaning:**	
> Get/report LED color to application.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | Red value | Green value | Blue value |

> Each RGB color code from 0-255.

> If need to close the LED, set all RGB color to 0.

> Sensor will also report the LED color to application actively every time the color changed.

---

### Tag `0x21`

> **Meaning:**	
> Set LED brightness.

> **Data format - app send:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | brightness value |

> Brightness value has 1 byte length, unsign integer, range from 0-100. The default brightness is 30.

> **Data format - app receive:**  
> Same as send format.

> Sensor will acknowledge the LED brightness after brightness has been set.

---

### Tag `0x22`

> **Meaning:**	
> Set LED color. Every time LED color set manually, the control by the system to LED will stop. System won't control the LED color anymore, the color user set will last forever, until user change to another color or the LED restore default command `0x23` is send.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | Red value | Green value | Blue value |

> Each RGB color code from 0-255.

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge to the user after command is executed.

---

### Tag `0x23`

> **Meaning:**  
> LED restore default command. User release the LED control, let system change the LED color to its default behavior again.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge to the user after command is executed.

---

### Tag `0x24`

> **Meaning:**  
> Display/Stop LED pattern. This command also let user to take LED control, use "stop display all pattern" command or LED restore default command `0x23` to release control.

> **Data format - app send:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | LED Pattern |

> | LED Pattern | Pattern Type |
> | :--- | :--- |
> | 0x00 | Stop display all pattern |
> | 0x01 | RGB Color Shift |

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge to the user after command is executed.

---












