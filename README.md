# 🏭 Thermal Conveyor Monitoring System & Supervisory Control

An integrated industrial-grade supervisory monitoring and control system developed specifically for heating conveyor production lines. This system provides real-time oversight of thermal energy distribution across both the **Upper Heating Zone** and **Lower Heating Zone**, while simultaneously tracking conveyor speed telemetry.

Built upon the **Modbus TCP/IP** communication protocol, this project integrates the field data acquisition layer from the PLC to a local control panel (**HMI**) and a web-based remote supervisory dashboard **Node-RED**. It is engineered to ensure material heating consistency, prevent product defects caused by temperature fluctuations, and optimize overall thermal efficiency.

---

## ✨ Key Features & Technical Implementation

* **Full-Scale 16-Channel Thermal Tracking:** Continuous data acquisition across 8 upper heating points and 8 lower heating points for precise thermal profiling.
* **Dynamic Set-Value & Process-Value Management:** Real-time synchronization allowing supervisors to adjust target setpoints (SV) while monitoring active field temperatures (PV).
* **Dual-Axis Speed Monitoring & Control:** Comprehensive tracking of target speeds (Speed SV) against physical sensor feedback (Speed PV) to maintain constant production cycle times.
* **High-Performance Modbus TCP/IP Polling:** Utilizes standard Modbus `Read_Holding_Register (4x)` functions for rapid binary data harvesting from PLC memory.
* **Dual-Interface Supervisory Framework:** Seamless, real-time data orchestration between local shop-floor visualization (**HMI**) and cloud/web-centric monitoring (**Node-RED Web Dashboard**).

---

## 🔧 Data Architecture & Modbus Register Mapping

All operational parameters are structured within Modbus holding registers. Temperature data is transmitted as scaled integers with a multiplier factor (scale of `/10 °C` for a 0.1°C decimal resolution), while speed parameters utilize a scale of `/100 M/Min`.

### A. Upper Heating Zone Register Mapping
| Parameter Code | Data Type | PLC Addr | Modbus Addr (4x) | Convert Addr | Functional Description | Unit |
| :--- | :---: | :---: | :---: | :---: | :--- | :---: |
| `UP SV 01` to `08` | UInt16 (RW) | D4101 - D4801 | 46102 - 46802 | 6101 - 6801 | Upper Heating Zone 1–8 Target Temperature | /10 °C |
| `UP PV 01` to `04` | UInt16 (RO) | D1010 - D1013 | 43011 - 43014 | 3010 - 3013 | Upper Heating Zone 1–4 Actual Temperature | /10 °C |
| `UP PV 05` to `08` | UInt16 (RO) | D1160 - D1163 | 43161 - 43164 | 3160 - 3163 | Upper Heating Zone 5–8 Actual Temperature | /10 °C |

### B. Lower Heating Zone Register Mapping
| Parameter Code | Data Type | PLC Addr | Modbus Addr (4x) | Convert Addr | Functional Description | Unit |
| :--- | :---: | :---: | :---: | :---: | :--- | :---: |
| `DOWN SV 01` to `08` | UInt16 (RW) | D4901 - D5601 | 46902 - 47602 | 6901 - 7601 | Lower Heating Zone 1–8 Target Temperature | /10 °C |
| `DOWN PV 01` to `04` | UInt16 (RO) | D1310 - D1313 | 43311 - 43314 | 3310 - 3313 | Lower Heating Zone 1–4 Actual Temperature | /10 °C |
| `DOWN PV 05` to `08` | UInt16 (RO) | D1460 - D1463 | 43461 - 43464 | 3460 - 3463 | Lower Heating Zone 5–8 Actual Temperature | /10 °C |

### C. Conveyor Speed System Register Mapping
| Parameter Code | Data Type | PLC Addr | Modbus Addr (4x) | Convert Addr | Functional Description | Unit |
| :--- | :---: | :---: | :---: | :---: | :--- | :---: |
| `UP SPEED SV` | UInt16 (RW) | D5800 | 47801 | 7800 | Upper Conveyor Target Speed | /100 M/Min |
| `DOWN SPEED SV` | UInt16 (RW) | D5700 | 47701 | 7700 | Lower Conveyor Target Speed | /100 M/Min |
| `UP SPEED PV` | UInt16 (RO) | D3790 | 45791 | 5790 | Upper Conveyor Actual Speed | /100 M/Min |
| `DOWN SPEED PV` | UInt16 (RO) | D3780 | 45781 | 5780 | Lower Conveyor Actual Speed | /100 M/Min |

---

## 🖥️ Operator Interface & Diagnostic Web Dashboard

The supervisory network provides an intuitive, real-time synchronized layout tailored for multi-platform operations:

### Core Dashboard Visual Components (HMI & Node-RED)
* **Live Thermal Trend Charts:** Dynamic curve plotting for all 16 temperature channels (`PV 01 - PV 08`) to instantly track thermal stability against target setpoints (`SV`).
* **Speed Gauges & Slip Indicators:** Dial-gauge visualizations comparing `SPEED SV` with `SPEED PV` to rapidly detect motor overloading, mechanical drag, or slippage.
* **Setpoint Controls:** Interactive numeric input elements allowing authorized operators to modify operational targets (`SV`) backed by safe high/low boundary validation.
