ustd
====

[![Mac/Linux Build Status](https://travis-ci.org/muwerk/ustd.svg?branch=master)](https://travis-ci.org/muwerk/ustd)
[![Dev Docs](https://img.shields.io/badge/docs-dev-blue.svg)](https://muwerk.github.io/ustd/docs/index.html)

ustd provides minimal and highly portable implementations  of the following classes:

- [`array`](https://muwerk.github.io/ustd/docs/classustd_1_1array.html), a lightweight c++11
  array implementation.
- [`queue`](https://muwerk.github.io/ustd/docs/classustd_1_1queue.html), a lightweight c++11
  queue implementation.
- [`map`](https://muwerk.github.io/ustd/docs/classustd_1_1map.html), a lightweight c++11
  map implementation.

The libraries are header-only and should work with any c++11 compiler and support platforms
starting with 8k attiny, avr, arduinos, up to esp8266, esp32 and mac and linux.

- [`functional.h`](https://muwerk.github.io/ustd/docs/functional_8h.html) provides a drop-in
  replacement for `std::function<>` for AVRs: `ustd::function<>` for low-resource AVRs (see
  project [functional-avr](https://github.com/winterscar/functional-avr))

Documentation: [ustd:: documentation](https://muwerk.github.io/ustd/docs/index.html)

Platform defines
----------------

Make sure to use the appropriate platform define before including from `ustd`.

| Platform | #define (by user) | Comment                                    |
| -------- | ----------------- | ------------------------------------------ |
| Mac      | `__APPLE__`       | Should be defined already                  |
| Linux    | `__linux__`       | Should be defined already                  |
| Arduino  | `__UNO__`         | Should work with low resource arduinos     |
| Arduino  | `__ATMEGA__`      | Should work with most arduinos             |
| ATtiny   | `__ATTINY__`      | For very low resource ATMELs               |
| STM32    | `__BLUEPILL__`    | STM32F103C8T6 ARM                          |
| ESP8266  | `__ESP__`         | For ESP8266 and ESP32                      |
| ESP32    | `__ESP32__`       | ESP32                                      |
| ESP32DEV | `__ESP32DEV__`    | ESP32 git head                             |
| Maix Bit | `__MAIXBIT__`     | Sipeed Maix Bit RISC-V                     |

**Note::** If the desired MCU is not in that list, select one with similar characteristics, these
platform defines are used to generate feature-lists that are used by muwerk's modules.

### Additional selectable options

| Option | Commend                                                            |
| ------ | ------------------------------------------------------------------ |
| `USTD_OPTION_FS_FORCE_SPIFFS`| to continue to use old SPIFFS instead of LittleFS. New default for ESP8266 is LittleFS. |
| `USTD_OPTION_FS_FORCE_NO_FS` | Disable all filesystem related functionality |

### Defines generated by `platform.h` depending on the platform define above

**Note:** All defines below are _automatically_ generated, they are derived from the platform define above:

#### Family defines

| Platform define | Automatically defined family                        | Comment
| --------------- | --------------------------------------------------- | ----------------------------- 
| `__UNO__`       | `__ARDUINO__`                                       | 8-bit Atmel Arduinos
| `__MEGA__`      | `__ARDUINO__`                                       |    "
| `__BLUEPILL__`  | `__ARM__`                                           | ARM cortex based MCUs
| `__ESP__`       |  t.b.d.                                             | t.b.d.
| `__ESP32__`     |    "                                                |   "
| `__ESPDEV__`    |    "                                                |   "
| `__MAIXBIT__`   | `__RISC_V__`                                        | RISC-V based MCUs
#### Features

| Define                     | Comment                                             |
| -------------------------- | --------------------------------------------------- |
| `USTD_FEATURE_MEMORY`      | This is set to a class of available memory, see below for possible values. |
| `USTD_FEATURE_FILESYSTEM`  | The system has a filesystem, muwerk APIs defined in `filesystem.h` and `jsonfile.h` are available. |
| `USTD_FEATURE_FS_SPIFFS`   | Filesystem is SPIFFS format (legacy ESP8266 and all ESP32) |
| `USTD_FEATURE_FS_LITTLEFS` | Filesystem is LittleFS (standard for ESP8266)
| `USTD_FEATURE_FS_SD`       | Future: SD Filesystem                               |
| `USTD_FEATURE_EEPROM`      | Platform has EEPROM storage                         |
| `USTD_FEATURE_SYSTEMCLOCK` | System has a clock                                  |
| `USTD_FEATURE_CLK_READ`    | Time can be read                                    |
| `USTD_FEATURE_CLK_SET`     | Time can be set                                     |
| `USTD_FEATURE_NET`         | Network access available                            |
| `USTD_FEATURE_FREE_MEMORY` | freeMemory() is available                           |

#### Possible values for `USTD_FEATURE_MEMORY`

(Automatically derived by `platform.h` from platform define `__xxx__`)

| Value                      | Example platform                         |
| -------------------------- | ---------------------------------------- |
| `USTD_FEATURE_MEM_512B`    | ATtiny85                                 |
| `USTD_FEATURE_MEM_2K`      | Arduino UNO, ATtiny1614, AT328P          |
| `USTD_FEATURE_MEM_8K`      | Arduino MEGA                             |
| `USTD_FEATURE_MEM_32k`     | ESP8266, Bluepill                        |
| `USTD_FEATURE_MEM_128`     | Blackpill                                |
| `USTD_FEATURE_MEM_512k`    | ESP32                                    |
| `USTD_FEATURE_MEM_1M`      | Unixoids & RISC-V                        |

To make code dependent on a memory-class, use something like:

```c++
#if USTD_FEATURE_MEMORY >= USTD_FEATURE_MEM_2K
// Feature that requires at least 2k RAM
#endif
```

### Example

```c++
// Sample usage:
#define __ATTINY__ 1
#include "platform.h"
#include "queue.h"
```

Installation
------------

`ustd` is available via Arduino library manager or platformio:

- [Arduino ustd](https://www.arduinolibraries.info/libraries/muwerk-ustd-library)
- [Platformio ustd](https://platformio.org/lib/show/5710/ustd/examples?file=ustd-test.cpp),
  library ID 5710.

Example
-------

See [ustdmin](https://github.com/muwerk/Examples/tree/master/ustdmin) for a complete build
example with `ustd` and `platformio`.

Related projects
----------------

- ustd is used by [muwerk](https://github.com/muwerk/muwerk) to implement a portable cooperative
scheduler with MQTT-like communication queues.

History
-------

- 0.4.1 (2021-01-22) Bugfix for USTD_FEATURE_MEMORY handling. ATtiny no longer supports ustd::function<>.
- 0.4.0 (2021-01-19) Feature defines in `platform.h` for better hardware
        specific adaptations.
- 0.3.6 (2021-01-12) Support for UNO and MEGA in functional.h via `__ARDUINO__` define.
- 0.3.5 (2021-01-12) New function `freeMemory()`, new platform define `__UNO__`. (Both `__UNO__`
        and `__ATMEGA__` implicitly define `__ARDUINO__`)
- 0.3.4 (2021-01-11) Small documentation fixes.
- 0.3.3 (2021-01-08) `ustd::array::resize()` did not correctly update the array size, which would
        lead to memory corrupts (tuxpoldo). Improvements for debug macros.
- 0.3.2 (2021-01-07) More consistent debug interface using `DBG()` macros (see
        [`platform.h`](https://github.com/muwerk/ustd/blob/2a64a88b59e8bc880d7a1ad63e96d8a66bb1aaf8/platform.h#L151)),
        fixes to `USTD_ASSERT` macro that was inconsistently named ASSERT*S*.
- 0.3.1 (2020-12-25) More SPIFFS vs LittleFS preparations
- 0.3.0 (2020-10-26) Cleanup platform.h: ESP32 continues to use SPIFFS by default, ESP8266
        LittleFS (since SPIFFS is deprecated for ESP8266, and LittleFS is not (yet) available
        for ESP32). This is a breaking change for ESP8266 installations that require the
        filesystem, since an upgrade from SPIFFS to LittleFS is required, see
        [munet Readme](https://github.com/muwerk/munet/blob/master/README.md) for additional
        information.
- 0.2.2 (2020-09-27) Support for LittleFS as successor of ESP8266/ESP32 filesystem
- 0.2.1 (2019-09-19) Functional support for AVRs added (from project
        [functional-avr](https://github.com/winterscar/functional-avr) by
        [winterscar](https://github.com/winterscar)).

References
----------

- `functional.h` is taken from project [functional-avr](https://github.com/winterscar/functional-avr)
  by [winterscar](https://github.com/winterscar)
- `ustd` and `muwerk` are derivatives and lightweight versions of
  [Meisterwerk](https://github.com/yeasoft/Meisterwerk).
