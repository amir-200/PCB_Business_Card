# PCB_Business_Card

## Final requirements 

1. LED Matrix
2. Battery powered
3. PCB Edge USB-C Connector (For programming/ act as MSC (mass storage class) device to update content)
4. MSC(Mass Storage Class) device so the content of the display can be updated easily
5. Fluid Simulation
6. RTC
7. NFC
8. Provision for interfacing an OLED display 

## Technical Specs

1. Double sided 2 layer PCB
2. 252 LEDs (Yellow/ White)
4. TM1640 LED drive controller IC x 2
5. PCB Edge USB-C Connector 
6. RP2350A MCU 
7. ST25TN01K NFC Forum Type 2 Tag IC
8. TP4057 Linear battery charger
9. QMI8658 IMU 
10. External Flash 
11. LIR2032 Coin cell
12. MOQ : 5

## TODO 

[ ] USB circuitry  : Mihir

[ ] battery charging : amir

[ ] Card Controller foundation: RP2350A power, USB data, flash, I2C bus, Motion Wake interrupt, TM1640 control pins, debug/programming support, and optional power-status sensing

## Card Controller Interface Table

| Subsystem | Candidate part | Interface | Controller signals | Current decision |
| --- | --- | --- | --- | --- |
| Program flash and content storage | Winbond W25Q128JVSIQ, 16 MB | Dedicated RP2350A QSPI | QSPI SCLK, SS, SD0-SD3 | Use one flash with a firmware area and MSC-exposed content partition |
| LED matrix | TM1640 x2 | TM1640 two-wire control | Driver A CLK/DIO, Driver B CLK/DIO | 21 x 12 matrix split as top 6 rows and bottom 6 rows |
| RTC | DS3231 candidate | I2C | SDA, SCL | Revisit later; DS3231 is preferred over DS1307 for 3V3 operation |
| IMU | QMI8658 | I2C plus interrupt | SDA, SCL, IMU_INT | Use dedicated Motion Wake interrupt |
| NFC tag | ST25TN01K | I2C if MCU configuration is needed | SDA, SCL, optional interrupt | Keep on shared I2C unless address or datasheet review says otherwise |
| OLED provision | TBD OLED module | SPI preferred | SCLK, MOSI, CS, DC, RESET | Keep as provision/header; do not share this assumption with TM1640 control |
| USB update | PCB edge USB-C | USB 2.0 plus VBUS sense | USB_DP, USB_DM, VBUS_SENSE | USB exposes MSC content partition and powers/charges card |
| Debug and recovery | Pads plus tiny BOOTSEL button | SWD and boot/reset access | SWDIO, SWCLK, RUN pad, GND, 3V3, BOOTSEL button | Low-profile debug pads; one tiny BOOTSEL button |
| Battery status | TBD divider/switch | ADC sense | BATTERY_SENSE | Include power-conscious battery voltage measurement |

## Collaboration

- Work on short feature branches for each hardware section, for example `feature/card-controller`.
- Only one person should edit the KiCad schematic/PCB files at a time.
- Push branches to GitHub and review changes before merging to `main`.
- Do not commit KiCad lock files, local history, generated DRC/ERC reports, or temporary files.
