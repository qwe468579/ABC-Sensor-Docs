# EMG Command

Tag `0x70`-`0x7f` command for Electromyography (EMG) function are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set to 0x00.

---

### Tag `0x70`

> **Meaning:**  
> Start EMG streaming with specific configuration. Recording continue until user call the stop recording command tag `0x71`. Data replay from EMG sensor will use tag `0x76`-`0x77`.

> **Data format - app send:**  

> | Byte Length | 1 | 1 |
> | :--- | :--- | :--- |
> | **Meaning** | ODR | Reply Format |

> **Output Data Rate (ODR)**: Output Data Rate of the EMG value while streaming, listed on the table below.

> **Reply Format**: There are 2 different format, each of them can output with time stamp or not.

> * _Standard Mode_: 12-bits accuracy, 16 data are output each notification, FIFO order

> * _Accumulate Mode_: 16-bits accuracy, one data is output each notification. Note that the output data rate is divided 16 under this mode, because of 16 12-bits data is accumulated into one 16-bits data.

> | ODR (Hz) | Code |
> | :--- | :--- |
> | 50 | 0xd2 |
> | 100 | 0x69 |
> | 200 | 0x35 |
> | 400 | 0x1a |
> | 600 | 0x12 |

> | Reply Format | Code | Tag Details |
> |  :--- | :--- | :--- |
> | Standard | 0x01 | [here](emg.md#tag-0x76) |
> | Standard with Time Stamp | 0x02 | [here](emg.md#tag-0x76) |
> | Accumulate | 0x03 | [here](emg.md#tag-0x77) |
> | Accumulate with Time Stamp | 0x04 | [here](emg.md#tag-0x77) |

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge after command is executed.

---

### Tag `0x71`

> **Meaning:**
> Stop EMG data streaming.

> **Data format - app send:**  
> None

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge to the user after command is executed.

---

### Tag `0x72`

> **Meaning:**
> Return the EMG value in integer.

> **Data format - app send:**  
> NA

> **Data format - app receive:**  

> | Byte Length | 4 |
> | :--- | :--- |
> | **Meaning** | EMG value |

> **EMG value**: Integer. Value from 0 to 10000, higher value mean stronger EMG signal is measured.

---

### Tag `0x76`

> **Meaning:**
> Return 16 EMG value in a single read.

> **Data format - app send:**  
> NA

> **Data format - app receive:**

> | Byte Length | 32 | 8 (optional) |
> | :--- | :--- | :--- |
> | **Meaning** | EMG Value | Time Stamp |

> **EMG value**: 16 EMG value in unsigned integer, 2 bytes each, little endian. Use the following equation to convert it to mV measured:

> EMG(mV) = ((EMG Value / 4095 * 3.3) - 1.65) / Gain

> **Time Stamp**: Unsigned Long, unit is 2.4414us. Represent the data record time. Only appear when the "with time stamp" format is chosen. Pay attention that this value might not start from zero. Usually it is used to help synchronize data from multiple sensors or calculate the accurate data rate.

---

### Tag `0x77`

> **Meaning:**
> Return EMG value in mV.

> **Data format - app send:**  
> NA

> **Data format - app receive:**

> | Byte Length | 2 | 8 (optional) |
> | :--- | :--- | :--- |
> | **Meaning** | EMG value | Time Stamp |

> **EMG value**: EMG value in unsigned integer, little endian. Use the following equation to convert it to mV measured:

> EMG(mV) = ((EMG Value / 65535 * 3.3) - 1.65) / Gain

> **Time Stamp**: Unsigned Long, unit is 2.4414us. Represent the data record time. Only appear when the "with time stamp" format is chosen. Pay attention that this value might not start from zero. Usually it is used to help synchronize data from multiple sensors or calculate the accurate data rate.

---





