#Barometric Pressure and Temperature Command

Tag `0x50`-`0x5f` command for Barometric Pressure and Temperature function are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set to 0x00.

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

> | Byte Length | 4 | 4 | 8 (optional) |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | Air Pressure | Temperature | Time Stamp |

> **Air Pressure**: Float with 4 bytes length. In the unit of Pa.

> **Temperature**: Float with 4 bytes length. In the unit of degree Celsius.

> **Time Stamp**: Unsigned Long, unit is 2.4414us. Represent the data record time. Only appear when the "with time stamp" format is chosen. This timer might not start from zero. Usually it is used to help synchronize data from multiple sensors or calculate the accurate data rate.

---

### Tag `0x5A`

> #### Meaning:
> Start air pressure and temperature offline record. Record data without Bluetooth connection.

> #### Data Format - Input:

> | Byte Length | 1 | 1 | 8 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | Period | Stop Adv | Current Unix Time |

> | Period | Code |
> | :--- | :--- |
> | 1 min | 0x01 |
> | 2 min | 0x02 |
> | 3 min | 0x03 |
> | 5 min | 0x05 |
> | 10 min | 0x0a |
> | 15 min | 0x0f |
> | 30 min | 0x1e |
> | 1 hour | 0x3c |
> | 2 hour | 0x3d |
> | 4 hour | 0x40 |
> | 8 hour | 0x44 |

> **Stop Adv:** Stop Advertising Code. If 0x01, BLE advertising will stop during recording for power saving, advertising reactivate on charging. If 0x00, advertising continue during recording.

> **Current Unix Time:** Input the current Unix Time in second, in format of 8 bytes, for recording date and time calculation. If not use, can be entered as 0.

> Sensor will auto disconnect and start recording after this command. Please wait for disconnect and don't send any other command after this command.

> For data reading, refer to the [**Storage**](storage.md#tag-0x40) command page.

> #### Data Format - Output:

> Same as send format. Same format will acknowledge after command is executed.

---

### Tag `0x5B`

> #### Meaning:
> Stop air pressure and temperature offline recording.

> #### Data Format - Input:

> | Byte Length | 8 | 1 |
> | :--- | :--- | :--- |
> | **Meaning** | Current Unix Time | Save Data Only |

> **Current Unix Time:** Input the current Unix Time in second, in format of 8 bytes, for recording date and time calculation. If not use, can be entered as 0.

> **Save Data Only:** Recording not stopped, only save the current record to storage ready for read.

> #### Data Format - Output:
> Same as send format. Same format will acknowledge after command is executed.

---













