# PCB Business Card

A battery-powered interactive PCB business card with display, USB, sensing, storage, and expansion features.

## Language

**Card Controller**:
The main MCU that coordinates the card's display, USB content update path, storage, sensing, timekeeping, NFC-related behavior, and expansion interfaces.
_Avoid_: MCU section, microcontroller block, programming chip

**Interface Map**:
The agreed assignment of card subsystems to controller buses, pins, headers, and test points.
_Avoid_: rough wiring, pin guesses, MCU hookup

**Idle Time Display**:
The low-activity display mode where the card shows the current time while resting.
_Avoid_: standby screen, clock mode

**Shake Animation**:
The short motion-triggered display mode where the card runs the fluid simulation after being shaken.
_Avoid_: fun mode, active mode, animation mode

**Motion Wake**:
The IMU interrupt signal that tells the Card Controller to start a Shake Animation.
_Avoid_: shake polling, movement check

**LED Matrix**:
The 21 by 12 rectangular display made from 252 LEDs on one side of the card.
_Avoid_: LEDs, display lights

**Display Map**:
The coordinate and driver ownership map for the LED Matrix.
_Avoid_: LED order, driver split

**Debug Pads**:
Low-profile physical pads that expose Card Controller bring-up and recovery signals without using a visible header.
_Avoid_: debug header, programming connector

**BOOTSEL Button**:
A tiny physical button used to request USB boot mode during Card Controller reset or power-up.
_Avoid_: reset button, dual-purpose button

**Program Flash**:
The external 16 MB QSPI flash that stores the Card Controller firmware and related program data.
_Avoid_: storage flash, memory chip, content storage

**Content Partition**:
The user-editable region of Program Flash exposed over USB mass storage for display content updates.
_Avoid_: files, MSC memory, flash storage

**USB Update Mode**:
The powered-by-USB mode where the Card Controller exposes the Content Partition as a mass storage device.
_Avoid_: programming mode, USB mode

**VBUS Sense**:
A Card Controller input that detects whether USB power is present.
_Avoid_: USB power wire, charger input

**Battery Sense**:
A power-conscious Card Controller measurement of battery voltage for brightness and runtime decisions.
_Avoid_: raw battery pin, battery wire

## Relationships

- The **Card Controller** coordinates the card's display, storage, sensing, timekeeping, NFC-related behavior, and expansion interfaces.
- The **Interface Map** defines how the **Card Controller** connects to the LED drivers, USB data path, storage, sensors, RTC, NFC behavior, OLED provision, power-status sensing, and debug access.
- The **Idle Time Display** uses the RTC and display while keeping power draw low.
- The **Shake Animation** uses IMU motion detection to trigger a short fluid simulation burst before returning to the **Idle Time Display**.
- **Motion Wake** is a dedicated IMU interrupt input to the **Card Controller**, separate from the I2C bus.
- The **LED Matrix** is controlled by two TM1640 LED drivers and is used by both the **Idle Time Display** and **Shake Animation**.
- The **Display Map** splits the **LED Matrix** into a top six rows controlled by one TM1640 and a bottom six rows controlled by the other TM1640.
- **Debug Pads** expose SWD, reset or run control, power reference, and ground for Card Controller bring-up.
- The **BOOTSEL Button** is separate from **Debug Pads** and does not replace reset or SWD recovery access.
- **Program Flash** is connected to the Card Controller through the dedicated QSPI interface.
- The **Content Partition** lives inside **Program Flash** and is exposed by the Card Controller over USB mass storage.
- **USB Update Mode** is entered when USB is connected and uses **VBUS Sense** to distinguish USB-powered behavior from battery behavior.
- **Battery Sense** helps the Card Controller limit LED brightness and Shake Animation behavior when battery voltage is low.

## Example dialogue

> **Dev:** "Should we work on the MCU section next?"
> **Domain expert:** "Yes, but treat it as the **Card Controller**, because its pin and interface choices affect nearly every remaining subsystem."

> **Dev:** "Can I place the controller symbol first and wire as I go?"
> **Domain expert:** "No, define the **Interface Map** first so pin choices do not accidentally block USB, flash, display, debug, or expansion."

> **Dev:** "Should the fluid simulation run all the time?"
> **Domain expert:** "No, the card normally shows the **Idle Time Display** and only runs the **Shake Animation** for a short timed burst after motion."

> **Dev:** "Can firmware just poll the IMU for shake events?"
> **Domain expert:** "No, use **Motion Wake** so shake detection is available without constant I2C polling."

> **Dev:** "Is the display just a pile of LEDs?"
> **Domain expert:** "No, it is a 21 by 12 **LED Matrix**, so routing and firmware need an explicit coordinate map."

> **Dev:** "Can we split the 21 by 12 matrix into left and right driver halves?"
> **Domain expert:** "No, use the **Display Map** because an 11-column half would exceed one TM1640's 128-LED capacity."

> **Dev:** "Can we skip debug access because USB programming exists?"
> **Domain expert:** "No, use **Debug Pads** so first-board bring-up has a recovery path."

> **Dev:** "Can one button act as both reset and BOOTSEL?"
> **Domain expert:** "No, use a tiny **BOOTSEL Button** and keep reset or run control on **Debug Pads**."

> **Dev:** "Can any SPI flash hold the firmware?"
> **Domain expert:** "No, use **Program Flash** that follows the RP2350A reference QSPI expectations."

> **Dev:** "Does the USB mass storage device need a separate storage chip?"
> **Domain expert:** "No, expose a **Content Partition** inside **Program Flash** for user-editable display content."

> **Dev:** "Can firmware just assume USB is present when data lines enumerate?"
> **Domain expert:** "No, add **VBUS Sense** so **USB Update Mode** and battery behavior can be separated cleanly."

> **Dev:** "Can the controller read battery voltage directly?"
> **Domain expert:** "No, use **Battery Sense** so voltage measurement is safe for the Card Controller and does not waste coin-cell current."

## Flagged ambiguities

- "MCU section" was resolved as **Card Controller**, meaning the central controller for all major card behavior rather than only the programming circuitry.
