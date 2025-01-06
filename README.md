## 💾 Data Description

The following table describes the columns in the machine operating data:

| **Column Name**                     | **Data Type**     | **Category**                         |
|-------------------------------------|-------------------|--------------------------------------|
| `Date`                              | datetime64[ns]    | Date/Time (Timestamp of the reading) |
| `Machine_ID`                        | text              | Identifier (Machine Identifier)     |
| `Assembly_Line_No`                  | text              | Identifier (Assembly Line Identifier) |
| `Hydraulic_Pressure(bar)`           | decimal           | Pressure (Hydraulic Pressure)       |
| `Coolant_Pressure(bar)`             | decimal           | Pressure (Coolant Pressure)         |
| `Air_System_Pressure(bar)`          | decimal           | Pressure (Air System Pressure)      |
| `Coolant_Temperature`               | decimal           | Temperature (Coolant Temperature)    |
| `Hydraulic_Oil_Temperature`         | decimal           | Temperature (Hydraulic Oil Temperature) |
| `Spindle_Bearing_Temperature`       | decimal           | Temperature (Spindle Bearing Temperature) |
| `Spindle_Vibration`                 | decimal           | Vibration (Spindle Vibration)       |
| `Tool_Vibration`                    | decimal           | Vibration (Tool Vibration)          |
| `Spindle_Speed(RPM)`                | decimal           | Speed (Spindle Rotational Speed)    |
| `Voltage(volts)`                    | decimal           | Electrical (Machine Voltage)        |
| `Torque(Nm)`                        | decimal           | Force (Machine Torque)              |
| `Cutting(kN)`                       | decimal           | Force (Cutting Force)               |
| `Downtime`                          | text              | Indicator (Machine Downtime)        |
