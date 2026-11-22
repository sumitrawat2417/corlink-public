# CorLink — Public Production Distribution & Web Console

![Platform](https://img.shields.io/badge/Platform-Web_Bluetooth_BLE_5.0-blue.svg)
![Target Hardware](https://img.shields.io/badge/Hardware-Cortex_S3_AI_Sense-orange.svg)
![Architecture](https://img.shields.io/badge/Architecture-React_19_|_Vite_|_Tailwind_CSS-purple.svg)
![Deployment](https://img.shields.io/badge/Deployment-Production_Ready-brightgreen.svg)
![License](https://img.shields.io/badge/License-Proprietary_&_Confidential-red.svg)

CorLink is the primary companion operating console and multimodal intelligence bridge for the **Cortex S3 AI** autonomous smartwatch. This repository provides the pre-compiled, optimized production distribution of the CorLink web client, ready for direct deployment on static web infrastructure or local hosting.

---

## System Overview & Architecture

CorLink interfaces between client web browsers and the Cortex S3 AI wearable over ultra-low-latency Web Bluetooth (BLE 5.0). It acts as an edge-intelligence pipeline, telemetry monitor, and device synchronizer:

```
+-------------------------------------------------------------+
|               Cortex S3 AI Smartwatch Hardware             |
|   - XIAO ESP32-S3 Sense (240 MHz Dual-Core, 8MB Octal PSRAM)|
|   - ST7789 240x240 IPS LCD (60 FPS Double-Buffered Engine)  |
|   - OV2640 DVP Camera + MSM261S4030H0R PDM Microphone       |
|   - MPU6050 6-Axis Inertial Measurement Unit (IMU)          |
|   - MicroSD FAT32 Storage Subsystem                         |
+-------------------------------------------------------------+
                               |
                               | Web Bluetooth BLE 5.0
                               | (Core OS GATT 0xFF00 + Multimodal AI 0x19B1)
                               v
+-------------------------------------------------------------+
|                  CorLink Web Client Console                 |
|   - Multimodal AI Orchestrator (Whisper STT + Llama 3 LLM)  |
|   - Look-and-Ask Vision Processing & Text Extraction        |
|   - Sub-Millisecond Time Sync & Ambient Weather Dispatch    |
|   - On-Wrist Application Suite & Macro Deck Controller      |
|   - Spatial Attitude Radar & Zero-Drift Stillness Monitor   |
+-------------------------------------------------------------+
```

---

## Core Capabilities & Features

### 1. Multimodal AI Processing Hub
* **Low-Latency Voice Stream Ingestion:** Ingests uncompressed 8 kHz / 16 kHz PCM audio packets streamed over BLE from the watch microphone.
* **Intelligent VAD & Transcription:** Applies client-side voice activity detection and routes audio to Groq Whisper Large V3 Turbo for near-instant speech recognition.
* **Bidirectional Response Streaming:** Segments LLM responses into MTU-optimized BLE frames and streams them directly to the watch double-buffer graphics engine.
* **Look-and-Ask Visual AI:** Captures, rotates, and inspects VGA JPEG frames from the watch camera, allowing users to ask visual questions about physical objects, printed text, or whiteboards.

### 2. Bidirectional Hardware Synchronization
* **Drift-Free Time Sync:** Transmits microsecond-accurate timestamp arrays to the watch kernel accumulator to eliminate clock drift.
* **Live Weather Engine:** Dispatches ambient temperature, conditions, and custom icons to the watchface live telemetry dial.
* **Notification Relay:** Bridges incoming browser or system notifications into the wearable notification center with application-level filtering.
* **Dynamic QR Generator:** Generates on-wrist QR cards for payments, authentication, and contact cards with auto-exposure compensation.

### 3. Application & Macro Configuration
* **Display & Power Controls:** Precision sliders for backlight duty cycle, auto-dim timeouts, and battery saver profiles.
* **Macro Deck & Media Remote:** Configures physical capacitive touch pads to trigger PC desktop actions, app launches, and media playback.
* **Encrypted Vault Gateway:** Manages encrypted credentials and recovery keys stored securely in on-chip non-volatile storage.

### 4. 3D Spatial Attitude & Zero-Drift Tracking
* **6-Axis IMU Radar:** Renders live pitch and roll crosshairs driven by MPU6050 sensor telemetry.
* **Zero-Drift Stillness Lock:** Monitors the stationary detection barrier (<0.85 deg/s) to ensure mouse pointer stability during air-mouse operation.

---

## BLE Communication Specification

CorLink communicates with the watch via two primary 128-bit GATT services:

### 1. Core OS Control Service
* **Service UUID:** `0000FF00-0000-1000-8000-00805F9B34FB`
* **Command Characteristic (Write):** `0000FF01-0000-1000-8000-00805F9B34FB`
* **Telemetry Characteristic (Notify):** `0000FF02-0000-1000-8000-00805F9B34FB`

| Command Header | Payload Syntax | Description |
| :--- | :--- | :--- |
| `ST` | `ST <yy> <mm> <dd> <hh> <min> <ss> <u>` | Kernel clock synchronization |
| `WE` | `WE <temp_celsius> <icon_id>` | Live weather telemetry update |
| `NO` | `NO <app_name>\|<title>\|<message>` | Relay notification to wearable screen |
| `DT` | `DT <display_text>` | Display instant toast dialog on watch |
| `AL` | `AL <hour_24> <minute>` | Remote alarm creation |
| `TI` | `TI <duration_seconds>` | Countdown timer initialization |
| `FM` | `FM <focus_minutes>` | Pomodoro focus session activation |
| `BR` | `BR <level_0_to_100>` | Backlight PWM brightness setting |
| `BS` | `BS <0_or_1>` | Battery saver toggle |
| `QR` | `QR <slot> <title>\|<payload>` | Dynamic QR code record upload |
| `QD` | `QD <slot>` | Delete QR code slot |
| `SA` | `SA <slot> <package>\|<title>` | Remote macro shortcut assignment |

### 2. Multimodal AI Service
* **Service UUID:** `19b10000-e8f2-537e-4f6c-d104768a1214`
* **Stream Ingestion (Notify):** `19b10001-e8f2-537e-4f6c-d104768a1214` (PCM audio slices and JPEG chunks)
* **Command & Response (Write):** `19b10002-e8f2-537e-4f6c-d104768a1214` (Structured AI answer stream, camera controls)

---

## Deployment & Hosting Guide

The assets in this repository constitute a compiled, standalone Single Page Application (SPA).

### Method 1: Local Static Server
You can serve the production distribution locally using any static web server:

```bash
# Using Node.js npx serve
npx serve .

# Using Python 3
python -m http.server 8080
```

### Method 2: Static Cloud Deployment
This repository can be connected directly to:
* **GitHub Pages:** Set deployment source to the `main` branch root.
* **Vercel / Netlify / Cloudflare Pages:** Connect repository with no build command required (pre-built output).

---

## Browser & System Compatibility

The Web Bluetooth API requires a secure context (`https://` or `localhost`) and a supported browser:

| Browser | Windows | macOS | Linux | Android | iOS |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Google Chrome** | Supported (v70+) | Supported (v70+) | Supported (with flag) | Supported (v70+) | Not supported |
| **Microsoft Edge**| Supported (v79+) | Supported (v79+) | Supported (with flag) | Supported (v79+) | Not supported |
| **Brave**         | Supported        | Supported        | Supported             | Supported        | Not supported |
| **Safari / WebKit**| Not supported   | Not supported    | Not supported         | Not supported    | WebBLE browser |

*Note for Linux users: Enable the `#enable-experimental-web-platform-features` flag in `chrome://flags` if Bluetooth scanning does not trigger automatically.*

---

## Proprietary Notice & Intellectual Property

This software and distribution package is strictly **PROPRIETARY AND CONFIDENTIAL**.

Copyright (c) 2026 Sumit Rawat. All Rights Reserved.

* The source code is maintained in an encrypted, private development repository.
* Unauthorized distribution, reverse engineering, decompilation, or commercial exploitation is strictly prohibited without prior written consent.
