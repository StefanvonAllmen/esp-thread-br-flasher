# ESP Thread Border Router – browser flasher

> **Made by Claude.** This repository, its web page, the firmware build and the
> firmware changes were created by Claude, an AI assistant from Anthropic, working
> on the owner's PC. Unofficial: not made or endorsed by Espressif or Anthropic.
> Use at your own risk.

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
- **ethernet**: like normal, but connects by LAN cable via Espressif's Ethernet sub-board (W5500)

## Talking to the router

### Web page (over the network)

Every board serves a web page on your LAN. Open it in any browser:

- `http://esp-ot-br-xxxx.local`: replace `xxxx` with the board's last 4 MAC digits.
  The name is printed at boot as `Board name: ...`.
- If `.local` names don't resolve on your PC, use the board's IP address from your router's
  DHCP list instead. The board shows up there under the same name.

Pages:

| Page | What it does |
|---|---|
| Dashboard | Thread role, network name, channel, PAN ID |
| Network | Scan, join or form a Thread network |
| Topology | Map of routers and child devices; click a node to ping it |
| Addresses | IPv6 addresses of the border router |
| Commissioner | Let new Thread devices join |
| Tools | Ping |
| About | Version and links |

The web page has **no Wi-Fi settings and no login**. Anyone on your LAN can open it, so
don't use "Form" or "Join" on a board that is already part of your network.

### Serial console (over USB)

Connect the board's ESP32-S3 USB port to the PC. It appears as a serial port: COMx on
Windows, `/dev/ttyACM0` on Linux, `/dev/cu.usbmodem*` on macOS. Use 115200 baud.

- **Browser:** on the flasher page, click a Connect button, pick the port, then choose
  **Logs & Console** (ESP Web Tools). This works in Chrome or Edge.
- **ESP-IDF:** `idf.py -p COMx monitor` (exit with `Ctrl+]`).
- **Any terminal program:** for example PuTTY (Serial, 115200) or `screen /dev/ttyACM0 115200`.

Only one program can use the port at a time. Close the console before flashing.

Every command starts with `ot `. Useful ones:

| Command | What it does |
|---|---|
| `ot state` | `router` or `child` = joined; `leader` on its own = it formed a separate network |
| `ot networkname` | name of the Thread network |
| `ot netdata show` | routes and prefixes from all border routers |
| `ot child table` | connected child devices |
| `ot wifi state` | whether Wi-Fi is connected |
| `ot wifi connect -s <wifi name> -p <password>` | set or change Wi-Fi (saved on the board) |
| `ot dataset set active <hex>` | set the Thread network: the dataset from Home Assistant |
| `ot ifconfig up` then `ot thread start` | start Thread by hand (setup firmware) |
| `ot rcp version` | firmware version of the ESP32-H2 radio |

The Thread dataset contains the network key, and `ot wifi config` prints the Wi-Fi password in
plain text. Treat both like passwords and don't paste them anywhere public.

## Combined images

`esp-ot-br-*-full-0x0.bin` are combined images for flashing at address 0x0 with other tools
(they overwrite saved settings).
