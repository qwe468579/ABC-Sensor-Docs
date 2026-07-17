# B10 Command

BLE-Sensor-B10 all command explained in this page.

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
> | B10 | 0x42 |
> | R10 | 0xFF |

> **Firmware Version**: Integer that represent the firmware version.

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
> Enter shutdown mode, with very low current consume, wake on charging. RED led blinking before shutdown.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> None.

---

### Tag `0x10`

> **Meaning:**	
> ask/reply current battery level.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | battery level |

> **battery level**: Battery level percentage as unsigned integer, from 0 to 100.

---

### Tag `0x30`

> **Meaning:**	
> Start recording from IMU. Recording continue until user call the stop recording command tag `0x31`. Data replay from IMU will use tag `0x36`-`0x39`.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Accel ODR | Accel FS | Gyro ODR | Gyro FS | Reply Format |

> Output Data Rate (ODR), Full Scale (FS) and reply format code listed below.

> | Accel ODR / Hz | code | Accel FS / +-g | code |
> | :--- | :--- | :--- | :--- |
> | ~~6777~~ | 0x0a | 16 | 0x01 |
> | ~~3333~~ | 0x09 | 8 | 0x03 |
> | ~~1667~~ | 0x08 | 4 | 0x02 |
> | ~~833~~ | 0x07 | 2 | 0x00 |
> | 416 | 0x06 |
> | 208 | 0x05 |
> | 104 | 0x04 |
> | 52 | 0x03 |
> | 26 | 0x02 |
> | 12.5 | 0x01 |
> | 1.6 | 0x0b |
> | OFF | 0x00 |

> | Gyro ODR / Hz | code | Gyro FS / +-dps | code |
> | :--- | :--- | :--- | :--- |
> | ~~6777~~ | 0x0a | 2000 | 0x03 |
> | ~~3333~~ | 0x09 | 1000 | 0x02 |
> | ~~1667~~ | 0x08 | 500 | 0x01 |
> | ~~833~~ | 0x07 | 250 | 0x00 |
> | 416 | 0x06 |
> | 208 | 0x05 |
> | 104 | 0x04 |
> | 52 | 0x03 |
> | 26 | 0x02 |
> | 12.5 | 0x01 |
> | OFF | 0x00 |

> Magnetometer ODR and FS is auto selected by the setting of Accelerometer and Gyroscope.

> Magnetometer only turn on when both **Accel ODR** and **Gyro ODR** are ON.

