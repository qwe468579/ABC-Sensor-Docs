#Barometric Pressure and Temperature Command

Tag `0x50`-`0x5f` command for Barometric Pressure and Temperature function are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set to 0x00.

---

### Tag `0x50`

> #### Meaning:
> Start air pressure and temperature streaming.

> #### Data Format - Input:

> | Byte Length | 1 | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | ODR | Meas Mode | OSR | IIR Filter |

> **ODR (Output Data Rate)**: Code listed below, set how many data output per second.

> **Meas Mode (Measurement Mode)**: Code listed below, choose weather air pressure or temperature is measured.

> **OSR (Over Sampling Ratio)**: Code listed below, higher over sampling ratio increase noise performance, but also increase current consume. Higher over sampling ratio support lower max ODR. Only apply on pressure measurement, if temperature only mode is chosen, fill this with OSR 512 (code: 0x0f).

> **IIR Filter**: Code listed below, this is used to filter out pressure glitches due to sudden pressure
changes caused by events such as slamming door or wind blowing on the sensor, but also make the sensor response to changes slower.

> | ODR / Hz | Code | 
> | :--- | :--- |
> | 1 | 0xff |
> | 2 | 0x80 |
> | 4 | 0x40 |
> | 8 | 0x20 |
> | 16 | 0x10 |
> | 32 | 0x08 |
> | 64 | 0x04 |
> | 128 | 0x02 |
> | 256 | 0x01 |

> | Measurement Mode | code | Reply Tag | tag info |
> | :--- | :--- | :--- | :--- |
> | air pressure only | 0x01 | `0x56` | [here](press-temp.md#tag-0x56) |
> | air pressure only with time stamp | 0x02 | `0x56` | [here](press-temp.md#tag-0x56) |
> | temperature only | 0x03 | `0x56` | [here](press-temp.md#tag-0x56) |
> | temperature only with time stamp | 0x04 | `0x56` | [here](press-temp.md#tag-0x56) |
> | air pressure and temperature | 0x05 | `0x56` | [here](press-temp.md#tag-0x56) |
> | air pressure and temperature with time stamp | 0x06 | `0x56` | [here](press-temp.md#tag-0x56) |

> | Over Sampling Ratio | Code | Max ODR Support |
> | :--- | :--- | :--- |
> | 512 | 0x0f | 561Hz |
> | 1024 | 0x1f | 294Hz |
> | 2048 | 0x3f | 151Hz |
> | 4096 | 0x7f | 76Hz |
> | 8191 | 0xff | 38Hz |

> | IIR Filter | Code | Step Response Rate (samples) |
> | :--- | :--- | :--- |
> | 0.2 | 0x02 | ~2 |
> | 0.3 | 0x03 | ~3 |
> | 0.4 | 0x04 | ~4 |
> | 0.5 | 0x05 | ~6 |
> | 0.6 | 0x06 | ~8 |
> | 0.7 | 0x07 | ~12 |
> | 0.8 | 0x08 | ~20 |
> | 0.9 | 0x09 | ~45 |

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

> | Byte Length | 4 (optional) | 4 (optional) | 8 (optional) |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | Air Pressure Value | Temperature Value | Time Stamp |

> **Air Pressure Value**: Two's complement integer with 4 bytes length. Use the following equation to calculate the air pressure in kPa:

> P(kPa) = (Air Pressure Value / 131072) * 40 + 70

> **Temperature Value**: Two's complement integer with 4 bytes length. Use the following equation to calculate the temperature in C:

> T(C) = (Temperature Value / 262144) * 65 + 25

> **Time Stamp**: Unsigned Long, unit is 2.4414us. Represent the data record time. Only appear when the "with time stamp" format is chosen. This timer might not start from zero. Usually it is used to help synchronize data from multiple sensors or calculate the accurate data rate.

> All optional bytes will appear when the corresponding Measurement Mode is selected.

---

### Tag `0x5A`

> #### Meaning:
> Start offline record. Record data without Bluetooth connection.

> #### Data Format - Input:

> | Byte Length | 1 | 1 | 1 | 1 | 8 |
> | :--- | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Data Selection | Period | Storage | Stop Adv | Current Unix Time |

> | Data Selection | Code |Period | Code |
> | :--- | :--- | :--- | :--- |
> | Pressure data only | 0x00 | 1 min | 0x01 |
> | Temperature data only | 0x01 | 2 min | 0x02 |
> | Pressure and temperature data | 0x02 | 3 min | 0x03 |
> | | | 5 min | 0x05 |
> | | | 10 min | 0x0a |
> | | | 15 min | 0x0f |
> | | | 30 min | 0x1e |
> | | | 1 hour | 0x3c |
> | | | 2 hour | 0x3d |
> | | | 4 hour | 0x40 |
> | | | 8 hour | 0x44 |

> **Storage:** Storage location. If 0x00, data will store to EEPROM, if 0x01, data store to SD card. Note that some model don't have SD card slot.

> **Stop Adv:** Stop Advertising Code. If 0x01, BLE advertising will stop during recording for power saving, advertising reactivate on charging. If 0x00, advertising continue during recording.

> **Current Unix Time:** Input the current Unix Time in second, in format of 8 bytes, for recording date and time calculation.

> Sensor will auto disconnect and start recording after this command. Please wait for disconnect and don't send any other command after this command, sensor might need some time to prepare the storage.

> For data reading, refer to the [storage](storage.md#tag-0x40) command page.

> #### Data Format - Output:

> Same as send format. Same format will acknowledge after command is executed.

---

### Tag `0x5B`

> #### Meaning:
> Stop offline recording.

> #### Data Format - Input:
> None.

> #### Data Format - Output:
> Same as send format. Same format will acknowledge after command is executed.

---

### Tag `0x5C`

> #### Meaning:
> Get

> #### Data Format - Input:
> None.

> #### Data Format - Output:
> Same as send format. Same format will acknowledge after command is executed.












