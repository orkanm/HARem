# HARem Hardware Pinout

This document outlines the hardware connections for the HARem Remote Controller (based on ESP32-C3 SuperMini).

## Pin Configuration

| GPIO Pin | Function | Mode | Notes |
| :--- | :--- | :--- | :--- |
| **GPIO 0** | Battery Voltage Monitor | ADC | Through voltage divider (Internal multiplier x2 in config) |
| **GPIO 4** | Encoder Button | INPUT_PULLUP | Inverted, Used as Deep Sleep Wakeup Source |
| **GPIO 5** | I2C SDA | I2C Data | Connect to OLED SDA |
| **GPIO 6** | I2C SCL | I2C Clock | Connect to OLED SCL |
| **GPIO 7** | OLED Power Supply | OUTPUT | Powers the display. Held LOW in deep sleep. |
| **GPIO 8** | Encoder Pin A | INPUT_PULLUP | Rotary Encoder Clock/Data |
| **GPIO 9** | Encoder Pin B | INPUT_PULLUP | Rotary Encoder Clock/Data |

## Power Connections

- **3.3V**: Main power rail for Peripherals (Encoder, etc.)
- **GND**: Common Ground
- **5V / VBUS**: Charging Input (SuperMini handles charging)

## Connection Diagram

```mermaid
graph LR
    subgraph ESP32_C3_SuperMini [ESP32-C3 SuperMini]
        GPIO0[GPIO 0<br>ADC]
        GPIO4[GPIO 4<br>Wakeup]
        GPIO5[GPIO 5<br>SDA]
        GPIO6[GPIO 6<br>SCL]
        GPIO7[GPIO 7<br>Power]
        GPIO8[GPIO 8<br>Enc A]
        GPIO9[GPIO 9<br>Enc B]
        V33[3.3V]
        GND[GND]
    end

    subgraph Peripherals
        subgraph OLED [OLED Display]
            O_VCC[VCC]
            O_GND[GND]
            O_SDA[SDA]
            O_SCL[SCL]
        end

        subgraph Encoder [Rotary Encoder]
            E_SW[SW]
            E_DT[DT]
            E_CLK[CLK]
            E_VCC[+]
            E_GND[GND]
        end

        subgraph Battery_Mon [Battery Monitor]
            BAT[Battery +]
            R1[100k]
            R2[100k]
        end
    end

    %% Wiring
    GPIO7 -->|Power| O_VCC
    GPIO5 --- O_SDA
    GPIO6 --- O_SCL
    GND --- O_GND

    GPIO4 --- E_SW
    GPIO8 --- E_DT
    GPIO9 --- E_CLK
    V33 --- E_VCC
    GND --- E_GND

    BAT --- R1
    R1 --- GPIO0
    GPIO0 --- R2
    R2 --- GND
```

