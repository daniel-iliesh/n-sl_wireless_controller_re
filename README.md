# N-SL WirelessController (Model NO.SW001) Reverse Engineering Project

## Project Goal

To understand the hardware and firmware of the controller and potentially modify the firmware to improve compatibility and functionality with Android TV, as a learning project in low-level development and reverse engineering.

## Known Details & Findings So Far

* **Controller Model:** N-SL WirelessController, Model NO.SW001.
* **Official Compatibility (according to limited manual):** Nintendo Switch (Bluetooth), PC (wired/X-Input).
* **Observed Compatibility:** Connects via Bluetooth to Android TV but does not function correctly. Some online sources and manual mention of X-Input suggest potential for broader compatibility.
* **Board Marking:** BM-829 (SW01) -V1.4A. (Specific technical documentation for this board version is not readily available).
* **Identified Components on Board:**
    * **Crystal Oscillator:** 26000MHZ (26 MHz). Provides the main clock signal.
    * **EEPROM:** 24C02N (2 Kilobit, I²C interface). Likely stores configuration data, pairing information, or calibration data.
    * **Other Chips:** RDK 5876, ATHYC8o2, SU27 D. Roles are not yet clearly identified through initial searches. One of these could be the Bluetooth chip or other supporting ICs.
    * **Main Chip (Microcontroller - MCU):** Unmarked, with a dot in the top right corner (indicating Pin 1). This is the central processing unit of the controller and the primary target for firmware analysis.

## Project Checklist

Here are the steps you can take to continue your investigation and learning, moving from hardware analysis to firmware:

**Hardware Analysis & Identification:**

* - [ ] **Detailed Examination of Unmarked Chip:**
    * Note the exact package type (e.g., QFP, QFN).
    * Count the precise number of pins.
    * Measure the chip's physical dimensions.
* - [ ] **Analyze Connections of Unmarked Chip:**
    * Use a multimeter in continuity mode to trace connections from the unmarked chip's pins to:
        * The 26 MHz crystal.
        * The 24C02N EEPROM (identify SCL and SDA pins).
        * The RDK 5876, ATHYC8o2, and SU27 D chips.
        * The Bluetooth antenna.
        * The USB port (power, data lines).
        * The battery connector.
        * Buttons, joysticks, and LEDs.
        * Vibration motor(s).
* - [ ] **Research RDK 5876, ATHYC8o2, and SU27 D:** Conduct more in-depth searches for these markings, perhaps combined with terms like "Bluetooth IC", "gamepad controller chip", or names of potential manufacturers like "iPega". Look for any datasheets, application notes, or forum discussions where these markings appear in a relevant context.
* - [ ] **Infer Pin Functionality:** Based on the connections traced, deduce the likely function of various pins on the unmarked MCU (e.g., power, ground, I/O, communication interfaces).
* - [ ] **Identify Potential Debugging Interfaces:** Look for unpopulated headers, test points, or clusters of pads on the PCB that match standard JTAG, SWD, or UART debugging interface pinouts.

**Firmware Interaction & Analysis:**

* - [ ] **Attempt to Connect with Debugging Tools (If Interface Found):** If you identify a potential debugging interface, acquire a compatible hardware debugger (e.g., ST-Link, J-Link, or a generic adapter) and use software like OpenOCD to attempt to connect to the unmarked chip.
* - [ ] **Attempt Firmware Extraction:** If you can successfully connect to the chip via a debugging interface and if read protection is not enabled, attempt to read the firmware from the chip's Flash memory.
* - [ ] **Analyze Firmware:** If you extract the firmware binary:
    * Use a disassembler (e.g., Ghidra, IDA Pro, Binary Ninja) to convert the machine code to assembly language.
    * Analyze the assembly code to understand the firmware's structure, initialization routine, input handling logic, Bluetooth communication stack interaction, and different mode implementations.
* - [ ] **Understand Bluetooth HID for Gamepads:** Study the technical specifications for the Bluetooth Human Interface Device (HID) profile, particularly how gamepads report input data to the host (like Android).

**Modification & Testing (Highly Advanced & Risky):**

* - [ ] **Identify Code for Android/X-Input Mode:** Based on your firmware analysis, pinpoint the code responsible for handling the X-Input or a potentially hidden Android mode.
* - [ ] **Implement Firmware Modifications:** Write or modify assembly code to implement the desired behavior for Android compatibility (e.g., changing the HID report descriptor or the way input data is formatted). This requires a deep understanding of the chip's instruction set and the Bluetooth stack implementation in the firmware.
* - [ ] **Flash Modified Firmware:** Use appropriate tools and the debugging/programming interface to flash your modified firmware back onto the controller. **(Be aware of the high risk of bricking the device at this stage).**
* - [ ] **Test on Android TV:** Test the controller's functionality on your Android TV with the modified firmware.
