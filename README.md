# actron-que-zone-controller — OTA channel

Over-the-air update channel for the Actron Que **wall/zone controller**
(Panlee WT32S3-86S, ESP32-S3). This repo hosts **only** the update manifest
and the firmware binaries — the firmware *source* lives elsewhere.

## How it works
Each panel polls `wall-firmware.json` (raw, over HTTPS) ~every 30 min. If its
`version` differs from the running firmware, the panel downloads the binary at
`url` (a GitHub release asset) and installs it via `esp_https_ota`, then reboots.

```jsonc
// wall-firmware.json
{ "version": "0.2.0",
  "url": "https://github.com/wojt-janowski/actron-que-zone-controller/releases/download/v0.2.0/actron-wall-0.2.0.bin" }
```

## Publishing an update
From the firmware source tree (`esp32-wall/`), with ESP-IDF exported:

```sh
./release-ota.sh 0.3.0
```

That builds the versioned binary, attaches it to a GitHub **release**
(`v0.3.0`), and updates `wall-firmware.json` here. Panels pick it up on their
next check (or immediately via *Settings → Developer → Check for update*).
