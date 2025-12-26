# 🚀 Production-Grade Bootloader & OTA Update System

A **robust, fail-safe, production-grade bootloader and OTA update system** designed for embedded and automotive-grade firmware.
This project demonstrates a **complete firmware lifecycle** including secure updates, rollback, power-loss recovery, and multi-transport OTA support.

---

## 📌 Overview

This project implements a **dual-bank (A/B) bootloader architecture** capable of safely updating firmware over multiple interfaces while ensuring system reliability under failure conditions such as power loss, corrupted images, or incomplete transfers.

The design follows **real-world automotive and embedded best practices**, focusing on:
- Reliability
- Deterministic behavior
- Security
- Maintainability

---

## ✨ Key Features

### 🔁 Bootloader Capabilities
- Dual-bank (A/B) firmware slots
- Power-loss safe update flow
- Automatic rollback to last-known-good image
- Firmware versioning and retry control
- Watchdog-assisted recovery

### 📡 OTA Update Support
- UART-based firmware update (baseline)
- USB / BLE / Wi-Fi support (configurable, optional)
- Transport-agnostic update engine
- Chunk-based download with ACK/NACK

### 🔐 Security & Integrity
- CRC-based integrity verification
- Signed firmware validation using ECC (ECDSA P-256)
- Anti-rollback protection via version checks
- Secure metadata handling

### 💾 Flash & Memory Management
- Custom linker scripts
- Flash partitioning and alignment
- Metadata journaling for power-failure recovery
- Resume-safe update logic

---

## 🧠 Architecture Overview


---

## 🧠 System Architecture (High Level)

+----------------------+
| Bootloader           |  ← Bare-metal (No OS)
+----------------------+
| Application Slot A   |  ← FreeRTOS Application
+----------------------+
| Application Slot B   |  ← FreeRTOS Application
+----------------------+

- The bootloader always executes first after reset
- Only one application slot runs at a time
- Old and new firmware images co-exist safely during OTA
---

## 🔹 Bootloader Design (Bare-Metal)

Responsibilities:
- Executes immediately after reset
- Validates firmware images (CRC + Signature)
- Manages A/B slot selection
- Handles rollback on failure
- Performs safe firmware installation
- Jumps to application reset handler

Characteristics:
- No OS, deterministic execution
- Minimal flash & RAM usage
- Rarely updated

---

## 🔹 Application Firmware Design (FreeRTOS)

The application runs on FreeRTOS and is responsible for:
- Device functionality
- Wi-Fi / BLE / UART communication
- OTA firmware download
- Update metadata preparation
- Triggering controlled system reboot

+----------------------+
| OTA Task             |  ← Firmware download
| Network Task         |  ← Wi-Fi / BLE stack
| Control Task         |  ← Device logic
| Watchdog Task        |
+----------------------+

---
## 🔄 OTA Update Flow

1. Application downloads firmware over Wi-Fi/BLE/UART.
2. Firmware stored in inactive slot or staging area.
3. Application updates metadata (update pending, target slot).
4. System reset.
5. Bootloader validates and installs firmware.
6. Bootloader switches active slot and reboots.
7. New application confirms successful boot.

---

## 🔁 Rollback & Power-Loss Safety

- Automatic rollback on boot failure
- Watchdog-assisted recovery
- Resume-safe update FSM
- Device never bricks

---

## 🔐 Security Features

- CRC-32 integrity verification
- ECC (ECDSA P-256) firmware signature validation
- Anti-rollback protection
- Protected metadata region

---

## 💾 Flash Memory Layout (Example)

Bootloader
Metadata
Application Slot A
Application Slot B

---
```
Boot ROM
   |
Bootloader
 ├─ Slot Selection & Rollback
 ├─ Image Verification (CRC / ECC)
 ├─ Update State Machine
 ├─ Flash & Metadata Manager
 └─ Transport Abstraction
      ├─ UART
      ├─ USB
      ├─ BLE
      └─ Wi-Fi

Application Slot A <--> Application Slot B
```

---

## 🔄 Firmware Update Flow

```
IDLE → DOWNLOAD → VERIFY → FLASH → VERIFY → ACTIVATE → REBOOT
```

Each stage is restart-safe, ensuring recovery after unexpected resets or power loss.

---

## 🧩 Flash Memory Layout (Example)

```
Bootloader
Metadata
Application Slot A
Application Slot B
```

---

## 🛠 Tech Stack

**Firmware:** C / C++  
**Toolchain:** arm-none-eabi-gcc  
**Build:** CMake, Makefiles  
**Debug:** GDB, OpenOCD, JTAG/SWD  

**Security:** CRC-32, SHA-256, ECDSA (ECC P-256)  
**OTA Interfaces:** UART, USB, BLE, Wi-Fi  

**Host Tools:** Python firmware packaging & signing utilities

---

## 🧪 Testing & Validation

- Power-loss injection testing
- Corrupted image handling
- Invalid signature rejection
- Watchdog recovery tests
- Rollback validation

---

## 🚗 Automotive Relevance

This project mirrors real automotive ECU firmware behavior:
- OTA-ready design
- Fail-safe updates
- Secure boot concepts
- Production reliability standards

---

## 📂 Repository Structure

```
bootloader/
├── core/
├── drivers/
├── security/
├── linker/
├── tools/
└── README.md
```

---

## 📜 License
MIT License

---

## 👤 Author
Nilesh Patil -- Embedded Software Engineer – Automotive / Firmware / Systems
