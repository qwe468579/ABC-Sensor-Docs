# WIFI Command

Tag `0x60`-`0x6f` command for WIFI connection are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set as 0x00.

---

### Tag `0x60`

> **Meaning:**	
> Start the WIFI connection. Set the WIFI parameters (tag`0x62` - tag`0x64`) before calling this function. If no new WIFI parameters are set, previous WIFI configuration will be used.

> BLE advertising and connection will stop after WIFI is connected.

> **Data format - app send:**  
> None.

> **Data format - app receive:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | Connection code |

> | Connection code | Meaning |
> | :--- | :--- |
> | 0x00 | Start connecting WIFI AP |
> | 0x01 | WIFI connection success |
> | 0x02 | WIFI disconnect / connection failed |

> Connection code will send through BLE to report the WIFI connection status, after success connect to WIFI AP, BLE connection will terminate.

---

### Tag `0x61`

> **Meaning:**	
> Stop WIFI connection and return to BLE advertising mode.

> **Data format - app send:**  
> None.

> **Data format - app receive:**  
> None.

> Same format will acknowledge to the user after command is executed.

---

### Tag `0x62`

> **Meaning:**	
> Set/Get the WIFI SSID, maximum length is 32 char. Data will save to the sensor EEPROM use for next WIFI connection.

> **Data format - app send:**  

> | Byte Length | (Length of the SSID string) |
> | :--- | :--- |
> | **Meaning** | SSID |

> No '\0' character needed.

> If no data appended, and data length is set to 0, the sensor will return currently saved SSID only.

> **Data format - app receive:**  

> | Byte Length | (Length of the SSID string) |
> | :--- | :--- |
> | **Meaning** | sensor saved SSID |

---

### Tag `0x63`

> **Meaning:**	
> Set/Get the WIFI password, maximum length is 32 char. Data will save to the sensor EEPROM use for next WIFI connection.

> **Data format - app send:**  

> | Byte Length | (Length of the WIFI password string) |
> | :--- | :--- |
> | **Meaning** | WIFI password |

> No '\0' character needed.

> If no data appended, and data length is set to 0, the sensor will return currently saved password only.

> **Data format - app receive:**  

> | Byte Length | (Length of the WIFI password string) |
> | :--- | :--- |
> | **Meaning** | sensor saved WIFI password |

---

### Tag `0x64`

> **Meaning:**	
> Set/Get WIFI static IP, net-mask and default gateway. Data will save to the sensor EEPROM use for next WIFI connection.

> **Data format - app send:**  

> | Byte Length | 4 | 4 | 4 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | static IP | net-mask | gateway |

> Each address is a 32-bits unsigned integer.

> If dynamic IP is used, set the static IP, net-mask and gateway to `0.0.0.0`.

> If no data appended, and data length is set to 0, the sensor will return currently saved IP, net-mask and gateway only.

> **Data format - app receive:**  

> | Byte Length | 4 | 4 | 4 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | saved static IP | saved net-mask | saved gateway |

---








