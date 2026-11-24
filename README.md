# ESP Thread Border Router – browser flasher

Firmware for the Espressif ESP Thread Border Router board (ESP32-S3 + ESP32-H2),
built from [esp-thread-br](https://github.com/espressif/esp-thread-br)
(`basic_thread_border_router`, ESP-IDF v5.5.6) with local changes:

- auto-start after power-up (saved Wi-Fi and Thread network)
- Wi-Fi retries forever, Wi-Fi power save off
- unique name per board: `esp-ot-br-xxxx` (last MAC digits)
- up to 64 child devices

Flash it from the browser (Chrome or Edge): open the GitHub Pages site of this repository.

- **normal**: for boards that are already set up (keeps saved settings unless "Erase device" is ticked)
- **setup**: for new boards; enter Wi-Fi and the Thread dataset in the serial console, then install normal

`esp-ot-br-*-full-0x0.bin` are combined images for flashing at address 0x0 with other tools
(they overwrite saved settings).
