# HARem Hardware Pinout

This document outlines the hardware connections for the HARem Remote Controller (based on ESP32-C3 SuperMini).

## Pin Configuration

| GPIO Pin | Function | Mode | Notes |
| :--- | :--- | :--- | :--- |
| **GPIO 0** | Battery Voltage Monitor | ADC | Through voltage divider (Ultra Low Power: 2x 1MΩ + 0.1µF) |
| **GPIO 1** | OLED Power Supply | OUTPUT | Powers the display. Held LOW in deep sleep. |
| **GPIO 4** | Encoder Button | INPUT_PULLUP | Inverted, Used as Deep Sleep Wakeup Source |
| **GPIO 5** | I2C SDA | I2C Data | Connect to OLED SDA |
| **GPIO 6** | I2C SCL | I2C Clock | Connect to OLED SCL |
| **GPIO 7** | Free | - | - |
| **GPIO 8** | Encoder Pin B | INPUT_PULLUP | Rotary Encoder Clock/Data |
| **GPIO 9** | Encoder Pin A | INPUT_PULLUP | Rotary Encoder Clock/Data |

## Power Connections

- **3.3V**: Main power rail for Peripherals (Encoder, etc.)
- **GND**: Common Ground
- **5V / VBUS**: Charging Input (SuperMini handles charging)

## Power Consumption (Voltage Divider)
With the **Ultra Low Power** design (2x 1MΩ):
- **Resistance**: 2,000,000 Ω (2 MΩ)
- **Max Current (at 4.2V)**: `4.2V / 2MΩ = 2.1 µA`
- **Previous Design (2x 100k)**: `21 µA`
- **Savings**: ~90% reduction in divider leakage.


## Connection Diagram

```mermaid
graph LR
    subgraph ESP32_C3_SuperMini [ESP32-C3 SuperMini]
        GPIO0[GPIO 0<br>ADC0]
        GPIO4[GPIO 4<br>Wakeup]
        GPIO5[GPIO 5<br>SDA]
        GPIO6[GPIO 6<br>SCL]
        GPIO1[GPIO 1<br>Power]
        GPIO8[GPIO 8<br>Enc B]
        GPIO9[GPIO 9<br>Enc A]
        V33[3.3V]
        GND[GND]
    end

    subgraph Peripherals
        subgraph OLED [1.3" OLED Display]
            O_VCC[VCC]
            O_GND[GND]
            O_SDA[SDA]
            O_SCL[SCL]
        end

        subgraph Encoder [Rotary Encoder]
            E_SW[SW]
            E_DT[DT]
            E_CLK[CLK]
            E_COM[COM]
            R1[10k Pullup]
            R2[100 Ohm]
            R3[100 Ohm]
            R4[100 Ohm]
            C1[100nF]
        end

        subgraph Battery_Mon [Battery Monitor]
            BAT[+5VD / BAT]
            R5[1M]
            R6[1M]
            C2[0.1uF]
        end
    end

    %% Wiring
    GPIO1 -->|Power| O_VCC
    GPIO5 --- O_SDA
    GPIO6 --- O_SCL
    GND --- O_GND

    GPIO4 --- R2
    R2 --- E_SW
    E_SW --- R1
    R1 --- V33
    E_SW --- C1
    C1 --- GND

    GPIO8 --- R3
    R3 --- E_CLK
    GPIO9 --- R4
    R4 --- E_DT
    E_COM --- GND
    GND --- E_GND

    BAT --- R5
    R5 --- GPIO0
    GPIO0 --- R6
    R6 --- GND
    
    %% Capacitor for stability
    GPIO0 --- C2
    C2 --- GND
```

