# IMU Command

Tag `0x30`-`0x3f` command for Inertial measurement unit (IMU) are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set to 0x00.

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
> Reply IMU data in accelerometer(ms^-2) and gyroscope(degree/s) value format.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  

> | Byte Length | 6 | 6 | 6 | 4 |
> | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Accel Value | Gyro Value | Mag value | Time Stamp |

> **Accel Value**: Accelerometer X, Y, Z axis value as 16bit-integer for each axis. Can be turn off by setting the ODR to 0x00.

> **Acceleration(g)** = **Accel Value** x **Sense Factor**

> | Accel FS / +-g | Sense Factor |
> | :--- | :--- |
> | 2 | 0.000061 |
> | 4 | 0.000122 |
> | 8 | 0.000244 |
> | 16 | 0.000488 |

> **Gyro Value**: Gyroscope X, Y, Z axis value as 16bit-integer for each axis. Can be turn off by setting the ODR to 0x00.

> **Angular Veolcity(degree/s)** = **Gyro Value** x **Sense Factor**

> | Gyro FS / +-dps | Sense Factor |
> | :--- | :--- |
> | 250 | 0.00875 |
> | 500 | 0.0175 |
> | 1000 | 0.035 |
> | 2000 | 0.07 |

> **Mag Value**: Magnetometer X, Y, Z axis value as 16_bit-integer for each axis. Only available when both accelerometer and gyroscope turn on. ODR and FS are auto selected.

> **Magnetic Strength(mG)** = **Mag Value** x **1.5**

> **Time Stamp**: Unsigned Int, unit is 25us. Represent the data record time.

---

### Tag `0x37`

> **Meaning:**  
> Reply IMU data in quaternion data format. 9 axis (accel + gyro + mag) are used to calculate the quaternion value.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  

> | Byte Length | 16 | 1 | 4 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | Quaternion | Mag Calibrate Level | Time Stamp |

> **Quaternion**: contains 4 Float value, 4 bytes each, in the order of X Y Z W, represent the rotation axis and angle of the sensor.

> **Mag Calibrate Level**: Magnetometer calibration level for the sensor fusion algorithm, 0 is worst, 3 is best.

> **Time Stamp**: Unsigned Int, unit is 25us. Represent the data record time.

---

### Tag `0x38`

> **Meaning:**  
> Reply IMU data in quaternion with Accel value data format. 9 axis (accel + gyro + mag) are used to calculate the quaternion value.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  

> | Byte Length | 16 | 1 | 12 | 4 |
> | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Quaternion | Mag Calibrate Level | Accel Value | Time Stamp |

> **Quaternion**: contains 4 Float value, 4 bytes each, in the order of X Y Z W, represent the rotation axis and angle of the sensor.

> **Mag Calibrate Level**: Magnetometer calibration level for the sensor fusion algorithm, 0 is worst, 3 is best.

> **Accel Value**: Accelerometer(g) value separated in X, Y, Z axis, each axis has 4 bytes, all in Float format.

> **Time Stamp**: Unsigned Int, unit is 25us.

---

### Tag `0x3A`

#### > **Meaning:**  
> Start IMU record in storage.

#### > **Data format - Input:**  

> | Byte Length | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 8 |
> | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Accel ODR | Accel FS | Gyro ODR | Gyro FS | Reply Format | Reset | Stop Adv | Current Unix Time |

> ODR and FS are the same as tag `0x30`.

> | Reply data format | code |
> | :--- | :--- |
> | Accel, Gyro and Mag Storage | 0x00 |
> | Quaternion 6x Storage | 0x01 |
> | Quaternion 9x Storage | 0x02 |

> **Reset:** If 0x01, all old records will be covered. If 0x00, old records keep, new data continue stored at the remaining storage space.

> **Stop Adv:** Stop Advertising Code. If 0x01, BLE advertising stop during recording for power saving, advertising reactivate on charging. If 0x00, advertising continue during recording.

> **Current Unix Time:** Input the current Unix Time in second, in format of 8 bytes, for recording date and time calculation. If not use, can be entered as 0.

> IMU must not working on other mode when call this command.

> Sensor auto disconnect and start recording after this command. Please wait for disconnect and don't send any other command after this command.

> For data reading, refer to the **[Storage]**(storage.md#tag-0x40) command page.

#### > **Data format - Output:** 

> Same as send format. Same format acknowledge after command is executed.

---

### Tag `0x3B`

#### > **Meaning:**
> Stop IMU record in storage.

#### > **Data format - Input:**

> | Byte Length | 8 |
> | :--- | :--- |
> | **Meaning** | Current Unix Time |

> **Current Unix Time:** Input the current Unix Time in second, in format of 8 bytes, for recording date and time calculation. If not use, can be entered as 0.

#### > **Data format - Output:** 

> Same as send format. Same format acknowledge after command is executed.

---











