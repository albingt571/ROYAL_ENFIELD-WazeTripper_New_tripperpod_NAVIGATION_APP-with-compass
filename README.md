# Royal Enfield WazeTripper Navigation App (with Compass & Pod Integration)

An integrated turn-by-turn navigation bridge between **Waze (Android)** and the **Royal Enfield Tripper Pod** (tested on Royal Enfield Hunter 350, Meteor 350, Super Meteor 650, Himalayan 450, Scram 411, and Classic 350).

---

## 🚀 Overview

This repository provides both the **ready-to-install Android APK** (`wazetripper.apk`) and the **full build system** that patches the official Waze APK to establish a direct Bluetooth Low Energy (BLE) connection with the Royal Enfield Tripper Pod (`RE_DISP`).

### Key Features
- **Real-Time Turn-by-Turn Navigation:** Live maneuvers, directional arrows, distance countdowns, and roundabout exit numbers beamed straight to the Tripper display.
- **Built-in Digital Compass:** Directional heading displayed directly on the Tripper pod when idle or cruising without an active navigation route.
- **Speed & Radar Alerts:** Speed camera and safety enforcement alerts within 300 meters.
- **Day / Night Synchronization:** Automatically syncs display theme with Waze's day/night mode.
- **Automatic BLE Reconnection:** Smart reconnect window when the bike's ignition is turned on or if connection drops.
- **Native Floating Control Overlay:** Integrated directly into Waze's UI with a status indicator (Green = Connected, Orange = Connecting, Red = Disconnected).

---

## 📥 Quick Download & Installation

The compiled and signed APK is tracked in this repository via **Git LFS**:
- **File:** `wazetripper.apk` (~144 MB)
- **Base Version:** Waze v5.23.0.2

### Step 1: Install APK on Android Phone
1. Download `wazetripper.apk` onto your phone (or install via ADB):
   ```bash
   adb install -r wazetripper.apk
   ```
2. Grant Bluetooth, Nearby Devices, and Location permissions when prompted.

---

## 📱 Crucial: Prevent Disconnection When Phone is Locked

Android (especially **Samsung One UI**, **Xiaomi MIUI/HyperOS**, and **OnePlus OxygenOS**) aggressively kills background Bluetooth connections when the screen turns off. The Tripper Pod has a **5-second watchdog timer**—if it does not receive a packet within 5 seconds, it will disconnect.

### 1. Disable Battery Optimization (Required)
- Go to **Settings > Apps > Waze**.
- Tap **Battery** and select **Unrestricted**.
- On Samsung:
  - Go to **Settings > Battery and device care > Battery > Background usage limits**.
  - Add **Waze** to **Never auto-sleeping apps** / **Never sleeping apps**.
  - Make sure Waze is **not** in *Deep sleeping apps*.

### 2. Location Permission
- Go to **Settings > Apps > Waze > Permissions > Location**.
- Set to **Allow all the time** (or ensure precision location is allowed in background).

### 3. Keep Route Active
- Always initiate route guidance before locking the screen or slipping the phone into your pocket / mount.

---

## 🏍️ How to Pair with Royal Enfield Tripper Pod

1. **Park Safely:** Keep your motorcycle stationary.
2. **Turn on Motorcycle Ignition:**
   - The Royal Enfield Tripper Pod only advertises over Bluetooth (`RE_DISP`) for a short window (~60-90 seconds) after ignition is turned on.
3. **Open Waze:**
   - Tap the circular **Tripper button** on the right side of the map overlay.
   - Tap **Connect**.
4. **Enter Pairing PIN:**
   - Enter the 6-digit PIN shown on the Tripper Pod screen and confirm.
   - The floating indicator will turn **green** once connected.
5. **Start Navigating:**
   - Search for your destination and tap **Drive**. Your Tripper Pod will immediately receive maneuvers and turn arrows.

---

## 🛠️ Build Pipeline & Source

This project builds reproducible patches against Waze using Docker, Apktool, D8, and Smali:

- `src/com/waze/wazetripper/`: Java sources for the BLE GATT client, keepalive manager, protocol parser, and UI overlay.
- `patches/apply_patches_wazetripper.py`: Smali bytecode patch injector for Waze navigation and lifecycle hooks.
- `scripts/`: Shell scripts for fetching, decompiling, grafting DEX bytecode, and repacking the APK.

### Rebuilding via Docker
```bash
cd wazetripper
bash scripts/build-image.sh
bash scripts/all.sh
```

---

## 📄 License & Credits
- Based on the open-source WazeTripper protocol bridge by João Dias.
- Waze is a trademark of Google LLC. This project is not affiliated with or endorsed by Royal Enfield or Google.
