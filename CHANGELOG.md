# Changelog for CAN IO Board Hardware

Put all development notes/changelog stuff here. Try to format something like below to minimize git conflicts:

```
## <date, or commit hash>

<notes>

### Added

### Changed

### Removed

## <older date/hash>

...
```

This format is adopted from [keepachangelog.com](https://keepachangelog.com).

## 62c16c2

### Added

- Added an initial BOM csv, with all the manufacturer names and part numbers listed out. A few are missing, either because they are out of stock or undecided; they will have to be resolved before the PCB is complete and the order is placed.

## 1cf1787

### Changed

- Made the 20k to 2 10ks, so we don't have to order just one 20k.
- Need to change USB connector from 14 to 16 pin
- Need to change from G0B1KET6 to KET6N
- Need to find replacement for ISOW1044
- Need to make footprint for the USB and CAN filter ICs
- Need to decide upon a CAN bus connector
    - Once all of that is done, setup PCB editor with 2oz outer layers, stackup of signal + gnd -> gnd -> pwr -> signal + pwr, with 12V going to relays on the bottom 2oz layer. All 12V vias must be laid out in a grid.
    - Then setup netclasses, trace width and via sizes, differential pair sizes, etc.
    - Finally the rest of the layout and routing.

## a1869ab

### Added

- Finished the connectors sheet.
    - GPIO connector is just a normal 5 pin header for now, can change later.
    - SWD is connected via both TC2030, and also a 3 pin header, with NRST, SWDIO, and SWCLK
    - CAN bus lines to be connected via DT connector
    - Added USB-C, with USB peripheral in CubeMX, for printf and other usb stuff.
    - High and low current outputs will be delivered via WAGO connectors
    - Finally, added a Wakeup and NRST push button.

## 10131ee

### Added

- This design was mainly done by Kostadin. I believe he followed the example schematic from the datasheet. The current design was simply ported over from his schematic, and a few issues resolved.
- The reason behind the 12V RWM is simply that we're *expecting* that much drift on the IC's input anyway. Whether it's the actual CAN wires throughout the car, or simply by the max input of the CAN transceiver IC, we expect that the signal-to-ground voltage may exceed 3.75V. Since an input of beyond 12V (not differential, idk what happens then) isn't desirable, that becomes the diode's RWM.

## fb34b0c

### Added

- Finished the relay driver schematics. Used two TPL7407LA load driver ICs.
- The requirement was 2 *high current* channels (300mA, powering a relay), and 3 *low current* channels, providing 1.2A directly.
- The current configuration has one high current channel, one doubled up low current channel, and then another half-doubled up low current channel per IC. This evenly splits the current draw, if all channels were to be on simultaneously.
- The high current channels then drive the coils of two relay sockets.

## e61d195

### Added

- Finished the power schematic. However, there's some confusion about the post-smps filter... The IC's datasheet suggests using a 4.7uH inductor and two 22uF capacitors in parallel (making 44uF). This circuit resonates at 11kHz, and is basically a stop-band after that. Our buck IC has a switching frequency of 100kHz, which is wayyy beyond 11kHz; and so it (and all harmonics) should be attenuated. However, I've had bad experiences with this in the past (inadequate filtering, following similar procedure), and so I want another set of eyes on this.

## 5c744d9

### Added

- In previous commits:
    - an IOC, [here](https://github.com/NJIT-Solar-Car/can-io-board/blob/5c744d9fd663344a5dfaa119cfbe4434d60c49f4/Software/Software.ioc).
    - Created hierarchical sheets backbone.
    - Initial documentation, app notes, etc.
- Created a bus for the STM, and individual buses for each peripheral. The idea is to make each peripheral modular and within its own hierarchical sheet if possible. Object-oriented schematics if you want to put it that way.
- Added LEDs for the heartbeat and fault outputs
- Added 5 push buttons. The schematic for these came directly from Kostadin's implementation of an RC debounce circuit with the button. I changed the values a little so that the time constant is 10ms, as a [TI App Note](https://github.com/NJIT-Solar-Car/can-io-board/blob/5c744d9fd663344a5dfaa119cfbe4434d60c49f4/docs/ApplicationNotes/Debounce_a_Switch.pdf) suggested.

## 1c9e212

first commit

### Added

- Added readme, changelog, and documentation for all the major ics being used.
