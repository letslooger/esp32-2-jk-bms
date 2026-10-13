# One ESP32 for two JK BMS in Home Assistant

Most guides use one ESP32 per battery. Here **a single ESP32** connects over Bluetooth to **two JK BMS** at the same time and sends everything to Home Assistant over Wi-Fi: all 2 × 16 cell voltages, per-cell resistance, temperatures, current, power, state of charge, cycles, alarms…

Step-by-step video (French, English subtitles) on the **Alf Fait Maison** channel: *link coming soon*

## Hardware
- 1 × **ESP32-WROOM-32U**, the version **with an external antenna** (IPEX / u.FL connector), sold with its antenna.
  ⚠️ Make sure you get the **32U**, not the 32D or 32E with a built-in antenna: range matters when the batteries are far from the router, and the 3D case is made for it.
- 2 × JK BMS (tested with JK_PB2A16S20P, hardware V19A, protocol `JK02_32S`)
- 1 USB charger to power the ESP32
- Optional: the 3D-printed case (`boitier_3d` folder)

## Quick setup
1. Install **ESPHome Device Builder** in Home Assistant (Settings > Add-ons > Add-on store).
2. Create a new ESP32 device, click **Edit** and paste `esp32_2_jk_bms.yaml`.
3. Replace `XX:XX:XX:XX:XX:01` and `XX:XX:XX:XX:XX:02` with the Bluetooth addresses of your two BMS (the free nRF Connect app shows them: devices whose name starts with JK).
   ⚠️ Use an **Android** phone: on iPhone, Apple hides Bluetooth addresses and nRF Connect cannot show them.
4. Fill in your secrets (see `secrets.exemple.yaml`).
5. **Install** > Plug into this computer the first time; later updates are wireless.

## How two batteries work
- Two `ble_client` (client0, client1) and two `jk_bms_ble` (bms0, bms1).
- Every sensor is declared twice; battery 2 sensors are prefixed **B2**.
- Watchdog: if a BMS is offline for 3 minutes, its Bluetooth connection is restarted automatically.
- Per-BMS "Bluetooth signal" sensors to place the antenna (above −85 dBm is fine).

## 3D-printed case
`boitier_3d/WiRex-Bottom-plus5mm.stl` and `WiRex-Top-plus5mm.stl`: a **5 mm longer** remix of
"ESP32-WROOM-32U casing" by **Loiseau créatif**: https://www.printables.com/model/762529-esp32-wroom-32u-casing
License **CC BY-NC-SA 4.0** (attribution, non-commercial, share-alike), same as the original.

## Credits
- ESPHome component: [syssi/esphome-jk-bms](https://github.com/syssi/esphome-jk-bms)
- Inspiration: Profes'Solaire video 126 (French): https://www.youtube.com/watch?v=ZjxKqJwwoc8
- Original case: Loiseau créatif (Printables)

## Licence / License
- `esp32_2_jk_bms.yaml` : Apache 2.0 (voir / see `LICENSE`), version modifiée de / modified version of syssi/esphome-jk-bms `esp32-ble-example.yaml`.
- `boitier_3d/` : CC BY-NC-SA 4.0 (voir / see `boitier_3d/LICENCE.txt`).
