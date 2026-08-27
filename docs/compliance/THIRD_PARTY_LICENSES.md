# Third-Party Components and Licenses

The primary firmware and custom web application of the SVA-Fencing-Tester are proprietary and closed-source. 

However, this project utilizes a variety of third-party open-source components, libraries, and frameworks within its compilation and build environments. The inventory below reflects the active components and their respective licenses.

## Core Firmware / Toolchain

| Component | Source / Package | License |
| --- | --- | --- |
| PlatformIO `espressif32` platform via pioarduino | `platform = https://github.com...` | Apache-2.0 |
| Arduino-ESP32 framework | `framework-arduinoespressif32` | LGPL-2.1-or-later |
| Arduino-ESP32 framework libraries | `framework-arduinoespressif32-libs` | LGPL-2.1-or-later |
| mklittlefs tool | `tool-mklittlefs` | MIT |

## PlatformIO Libraries

| Component | Version | License |
| --- | --- | --- |
| PsychicHttp | 2.1.1 | MIT |
| TaskScheduler | 3.8.5 | BSD-2-Clause |
| ArduinoJson | 7.4.3 | MIT |
| ADS1115_WE | 1.5.5 | MIT |
| ESP32Time | 2.0.6 | MIT |
| CRC | 1.0.4 | MIT |
| DS3232 | 0.6.1 | MIT |
| I2C_EEPROM | 1.9.4 | MIT |
| htcw_rmt_led_strip | 0.2.2 | MIT |
| Sqlite Micro Logger | 1.2.0 | Apache-2.0 |
| UrlEncode | transitive | MIT |

## Local / Vendored Libraries

| Component | Location | License |
| --- | --- | --- |
| XPT2046_Touchscreen_TT | `lib/XPT2046_Touchscreen_TT` | MIT; the remaining source files `XPT2046_Touchscreen_TT.h/.cpp` carry the MIT permission notice from the Paul Stoffregen-based upstream |

## Webapp Runtime Dependencies

| Component | Version | License |
| --- | --- | --- |
| Vue | 3.5.30 | MIT |
| Vue Router | 5.0.4 | MIT |
| Vue I18n | 11.3.0 | MIT |
| Bootstrap | 5.3.7 | MIT |
| bootstrap-icons-vue | 1.11.3 | MIT |
| @popperjs/core | 2.11.8 | MIT |
| mitt | 3.0.1 | MIT |
| sortablejs | 1.15.7 | MIT |
| spark-md5 | 3.0.2 | MIT or WTFPL |
| pulltorefreshjs | 0.1.22 | MIT |
| uPlot | 1.6.32 | MIT |
| uplot-vue | 1.2.4 | MIT |
| sass-embedded | 1.98.0 | MIT |
| @intlify/message-resolver | 9.1.10 | MIT |

## Webapp Build / Development Dependencies

| Component | Version | License |
| --- | --- | --- |
| @eslint/js | 10.0.1 | MIT |
| @intlify/unplugin-vue-i18n | 11.0.7 | MIT |
| @tsconfig/node22 | 22.0.2 | MIT |
| @types/bootstrap | 5.2.10 | MIT |
| @types/node | 25.5.0 | MIT |
| @types/pulltorefreshjs | 0.1.7 | MIT |
| @types/sortablejs | 1.15.8 | MIT |
| @types/spark-md5 | 3.0.5 | MIT |
| @vitejs/plugin-vue | 6.0.5 | MIT |
| @vue/eslint-config-typescript | 14.7.0 | MIT |
| @vue/tsconfig | 0.9.1 | MIT |
| eslint | 10.1.0 | MIT |
| eslint-plugin-vue | 10.8.0 | MIT |
| globals | 17.4.0 | MIT |
| npm-run-all | 4.1.5 | MIT |
| prettier | 3.8.1 | MIT |
| sass | 1.98.0 | MIT |
| sass-loader | 16.0.7 | MIT |
| TypeScript | 5.9 | Apache-2.0 |
| Terser | 5.46.1 | BSD-2-Clause |
| Knip | 6.0.4 | ISC |
| Vite | 8.0.2 | MIT |
| vite-plugin-compression | 0.5.1 | MIT |
| vite-plugin-css-injected-by-js | 4.0.1 | MIT |
| vue-eslint-parser | 10.x | MIT |
| vue-tsc | 3.2.6 | MIT |

## Notes

- This inventory is based on the package metadata and license files present in the current local project environment.
- The web dependency sections reflect the direct production and development dependencies declared in `webapp/package.json`.
- Transitive dependencies may introduce additional notices or obligations.
- The proprietary license of the primary SVA-Fencing-Tester firmware does not affect or alter the original open-source licenses governing these third-party tools and components.
