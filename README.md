# Unique ESP32 Web Flasher

A browser-based flasher for [pfsense-status-esp32](https://github.com/UniqueDroid/pfsense-status-esp32) and [fritzbox-status-esp32](https://github.com/UniqueDroid/fritzbox-status-esp32) - flash a LilyGO T-Display S3 (blank or already running the firmware) straight from Chrome/Edge over USB, no PlatformIO or IDE install required.

Built on [ESP Web Tools](https://github.com/esphome/esp-web-tools) (Apache-2.0), the same browser-flashing library used by ESPHome, Home Assistant and other ESP32 projects (including Bruce's own web launcher).

Link Unique ESP32 Web Flasher
https://uniquedroid.github.io/unique-esp-web-flasher

## How it works

- `index.html` is the landing page with one install button per project.
- `manifests/*.json` describe each build: bootloader + partition table + app firmware, at the flash offsets PlatformIO uses for the LilyGO T-Display S3 (ESP32-S3, `dio`/16MB).
- Bootloader and partition-table images come from each project's GitHub release under stable, unversioned filenames (`.../releases/latest/download/...`), so they never need updating here.
- The app firmware image is versioned (GitHub doesn't offer a stable alias for it without also duplicating it under a fixed name), so the manifest's firmware URL and `version` field need a one-line bump whenever a new release ships.
- `firmware/boot_app0.bin` is bundled directly - it's a static file from the Arduino-ESP32 core, identical across all builds.

## Updating a manifest after a new release

Bump `version` and the firmware asset URL (`.../releases/download/<tag>/...`) in the relevant `manifests/*.json` to the new tag. Bootloader/partitions URLs don't need to change.

## Development

Any static file server works, e.g.:

```sh
python3 -m http.server 8080
```

Open `http://localhost:8080` - Web Serial requires either `localhost` or HTTPS.
