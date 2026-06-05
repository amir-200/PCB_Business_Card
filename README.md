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

## Collaboration

- Work on short feature branches for each hardware section, for example `feature/card-controller`.
- Only one person should edit the KiCad schematic/PCB files at a time.
- Push branches to GitHub and review changes before merging to `main`.
- Do not commit KiCad lock files, local history, generated DRC/ERC reports, or temporary files.
