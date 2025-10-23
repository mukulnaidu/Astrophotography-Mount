# Navigator II — Dual-Axis Equatorial Mount Controller

An Arduino-based precision controller for an astrophotography equatorial mount, designed to automate sidereal tracking, camera exposure sequences

---

## Project Overview
Navigator II is a fully self-contained mount controller that integrates mechanical control, coordinate computation, and camera triggering. 
Users can zero the mount, jog in Right Ascension and Declination, automate camera exposures and maintain tracking

This system replaces the need for commercial GoTo mounts by implementing its own coordinate transformation and limit switch calibration logic.

---
## Hardware Components
- **Microcontroller:** Arduino Uno R4 Minima
- **Motors:** 2× NEMA 17 stepper motors
- **Drivers:** TMC2208 stepper motor drivers (configured for 1/16 microstepping)
- **Limit switches:** 2× microswitches for zeroing
- **LCD:** 16×2 Arduino LCD display
- **Camera trigger:** 2.5mm jack to DSLR remote port, with one end cut to expose wires
- **Optocoupler** Optocoupler to protect camera from high voltages
- **Analog buttons:** 5-button resistor ladder 
- **Power:** 18V regulated input shared between logic and steppers

---
##Software Architecture
Firmware is written in C++ for arduino using **AccelStepper** and **LiquidCrystal** Libraries

### Core Functions
| Function | Purpose |
|-----------|----------|
| `setZeroFunction()` | Homes both RA and Dec axes using limit switches, allows user to initiate coordinates |
| `jogFunction()` | Allows manual control of axes for targeting of celestial objects |
| `trackingFunction()` | Runs sidereal tracking and triggers light exposures |
| `CameraCalibrationFrames()` | Automates dark frames |
| `SetCamera()` | Configures number and duration of light exposures |

The program uses **non-blocking timing** (`millis()`) for menu refreshes and movement updates, ensuring smooth operation and no loss of tracking. Even when tracking function is not active, the mount pans at the correct rate to lock the user's target in the sky.

---

## Coordinate Logic
- RA and Dec are updated in arcseconds using `addOrSubtractToRA()` and `addOrSubtractToDec()`.
- The code handles pole and equator crossings mathematically, flipping sign and quadrant when needed, since declination operates from 0 to ±90 degrees, and RA adds 12h as it passes the pole.
- Stepper motion rate: ~45.37 microsteps/sec for sidereal rate.

---

## Usage
1. Power the system (18V input).  
2. Use menu options to:
   - **Zero:** Home both axes using limit switches.  
   - **Jog:** Manually align telescope.  
   - **Camera:** Set number of exposures and duration.  
   - **Track:** Begin automated tracking and imaging.
   - - **Darks:** Set number of dark exposures and duration. 
3. Exit any mode by pressing the analog "exit" button.

---

## Author
**Mukul Naidu**  
MIT Maker Portfolio Submission, 2025
