# TOF Command

Tag `0x60`-`0x6f` command for Time Of Flight (TOF) function are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set to 0x00.

---

### Tag `0x60`

> **Meaning:**  
> Start TOF streaming with specific configuration. Recording continue until user call the stop recording command tag `0x61`. Data replay from TOF sensor will use tag `0x66`.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | ODR | Timing Budget | Distance Mode | Reply Mode | ROI (optional) |

> **Output Data Rate (ODR)**: Output data rate of the distance measured while streaming, all support output data rate are listed below.

> **Timing Budget**: Increasing the timing budget improves the measurement reliability but also increases power consumption. Longer timing budget have lower maximum support ODR, user should choose the capable ODR depend on timing budget.

> **Distance Mode**: There are short mode and long mode, which support maximum distance measurable are 1.3m and 4m. Short mode have better ambient immunity.

> **Reply Mode**: Choose the reply mode with time stamp or not. Code listed below.

> **Region of Interest (ROI)**: Smaller ROI can reduce the field of view of the sensor, this is an optional parameter, default is 16x16.

> | ODR (Hz) | Code |
> | :--- | :--- |
> | 0.5 | 0x64 |
> | 1 | 0x32 |
> | 3 | 0x10 |
> | 6 | 0x08 |
> | 12 | 0x04 |
> | 25 | 0x02 |
> | 50 | 0x01 |

> | Timing Budget (ms) | Code | Max ODR Support |
> | :--- | :--- | :--- |
> | 15 | 0x0f | 66Hz |
> | 20 | 0x14 | 50Hz |
> | 33 | 0x21 | 30Hz |
> | 50 | 0x32 | 20Hz |
> | 100 | 0x64 | 10Hz |
> | 200 | 0xc8 | 5Hz |
> | 500 | 0xff | 2Hz |

> | Distance Mode | Code | Max Distance |
> | :--- | :--- | :--- |
> | Short Mode | 0x01 | 1.3m |
> | Long Mode | 0x02 | 4m |

> | Reply Mode | Code |
> | :--- | :--- |
> | Distance | 0x01 |
> | Distance with Time Stamp | 0x02 |

> | ROI | Code |
> | :--- | :--- |
> | 16x16 | 0xff |
> | 8x8 | 0x77 |
> | 4x4 | 0x33 |

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge after command is executed.

---

### Tag `0x61`

> **Meaning:**  
> Stop TOF data streaming.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge after command is executed.

---

### Tag `0x66`

> **Meaning:**  
> Reply the TOF streaming data start from tag `0x60`.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  

> | Byte Length | 2 | 8 |
> | :--- | :--- | :--- |
> | Meaning | Distance (mm) | Time Stamp (optional) |

> **Distance**: Unsigned integer with 2 bytes, unit is mm, represent the distance measured by the TOF sensor.

> **Time Stamp**: Unsigned Long, unit is 2.4414us. Represent the data record time. Only appear when the "with time stamp" format is chosen. Pay attention that this value might not start from zero. Usually it is used to help synchronize data from multiple sensors or calculate the accurate data rate.