> | Reply data format | code | reply tag | tag info |
> | :--- | :--- | :--- | :--- |
> | Accel, Gyro and Mag | 0x01 | `0x36` | [here](imu.md#tag-0x36) |
> | Quaternion 9x | 0x02 | `0x37` | [here](imu.md#tag-0x37) |
> | Quaternion 9x with Accel value | 0x03 | `0x38` | [here](imu.md#tag-0x38) |

> IMU must not working on other mode when call this command.

> **Data format - app receive:**  

> Same as send format. Same format acknowledge after command is executed.

---

### Tag `0x31`

> **Meaning:**  
> Stop recording from IMU.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> Same as send format. Same format acknowledge after command is executed.

---

### Tag `0x32`

> **Meaning:**  
> Set IMU offset. (On development...)

> **Data format - app send:**  

> | Byte Length | 16 (optional) |
> | :--- | :--- |
> | **Meaning** | Offset Quaternion |

> **Offset Quaternion**: contains 4 Float value, 4 bytes each, in the order of X Y Z W, represent the quaternion offset. All zero or no offset given means remove the current offset.

> When starting a new IMU data streaming with 6x or 9x quaternion format, sensor will load the last offset from save on default.

> **Data format - app receive:**  

> Same as send format. Same format acknowledge after command is executed.

---

### Tag `0x36`

> **Meaning:**  
> Reply IMU data with Acceleration, Angular Velocity and Magnetic Strength.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  

> | Byte Length | 6 | 6 | 6 | 4 |
> | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Accel Value | Gyro Value | Mag value | Time Stamp |

> **Accel Value**: Accelerometer X, Y, Z axis value as 16bit-integer for each axis. Can be turn off by setting the ODR to 0x00.

>>> ***Acceleration(g) = Accel Value x Sense Factor***

> | Accel FS / +-g | Sense Factor |
> | :--- | :--- |
> | 2 | 0.000061 |
> | 4 | 0.000122 |
> | 8 | 0.000244 |
> | 16 | 0.000488 |

> **Gyro Value**: Gyroscope X, Y, Z axis value as 16bit-integer for each axis. Can be turn off by setting the ODR to 0x00.

>>> ***Angular Veolcity(degree/s) = Gyro Value x Sense Factor***

> | Gyro FS / +-dps | Sense Factor |
> | :--- | :--- |
> | 250 | 0.00875 |
> | 500 | 0.0175 |
> | 1000 | 0.035 |
> | 2000 | 0.07 |

> **Mag Value**: Magnetometer X, Y, Z axis value as 16bit-integer for each axis. Only available when both accelerometer and gyroscope turn on. ODR and FS are auto selected.

>>> ***Magnetic Strength(mG) = Mag Value x 1.5***

> **Time Stamp**: Data record time as 32bit-unsigned-integer, time unit is 25us.

---

### Tag `0x37`

> **Meaning:**  
> Reply IMU data with 9x Quaternion.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  

> | Byte Length | 16 | 1 | 4 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | Quaternion | Mag Calibrate Level | Time Stamp |

> **Quaternion**: Quaternion axis as 4 float, in the order of X Y Z W, represent the rotation of the sensor.

> **Mag Calibrate Level**: Magnetometer calibration level for the sensor fusion algorithm, 0 is worst, 3 is best.

> **Time Stamp**: Data record time as 32bit-unsigned-integer, time unit is 25us.

---

### Tag `0x38`

> **Meaning:**  
> Reply IMU data with 9x Quaternion and Acceleration.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  

> | Byte Length | 16 | 1 | 12 | 4 |
> | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Quaternion | Mag Calibrate Level | Accel Value | Time Stamp |

> **Quaternion**: Quaternion axis as 4 float, in the order of X Y Z W, represent the rotation of the sensor.

> **Mag Calibrate Level**: Magnetometer calibration level for the sensor fusion algorithm, 0 is worst, 3 is best.

> **Accel Value**: Accelerometer(g) value separated in X, Y, Z axis, each axis has 4 bytes, all in Float format.

> **Time Stamp**: Data record time as 32bit-unsigned-integer, time unit is 25us.

---

### Tag `0x50`

> #### Meaning:
> Start air pressure and temperature streaming. Measurement will continue until `0x51` stop command is called. Data reply with tag `0x56`.

> #### Data Format - Input:

> | Byte Length | 1 | 1 |
> | :--- | :--- | :--- |
> | **Meaning** | ODR | Average |

> **ODR (Output Data Rate):** Code listed below, set how many data output per second.

> **Average:** Set how many data are averaging for each measurement.

> | ODR / Hz | Code | 
> | :--- | :--- |
> | 1 | 0x01 |
> | 4 | 0x02 |
> | 10 | 0x03 |
> | 25 | 0x04 |
> | 50 | 0x05 |
> | 75 | 0x06 |
> | 100 | 0x07 |
> | 200 | 0x08 |

> | Averaging Data | Code | Max ODR Support |
> | :--- | :--- | :--- |
> | 4 | 0x00 | 500 |
> | 8 | 0x01 | 400 |
> | 16 | 0x02 | 300 |
> | 32 | 0x03 | 200 |
> | 64 | 0x04 | 100 |
> | 128 | 0x05 | 75 |
> | 512 | 0x07 | 25 |

> #### Data Format - Output:
> Same as send format. Same format will acknowledge after command is executed.

---

### Tag `0x51`

> #### Meaning:
> Stop air pressure and temperature streaming.

> #### Data Format - Input:
> None.

> #### Data Format - Output:
> Same as send format. Same format will acknowledge after command is executed.

---

### Tag `0x56`

> #### Meaning:
> Measured air pressure and temperature reply through streaming with this tag.

> #### Data Format - Input:
> NA.

> #### Data Format - Output:

> | Byte Length | 4 | 2 |
> | :--- | :--- | :--- |
> | **Meaning** | Press Value | Temp Value |

> **Press Value**: Air Pressure Value as 32bit-integer.

>>> ***Air Pressure(Pa) = Press Value / 40.96***

> **Temp Value**: Temperature Value as 16bit-integer. degree Celsius.

>>> ***Temperature(&#176;C) = Temp Value / 100***

---









