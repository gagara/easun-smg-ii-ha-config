# easun-smg-ii-ha-config

This fork contains Home Assistant configuration to monitor and control a EASUN ISolar SMG II 8/11kW inverter models (with 2 MPPTS, feed-to-grid option, and no-parallel support) via USB or RS232 cable.

(https://www.powland.net/collections/isolar-smg-ii/products/isolar-smg-ii-11kw-48v-wifi)

# Installation

Copy easun_smg_ii*.yaml files to the config directory or subdirectory.
In the easun_smg_ii.yaml file, set the correct port if other than ttyUSB0.

Add the following to /config/configuration.yaml:
```bash
homeassistant:
   packages:
     easun_smg_ii: !include [subdirectory/]easun_smg_ii.yaml
````

## Modbus protocol description for Easun ISolar SMG II 8/11kW

> NB: Original doc was re-translated several times between different languages, thus some info might be inaccurate.

> NB2: Some options found here are not documented in official manual. Use them on your own risk.

### Registers Description

| Setting | Units | Data type | Register Addr | Register Count | Read/Write | Comment | Protocol Number |
| -- | -- | -- | -- | -- | -- | -- | -- |
| Fault Code |  | uint32 | 100 | 2 | R | 32-bit error code, each bit corresponds to an error code, see details in Error Code Table | 3,4,5,6 |
| Warning Code |  | uint32 | 104 | 2 | R | 32-bit warning code, each bit corresponds to a warning code, see details in Warning Code Table | 3,4,5,6 |
| Type of device |  | uint16 | 171 | 1 | R |  | 3,4,5,6 |
| Protocol Number |  |  | 184 | 1 | R | 0x03 | 3 |
| Protocol Number |  |  | 184 | 1 | R | 0x04 | 4 |
| Protocol Number |  |  | 184 | 1 | R | 0x05 | 5 |
| Protocol Number |  |  | 184 | 1 | R | 0x06 | 6 |
| Serial number |  | ASCII | 186 | 12 | R |  | 3,4,5,6 |
| Power flow State |  | unit16 | 198 | 1 | R | See Power Flow State Description | 3,4,5,6 |
| Working Mode |  |  | 201 | 1 | R | 0: Power On Mode; 1: Standby Mode; 2: Mains Mode; 3: Off-Grid Mode; 4: Bypass Mode; 5: Charging Mode; 6: Fault Mode | 3,4,5,6 |
| Total Grid Current | 0,1A | unit16 | 202 | 1 | R |  |  |
| Total Grid Frequency | 0,01Hz | unit16 | 203 | 1 | R |  | 3,4,5,6 |
| Total Grid Active Power | 1W | uni16 | 204 | 1 | R |  |  |
| Total Grid Apparent Power | 1VA | unit16 |  | 205 | 1 | R |  |  |
| Total Grid Charging Power | 1W | unit16 | 206 | 1 | R |  | 3,4,5,6 |
| Expansion of Power flow State |  | uint16 | 207 | 1 | R | See Power Flow State Expansion Description | 3,4,5,6 |
| not used |  |  | 208 | 18 |  |  |  |
| Total Inverter Current | 0,1A | uint16 | 226 | 1 | R |  |  |
| Total Inverter Frequency | 0,01Hz | uint16 | 227 | 1 | R |  | 3,4,5,6 |
| Total Inverter Active Power | 1W | uint16 | 228 | 1 | R |  |  |
| Total Inverter Apparent Power | 1VA | uint16 | 229 | 1 | R |  |  |
| Total Charging Current | 0,1A | uint16 | 230 | 1 | R |  | 3,4,5,6 |
| Inverter Module Temperature | 1°C | uint16 | 231 | 1 | R |  | 3,4,5,6 |
| not used |  |  | 232 | 20 |  |  |  |
| Total Output Current | 0,1A | uint16 | 252 | 1 | R |  | 4,6 |
| Total Output Frequency | 0,01Hz | uint16 | 253 | 1 | R |  | 3,4,5,6 |
| Total Output Active Power | 1W | uint16 | 254 | 1 | R |  | 4,6 |
| Total Output Apparent Power | 1W | uint16 | 255 | 1 | R |  | 4,6 |
| Total Load Percentage | 1% | uint16 | 256 | 1 | R |  | 4,6 |
| not used |  |  | 257 | 20 |  |  |  |
| Battery Voltage | 0,1V | uint16 | 277 | 1 | R |  | 3,4,5,6 |
| Battery Current | 0,1A | uint16 | 278 | 1 | R | A positive number indicatesa charge and a negative number indicates a discharge. | 3,4,5,6 |
| Battery Power | 1W | uint16 | 279 | 1 | R |  | 3,4,5,6 |
| Battery SOC | 1% |  | 280 | 1 | R |  | 3,4,5,6 |
| DC Module Temperature | 1°C | uint16 | 281 | 1 | R |  | 3,4,5,6 |
| not used |  |  | 282 | 20 |  |  |  |
| Total PV Power | 1W | uint16 | 302 | 1 | R |  | 3,4 |
| Total PV Charging Power | 1W | uint16 | 303 | 1 | R |  | 3,4 |
| Total PV Charging Current | 0,1A | uint16 | 304 | 1 | R |  | 3,4 |
| PV Module Temperature | 1° | uint16 | 305 | 1 | R |  | 3,4,5,6 |
| not used |  |  | 306 | 20 |  |  |  |
| External CT Total Power Balance | 1W | int32 | 326 | 2 | R | Positive: consumed from grid; Negative: feed to grid |  |
| not used |  |  | 328 | 10 |  |  |  |
| L1 Grid Voltage | 0,1V | uint16 | 338 | 1 | R |  | 3,4,5,6 |
| L1 Grid Current | 0,1A | uint16 | 339 | 1 | R |  |  |
| L1 Grid Active Power | 1W | uint16 | 340 | 1 | R |  | 3,4,5,6 |
| L1 Grid Apparent Power | 1VA | uint16 | 341 | 1 | R |  |  |
| L1 Inverter Voltage | 0,1V | uint16 | 342 | 1 | R |  | 3,4,5,6 |
| L1 Inverter Current | 0,1A | uint16 | 343 | 1 | R |  | 3,4,5,6 |
| L1 Inverter Active Power | 1W | int16 | 344 | 1 | R | Positive: inverter output; Negative: inverter input | 3,4,5,6 |
| L1 Inverter Apparent Power | 1VA | uint16 |  | 345 | 1 | R |  |  |
| L1 Output Voltage | 0,1V | uint16 | 346 | 1 | R |  | 3,4,5,6 |
| L1 Output Current | 0,1A | uint16 | 347 | 1 | R |  | 3,4,5,6 |
| L1 Output Active Power | 1W | uint16 | 348 | 1 | R |  | 3,4,5,6 |
| L1 Output Apparent Power | 1VA | uint16 | 349 | 1 | R |  | 3,4,5,6 |
| L1 Output Load Percentage | 1% | uint16 | 350 | 1 | R |  | 3,4,5,6 |
| PV1 Voltage | 0,1V | uint16 | 351 | 1 | R |  | 3,4,5,6 |
| PV1 Current | 0,1A | uint16 | 352 | 1 | R |  | 3,4,5,6 |
| PV1 Power | 1W | uint16 | 353 | 1 | R |  | 3,4,5,6 |
| L1 Output Power Balance | 1W | int32 | 354 | 2 | R | Positive: energy consumed from grid; Negative: energy feed to grid |  |
| not used |  |  | 356 | 20 |  |  |  |
| L2 Grid Voltage | 0,1V | uint16 | 376 | 1 | R |  |  |
| L2 Grid Current | 0,1A | uint16 | 377 | 1 | R |  |  |
| L2 Grid Active Power | 1W | uint16 | 378 | 1 | R |  |  |
| L2 Grid Apparent Power | 1VA | uint16 | 379 | 1 | R |  |  |
| L2 Inverter Voltage | 0,1V | uint16 | 380 | 1 | R |  |  |
| L2 Inverter Current | 0,1A | uint16 | 381 | 1 | R |  |  |
| L2 Inverter Active Power | 1W | int16 | 382 | 1 | R | Positive: inverter output; Negative: inverter input |  |
| L2 Inverter Apparent Power | 1VA | uint16 | 383 | 1 | R |  |  |
| L2 Output Voltage | 0,1V | uint16 | 384 | 1 | R |  | 4,6 |
| L2 Output Current | 0,1A | uint16 | 385 | 1 | R |  | 4,6 |
| L2 Output Active Power | 1W | uint16 | 386 | 1 | R |  | 4,6 |
| L2 Output Apparent Power | 1VA | uint16 | 387 | 1 | R |  | 4,6 |
| L2 Output Load Percentage | 1% | uint16 | 388 | 1 | R |  | 4,6 |
| PV2 Voltage | 0,1V | uint16 | 389 | 1 | R |  | 3,4 |
| PV2 Current | 0,1A | uint16 | 390 | 1 | R |  | 3,4 |
| PV2 Power | 1W | uint16 | 391 | 1 | R |  | 3,4 |
| L2 Output Power Balance | 1W | int32 | 392 | 2 | R | Positive: energy consumed from grid; Negative: energy feed to grid |  |
| not used |  |  | 394 | 20 |  |  |  |
| L3 Grid Voltage | 0,1V | uint16 | 414 | 1 | R |  |  |
| L3 Grid Current | 0,1A | uint16 | 415 | 1 | R |  |  |
| L3 Grid Active Power | 1W | uint16 | 416 | 1 | R |  |  |
| L3 Grid Apparent Power | 1VA | uint16 | 417 | 1 | R |  |  |
| L3 Inverter Voltage | 0,1V | uint16 | 418 | 1 | R |  |  |
| L3 Inverter Current | 0,1A | uint16 | 419 | 1 | R |  |  |
| L3 Inverter Active Power | 1W | int16 | 420 | 1 | R | Positive: inverter output; Negative: inverter input |  |
| L3 Inverter Apparent Power | 1VA | uint16 | 421 | 1 | R |  |  |
| L3 Output Voltage | 0,1V | uint16 | 422 | 1 | R |  |  |
| L3 Output Current | 0,1A | uint16 | 423 | 1 | R |  |  |
| L3 Output Active Power | 1W | uint16 | 424 | 1 | R |  |  |
| L3 Output Apparent Power | 1VA | uint16 | 425 | 1 | R |  |  |
| L3 Output Load Percentage | 1% | uint16 | 426 | 1 | R |  |  |
| PV3 Voltage | 0,1V | uint16 | 427 | 1 | R |  |  |
| PV3 Current | 0,1A | uint16 | 428 | 1 | R |  |  |
| PV3 Power | 1W | uint16 | 429 | 1 | R |  |  |
| L3 Output Power Balance | 1W | int32 | 430 | 2 | R | Positive: energy consumed from grid; Negative: energy feed to grid |  |
| not used |  |  | 432 | 168 |  |  |  |
| Output Mode |  | uint16 | 600 | 1 | R/W | 0: Single; 1: Parallel; 2: 3-Phase P1; 3: 3-Phase P2; 4: 3-Phase P3; 5: Split-phase P1; 6: Split-phase P2 | 3,4,5,6 |
| Output Priority (primary) |  | uint16 | 601 | 1 | R/W | 1: SUB; 2: SBU; 3: SUF | 3,4,5,6 |
| Output Priority (secondary) |  | uint16 | 602 | 1 | R/W | 0: Secondary output disabled; 1: SUB; 2: SBU; 3: SUF | 3,4,5,6 |
| Start time from secondary output priority |  | uint16 | 603 | 1 | R/W | Range: 0000-2359,thousands and hundreds are hours, tens and single digits are minutes | 3,4,5,6 |
| End time from secondary output priority |  | uint16 | 604 | 1 | R/W | Range:0000 2359,thousands and hundreds are hours, tens and single digits are minutes | 3,4,5,6 |
| Current Output Priority |  | uint16 | 605 | 1 | R | 1: SUB; 2: SBU; 3: SUF | 3,4,5,6 |
| Output Voltage Setting | 0,1V | uint16 | 606 |  | 1 | R/W | 2200: output 220V; 2300: output 230V; 2400: output 240V; | 3,4,5,6 |
| Output Frequency Setting | 0,01Hz | uint16 | 607 | 1 | R/W | 5000: 50Hz output; 6000: 60Hz output; | 3,4,5,6 |
| OP2 Overload Alarm Setting | 1% | uint16 | 608 | 1 | R/W | Range: 10%-100% | 4,6 |
| OP2 Output Allowed Setting |  | uint16 | 609 |  | 1 | R/W | 0: Disabled 1: Enabled | 4,6 |
| not used |  |  | 610 | 20 |  |  |  |
| Battery Type |  | uint16 | 630 | 1 | R/W | 0: AGM; 1: FLD; 2: USER; 4: Li2; 6: Li4; 8: LiB | 3,4,5,6 |
| Overvoltage protection Setting | 0,1V | uint16 | 631 | 1 | R/W |  | 3,4,5,6 |
| Charger Source Priority (primary) |  | uint16 | 632 | 1 | R/W | 1: Solar First (SOF); 2: Solar & Utility (SNU); 3: Only Solar (OSO); 4: Solar Residual (SOR) | 3,4,5,6 |
| Charger Source Priority (secondary) |  | uint16 | 633 | 1 | R/W | 0: Disabled; 1: (SOF); 2: (SNU); 3: (OSO); 4: (SOR) | 3,4,5,6 |
| Start time for secondary charger source priority |  | uint16 | 634 | 1 | R/W | Range: 0000 2359, thousands and hundreds are hours, tens and single digits are minutes | 3,4,5,6 |
| End time for secondary charger source priority |  | uint16 | 635 | 1 | R/W | Range: 0000 2359, thousands and hundreds are hours, tens and single digits are minutes | 3,4,5,6 |
| Current Charger Source Priority |  | uint16 | 636 | 1 | R | 1: (SOF); 2: (SNU); 3: (OSO); 4: (SOR) | 3,4,5,6 |
| Bulk Charging Voltage Setting | 0,1V | uint16 | 637 | 1 | R/W |  | 3,4,5,6 |
| Floating Charging Voltage Setting | 0,1V | uint16 | 638 | 1 | R/W |  | 3,4,5,6 |
| Bulk Charging time (CV stage) Setting | 1m | uint16 | 639 | 1 | R/W | Interval: 1-900 minutes, 0 - auto | 3,4,5,6 |
| Max Charging Current Seting | 0,1A | uint16 | 640 | 1 | R/W |  | 3,4,5,6 |
| Max AC Charging Current Setting | 0,1A | uint16 | 641 | 1 | R/W |  | 3,4,5,6 |
| Max Discharge Current Protection Setting | 1A | uint16 | 642 | 1 | R/W | Range: 0-500A | 3,4,5,6 |
| Back to Battery Voltage Setting | 0,1V | uint16 | 643 | 1 | R/W | 0 - 100% SOC | 3,4,5,6 |
| Back to Grid Voltage Setting | 0,1V | uint16 | 644 | 1 | R/W |  | 3,4,5,6 |
| OP1 Low Voltage Protection Setting (off-grid mode) | 0,1V | uint16 | 645 | 1 | R/W |  | 4,6 |
| Low Voltage Protection Setting (off-grid mode) | 0,1V | uint16 | 646 | 1 | R/W |  | 3,4,5,6 |
| Back to Battery SOC Setting | 1% | uint16 | 647 | 1 | R/W |  | 3,4,5,6 |
| Back to Grid SOC Setting | 1% | uint16 | 648 | 1 | R/W |  | 3,4,5,6 |
| OP1 Low SOC Protection Setting (off-grid mode) | 1% | uint16 | 649 | 1 | R/W |  | 4,6 |
| Low SOC Protection Setting (off-grid mode) | 1% | uint16 | 650 | 1 | R/W |  | 3,4,5,6 |
| Battery Equalization mode |  | uint16 | 651 | 1 | R/W | 0: Disabled; 1: Enabled | 3,4,5,6 |
| Battery Equalization Voltage Setting | 0,1V | uint16 | 652 | 1 | R/W |  | 3,4,5,6 |
| Battery Equalization Time Setting | 1m | 653 | 1 | R/W | Range: 0-900 minutes, step 5 | 3,4,5,6 |
| Battery Equalization Timeout Setting | 1m | 654 | 1 | R/W | Range: 0-900 minutes, step 5 | 3,4,5,6 |
| Battery Equalization Interval Setting | 1d | uint16 | 655 | 1 | R/W | Interval: 1-90 days | 3,4,5,6 |
| Equalization activated immediately |  | uint16 | 656 | 1 | R/W | 0: Disabled; 1: Enabled | 3,4,5,6 |
| not used |  |  | 657 | 20 |  |  |  |
| AC Input Voltage Range |  | uint16 | 677 | 1 | R/W | 0: APL; 1: UPS; 2:GNT | 3,4,5,6 |
| Buzzer Mode |  | uint16 | 678 | 1 | R/W | 0: Silent; 1: Faults, Warnings, Source Change; 2: Faults, Warnings; 3: Faults | 3,4,5,6 |
| LCD Backlight |  | uint16 | 679 | 1 | R/W | 0: Timed-off; 1: Always on | 3,4,5,6 |
| LCD Automatic return to homepage |  | uint16 | 680 | 1 | R/W | 0: Disabled; 1: Enabled | 3,4,5,6 |
| Energy-saving Mode |  | uint16 | 681 | 1 | R/W | 0: Disabled; 1: Enabled | 3,4,5,6 |
| Overload Automatic Restart |  | uint16 | 682 | 1 | R/W | 0: Disabled; 1: Enabled | 3,4,5,6 |
| Overtemperature Automatic Restart |  | uint16 | 683 | 1 | R/W | 0: Disabled; 1: Enabled | 3,4,5,6 |
| Overload Transfer to Bypass |  | uint16 | 684 | 1 | R/W | 0: Disabled; 1: Enabled | 3,4,5,6 |
| PV parallel detection Mode |  | uint16 | 685 | 1 | R/W | 0: Disabled; 1: Enabled |  |
| External CT enabled |  | uint16 | 686 | 1 | R/W | 0: Disabled; 1: Enabled |  |
| Mask Warnings |  | uint32 | 687 | 2 | R/W | Alerts matching 1 are displayed normally and alerts matching 0 are blocked. | 3,4,5,6 |
| Dry contact |  | uint16 | 689 | 1 | R/W | 0: Normal mode; 1: Box Mode of grounding; | 3,4,5,6 |
| Automatic Mains Output enabled |  | uint16 | 690 | 1 | R/W | 0: No mains output until power button pressed; 1: Mains outputs even if power button not pressed | 3,4,5,6 |
| Max PV power Connected | 1W | uint16 | 691 | 1 | R/W | Range: 200W. Rated power |  |
| Islanding Detection Enabled |  | uint16 | 692 | 1 | R/W | 0: Disabled; 1: Enabled |  |
| Boot Method |  | uint16 | 693 | 1 | R/W | 0: Can be turned on locally or remotely; 1: Can only be turned on locally; 2: Can be turned on remotely only | 3,4,5,6 |
| Remote Switch |  | uint16 | 694 | 1 | R/W | 0: Remote shutdown; 1: Remote boot | 3,4,5,6 |
| Exit Fault State |  | uint16 | 695 | 1 | W | 1: Exit fault state lock (will only take effect if the machine enters fault mode) | 3,4,5,6 |
| Inverter Date |  | uint16 | 696 | 3 | R/W | Three numbers of 16 digits representing the year, month and day (for example: 07H E6H 00H 07H 00H 0EH means July 14, 2022) | 3,4,5,6 |
| Inverter Time |  | uint16 | 699 | 3 | R/W | Three numbers of 16 digits representing hours, minutes and seconds (for example: 00H 0DH 00H 00H 00H 10H means 13:15:16) | 3,4,5,6 |
| Daily PV Energy generation | 0,01kWh | uint16 | 702 | 1 | R |  | 3,4,5,6 |
| Total PV Energy generation | 0,01kWh | uint32 | 703 | 2 | R |  | 3,4,5,6 |
| Reset generated Power |  | uint16 | 705 | 1 | W | OxAA: Clear all the data on the energy production | 3,4,5,6 |
| Reset User Settings |  | uint16 | 706 | 1 | W | OxAA: Reset user parameters to default value (takes effect in non-offgrid mode) | 3,4,5,6 |
| Ground Relay Enabled |  | unit16 | 707 | 1 | R/W | 0: Disabled; 1: Enabled | 4,6 |
| Lithium Battery Activation Time | 1m |  | 709 | 1 | R/W | Interval: 6-300 minutes | 3,4 |


### Error Code Table (register 100)

| Bit offset | Description |
| -- | -- |
| 0 | Inverter overheating |
| 1 | PV Overtemperature |
| 2 | DCDC Overtemperature |
| 3 | Battery overvoltage |
| 4 | not used |
| 5 | Output short circued |
| 6 | Inverter overvoltage |
| 7 | Output overloaded |
| 8 | Bus overvoltage |
| 9 | Bus soft start failed |
| 10 | PV overcurrent |
| 11 | PV overvoltage |
| 12 | DCDC overcurrent |
| 13 | Inverter overcurrent |
| 14 | Bus voltage too low |
| 15 | not used |
| 16 | Over DC voltage in AC output |
| 17 | PV1 current is too distorted |
| 18 | Output current is too distorted |
| 19 | Inverter current is too distorted |
| 20 | Battery current is too distorted |
| 21 | PV1 current is too distorted |
| 22 | Inverter low voltage |
| 23 | Inverter negative power |
| 24 | Host in parallel system is lost |
| 25 | Synchronization signal abnormal in the parallel system |
| 26 | Battery type is incompatible |
| 27 | Parallel versions are incompatible |
| 28 | External TC settings are duplicated |
| 29 | Output current L2 is too distorted |


### Warning Code Table (register 104)

| Bit offset | Description |
| -- | -- |
| 0 | Mains zero crossing is lost |
| 1 | Mains waveform abnormal |
| 2 | Mains overvoltage |
| 3 | Mains low voltage |
| 4 | Mains overfrequency |
| 5 | Mains low frequency |
| 6 | PV1 low voltage |
| 7 | Overtemperature |
| 8 | Battery low voltage |
| 9 | Battery not connected |
| 10 | Overload |
| 11 | Battery Eq charging |
| 12 | Battery is discharged to low voltage and has not been recharged to the recovery point |
| 13 | Output power derating |
| 14 | Fan blocked |
| 15 | PV energy is too low to be used |
| 16 | Parallel communication interrupted |
| 17 | Output mode of Single and Parallel systems is inconsistent |
| 18 | Battery voltage difference of parallel system is too large |
| 19 | Communication with lithium battery is abnormal |
| 20 | Battery discharge current too high |
| 21 | Islanding detected |
| 22 | PV2 low voltage |
| 23 | OP1 is overloaded |
| 24 | OP2 load exceeds the set value |
| 25 | OP2 is overloaded |
| 26-31 | not used |

### Power Flow State (register 198)

| Bit offset | Description |
| -- | -- |
| 0-1 | 0: PV is not connected; 1: PV is connected |
| 2-3 | 0: Mains is not connected; 1: Mains is connected |
| 4-5 | 0: Battery is not charging/discharging; 1: Battery is charging; 2: Battery is discharging |
| 6-7 | 0: Load is not connected; 1: Load is connected |
| 8 | 0: Mains is not charging; 1: Mains is charging |
| 9 | 0: PV is not charging; 1: PV is charging |
| 10 | 0: Battery icon is on; 1: Battery icon is off |
| 11 | 0: PV icon is on; 1: PV icon is off |
| 12 | 0: Mains icon is on; 1: Mains icon is off |
| 13 | 0: Load icon is on 1: Load icon is off |


### Power Flow State Expansion (register 207)

| Bit offset | Description |
| -- | -- |
| 0 | 0: PV1 is not connected; 1: PV1 is connected |
| 1 | 0: PV2 is not connected; 1: PV2 is connected |
| 2-3 | 0: AC input 1 is not connected; 1: AC input 1 is connected |
| 4-5 | 0: AC input 2 is not connected; 1: AC input 2 is connected |
| 6-7 | 0: AC input 3 is not connected; 1: AC input 3 is connected |
| 8 | 0: OP1 loaded; 1: OP1 not loaded |
| 9 | 0: OP2 loaded; 1: OP2 not loaded |
| 10-15 | not used |
