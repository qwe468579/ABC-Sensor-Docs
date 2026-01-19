#Storage Command

Tag `0x40`-`0x4f` command for storage function are explained in this page.

---

### Keyword

> **NA**: Not applicable, this command should no be sent by the sensor/application.

> **None**: No data contained in this command, data length can be set to 0x00.

---

### Tag `0x40`

> #### Meaning:
> Read offline recorded data status and header.

> #### Data Format - Input:

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | Storage |

> | Storage | Code | For Data Type |
> | :--- | :--- | :--- |
> | Record Status | 0x00 | |
> | EEPROM Header | 0x01 | Press and Temp |
> | SD Card Header | 0x02 | IMU or Resistive Sensor |

> #### Data Format - Output:

> If storage selected Record Status (0x00):

> | Byte Length | 1 |
> | :--- | :--- |
> | **Meaning** | Record Status |

> | Record Status | Code |
> | :--- | :--- |
> | Storage Record Off | 0x00 |
> | Storage Record On | 0x01 |
> | Storage Record On with no Adv | 0x02 |

> If storage selected header (0x01 or 0x02):

> | Byte Length | 2 | 8 | 8 | 8 | 8 | 8 | 4 | 1 |
> | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
> | **Meaning** | Data Type | Data Period Tick | Start Unix Time | Start Sys Tick | End Unix Time | End Sys Tick | Data Count | Recording |

> **Data Type** represent by setting the following bit to 1, means that data type is recorded.

> | Bit Order | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11-15 |
> | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
> | Data Type | Accel XYZ | Gyro XYZ | Mag XYZ | Quat WXYZ | Press and Temp | Resis CH0 | Resis CH1 | Resis CH2 | Resis CH3 | Resis CH4 | Resis CH5 | Reserve |

> | Data Type | Type | Length | Remark |
> | :--- | :--- | :--- | :--- |
> | Accel XYZ | Float + FLoat + Float | 12 | Accelerometer Data |
> | Gyro XYZ | Float + FLoat + Float | 12 | Gyroscope Data |
> | Mag XYZ | Float + FLoat + Float | 12 | Magnetometer Data |
> | Quat WXYZ | Float + FLoat + Float + Float | 16 | Quaternion 6x or 9x |
> | Press and Temp | Float + Float | 8 | Air Pressure and Temperature Data |
> | Resis CHX | Signed Integer | 2 | Resistive Sensor Data |

> If **End Sys Tick** is 0x00, means sensor is recording.

---

### Tag `0x41`

> #### Meaning:

> Quarter page read data from storage.

> #### Data Format - Input:

> | Byte Length | 1 | 4 |
> | :--- | :--- | :--- |
> | **Meaning** | Storage | Storage Address |

> | Storage | Code |
> | :--- | :--- |
> | EEPROM | 0x01 |
> | SD Card | 0x02 |

> **Storage Address:** Page address of the storage, each page has 512 bytes.

> #### Data Format - Output:

> | Byte Length | 128 |
> | :--- | :--- |
> | **Meaning** | Data |

> **Data:** 128 bytes length for each read, call this command 4 times to read a complete 512 bytes page.

---

### Tag `0x42`

> #### Meaning:
> Quarter page write to storage.

> #### Data Format - Input:

> | Byte Length | 1 | 4 | 128 |
> | :--- | :--- | :--- | : --- |
> | **Meaning** | Storage | Storage Address | Data |

> | Storage | Code |
> | :--- | :--- |
> | EEPROM | 0x01 |
> | SD Card | 0x02 |

> **Storage Address:** Page address of the storage, each page has 512 bytes.

> **Data:** Data write to the storage.

> Call this command 4 times at same address to write one page storage.

> #### Data Format - Output:

> None.

> Acknowledge to host means ready to receive next data.

---
























