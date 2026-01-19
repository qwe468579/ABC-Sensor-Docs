# LED Command

Tag `0x20`-`0x2f` command for LED are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set as 0x00.

---

### Tag `0x20`

> **Meaning:**	
> Get LED color.

> **Data format - app send:**  
> None

> **Data format - app receive:**  

> | Byte Length | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | Red value | Green value | Blue value |

> Each RGB color value from 0-255.

> If sensor LED is set to blink mode, this command will always report the LED origin color, no matter it is under the "off time" during blinking or not.

---

### Tag `0x21`

> **Meaning:**	
> Set LED brightness.

> **Data format - app send:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | brightness value |

> Unsigned integer, valid value is 0-255. The default brightness is 3.

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge after command is executed.

---

### Tag `0x22`

> **Meaning:**	
> Set LED color, can be set as static color or blinking pattern. This command will not override the sensor default LED behavior.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | 1 | 1 (optional) | 1 (optional) |
> | :--- | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Red Value | Green Value | Blue Value | On Time (optional) | Off Time (optional) |

> Each RGB color value range is 0-255.

> **On Time** and **Off Time** range is 0-255 (0s-5.66s), 22.2ms LSB. **On Time** and **Off Time** are optional, if it is set, LED will work under blinking mode.

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge after command is executed.

---

### Tag `0x23`

> **Meaning:**  
> Change LED color, keep the current LED pattern. Only use this command after `0x22` set LED color command is called. This command execute faster than `0x22`.

> **Data format - app send:**  

> | Byte Length | 1 | 1 | 1 |
> | :--- | :--- | :--- | :--- |
> | **Meaning** | Red Value | Green Value | Blue Value |

> Each RGB color value range is 0-255.

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge after command is executed.

---

### Tag `0x24`

> **Meaning:**  
> Completely disable the LED function of the sensor, this command can be used to override the default LED behavior of the sensor, close the LED completely for power saving.

> **Data format - app send:**  

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | Function Code |

> | Function Code | Meaning |
> | :--- | :--- |
> | 0x00 | Enable LED function to default state |
> | 0x01 | Disable LED with lock on |
> | 0x02 | Disable LED without lock on |
> | 0x03 | Lock on but not disable led |

> When LED is disabled, LED will turn off and remain in low power consumption.

> When LED is locked, the LED color cannot be changed any more. It will remain the last color and pattern set before it get locked.

> **Data format - app receive:**  
> Same as send format.

> Same format will acknowledge after command is executed.

---
