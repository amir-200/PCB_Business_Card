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

## Relationships

- The **Card Controller** coordinates the card's display, storage, sensing, timekeeping, NFC-related behavior, and expansion interfaces.
- The **Interface Map** defines how the **Card Controller** connects to the LED drivers, USB data path, storage, sensors, RTC, NFC behavior, OLED provision, power-status sensing, and debug access.
- The **Idle Time Display** uses the RTC and display while keeping power draw low.
- The **Shake Animation** uses IMU motion detection to trigger a short fluid simulation burst before returning to the **Idle Time Display**.
- **Motion Wake** is a dedicated IMU interrupt input to the **Card Controller**, separate from the I2C bus.
- The **LED Matrix** is controlled by two TM1640 LED drivers and is used by both the **Idle Time Display** and **Shake Animation**.
- The **Display Map** splits the **LED Matrix** into a top six rows controlled by one TM1640 and a bottom six rows controlled by the other TM1640.

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

## Flagged ambiguities

- "MCU section" was resolved as **Card Controller**, meaning the central controller for all major card behavior rather than only the programming circuitry.
