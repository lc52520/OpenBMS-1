Intro
==========
* This project aims to create an open source, scalable Battery management system for use in just about anything with more than 4 lithium batteries. (and currently up to 96 cells without a second master board)
* It aims to be as open source as possible all software should ideally be free and unlimited. All PCB designs are done in KiCad (fairly recent dev builds) and all programming is done in ..... arduino? (deciding between arduino and something like eclipse with an external compiler)

You can find a discussion thread for this project here on the Endless Sphere fourm: http://endless-sphere.com/forums/viewtopic.php?f=14&t=63863
Goals
==========
* Open source - it seems that almost all gms systems on the market are closed down these days. we want to be able to add whatever features we please as the EV world gets ever more integrated and complex
* Low cost - The BMS IC was selected to be as low cost as possible with a daisy chain approach used to further attempt to reduce costs through high manufacturing volume. Goal of $2 per cell can be reached. Master board may bump that up for smaller systems.
* Reliable - Even though a goal is to keep costs low, reliability should not be unnecessarily decreased for small cost improvements
* Safe - in going with reliable, I don't want this thing to start any fires or cause any damage. I want it to prevent any possible catastrophes associated with using such a power dense energy source.
* Easy to use - Board is programmed in the commonly known Arduino, but may use an rtos like chibios to facilitate low power-modes and can/usb tasks. Integration with a larger system could possibly be done with ladder logic programming.
* Multi chemistry - Can monitor and balance almost anything with "lithium" in the name. (lipo, lifepo4, etc. Possibly even super-capacitor banks)

Boards
===============
There are several boards contained within this project

* The Master board. it contains the mcu (The same microcontroller used in the teensy 3.1) The CAN transceiver and a couple other odds and ends such as a contactor driver.
* The MicroMaster board. This one contains an atmega328 with current sensing and little else. 
This board is meant as a lower cost alternative to the regular master board. It can also be used to control packs >96 cells by connecting it to a master via optoisolators.
* The daughter cell management board. It contains the voltage measuring ic and balancing resistors.
* The bus termination board. This one does precious little except ties the datapins of the top slave board together and allow activation of the system with an optoisolator.


Master Features (plannned)
===============
* Teensy 3.1 microcontroller (MK20DX256VLH7)
* USB programmable with a possibility of usb otg for android devices / dashboards. (and could charge phones/tablets with switching regulator)
* CAN transceiver for data connection to other modules.
* Connectors for an i2c/serial LCD dashboard and all extra pins broken out for auxiliary I/O.
* Input voltage sensing
* Switching voltage regulator to bring pack voltage down to 5-12v when system is on for auxilary functions.
* 4 mosfet controlled low side outputs. These can be used to control a contactor, precharge circuit relay, fan, etc.
* 3 dedicated current sensor inputs. one for charging, one for the motor and one on-board for monitoring auxiliary systems.
* SD card slot for datalogging (and possible configuration via txt file)
* Analog and pwm throttle input & output. This can map the throttle to different values, change a analog signal to a pwm one, or/and cut/hold back throttle when the pack is almost empty/dead.
* Opto isolators for controlling charger or communicating to voltage shifted masters.
* Buzzer / alarm - If something goes severely wrong, this board will let you know


MicroMaster Features (plannned)
===============
* Atmega328 microcontroller programmable with arduino with usb -> serial adapter
* Runs completely off power output from slave board/s - any extra modules must consume less that 10ma or be run off an external voltage supply.
* 2 dedicated current sensor inputs
* 2 mosfet controlled outputs
* SD card slot for datalogging
* Opto isolators for controlling charger or communicating to voltage shifted masters. - all serial communication will be isolated.
* Connectors for an i2c/serial LCD dashboard (will not be able to supply power) and all extra pins broken out for auxiliary I/O.
* Analog and pwm throttle input & output. This can map the throttle to different values, change a analog signal to a pwm one, or/and cut/hold back throttle when the pack is almost empty/dead.


Slave Board Features (plannned)
===========
* Measures 4-6 Li-Ion Cells in Series 
* Stackable Architecture Enables >400V Systems (Up to 16 6s modules)). - (Micro)Master modules can be isolated and daisy chained  for even higher voltages 
* 0.25% Maximum Total Measurement Error (12 bit adc) - This will be tested in reality once the first boards are constructed.
* Extremely low power when asleep. (<100ua)
* Two Thermistor Inputs - One connected to an On-Board Temperature Sensor and the other one brought out to a header.
* temperature management. software will start to pwm balance resistors of board gets too hot.
* Cell Balancing:
===========
        * Balancing achieved with 10 ohm power resistors. This means ~1.8 watts power of dissipation per cell. Heatsinking is probably needed at this level for full balancing current.
        * The onboard thermistor will reduce the duty cycle of the resistors in 25% increments (from 25-100%) to keep the board from overheating / melting.
        * Predictive cell balancing? If a cell always needs a certain amount of energy removed while charging the board will pre-emptively balance the cell. I know of no example of this and will investigate its utility 

    

software Goals
===========
* Readout of all individual cell voltages and analog inputs
* Manually control system outputs 
* Dataloggging, including current, cell voltages, and recording of total time the balance resistor has been on for each cell. useful for identifying weak/strong cells.
* Cell resistance estimate - use amperage draw and cell voltages to estimate cell resistances.
* Coulomb Counting for entire battery pack as well as cumulative coulumbs in/out for battery wear estimation

  
  
Photos
===========
![board](https://github.com/Teslafly/OpenBMS/blob/master/Docs/photos/Open%20source%20BMS.jpg?raw=true)
![schematic] (https://raw.githubusercontent.com/Teslafly/OpenBMS/master/Docs/photos/Open%20source%20BMS.png)


TODO:
===============
* Actually write code for the thing
* finish and post the micromaster board
* make a separate folder for all the kicad libraries and 3d models used in the project
* write a blog post on teslafly.wordpress.com detailing this project.
* make this description better
* make some prototypes 



License
===========
TBA