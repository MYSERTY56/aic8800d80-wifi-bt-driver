# AIC8800D80 Wi-Fi 6 + Bluetooth 5.4 Linux Driver

Automated native Linux driver installer package for USB Wi-Fi 6 (802.11ax) + Bluetooth 5.4 combo dongles based on the **AICSemi AIC8800D80 / AX900 / 88M80** chipset.

---

> [!IMPORTANT]
> **Hardware Target Notice:**
> * This package is specifically engineered for **AIC8800D80 / AX900 / 88M80** USB adapters (USB ID `1111:1111` in initial mass-storage mode, mode-switched to `a69c:8d81` in Wi-Fi + Bluetooth mode).
> * **This package is NOT for Realtek RTL8192EU adapters** (such as USB ID `0bda:818b`). Do not install this package for Realtek-based adapters.

---

## Package Overview

- **Package Filename:** `aic8800d80-wifi-bt-driver_1.0.0_amd64.deb`
- **Version:** `1.0.0`
- **Architecture:** `amd64` / `x86_64`
- **License:** GPL v2
- **Supported Kernels:** Tested on Linux Kernel `6.x` and `7.0+` (Ubuntu 24.04 LTS / Ubuntu 26.04 LTS)

### Included Components

1. **Wi-Fi 6 DKMS Driver (`aic8800_fdrv`)**:
   Automatically compiles and integrates with your Linux kernel, updating seamlessly on kernel upgrades.
2. **Bluetooth 5.4 Driver (`aic_btusb`)**:
   Native Linux HCI driver integrated with standard BlueZ stack (`CONFIG_BLUEDROID=0`).
3. **Firmware Blobs**:
   Full Radxa firmware binaries (`/lib/firmware/aic8800D80/`), including Bluetooth patch binaries (`fw_patch_8800d80_u02_ext0.bin`, `fw_patch_table_8800d80_u02.bin`, `fw_adid_8800d80_u02.bin`).
4. **Automated USB Mode-Switching**:
   Custom `usb_modeswitch` SCSI eject sequences and `udev` rules to transition the adapter from CD-ROM driver disk mode (`1111:1111`) to active Wi-Fi + BT mode (`a69c:8d81`).
5. **Conflict Resolution**:
   Modprobe configuration to prevent generic `btusb` conflict and prioritize `aic_btusb`.

---

## Installation

Download or clone the `.deb` file and install using `apt`:

```bash
sudo apt update
sudo apt install ./aic8800d80-wifi-bt-driver_1.0.0_amd64.deb
```

Once installed, unplug and re-plug the USB adapter.

---

## Verification

### 1. Check USB Device Mode
```bash
lsusb
```
Expected output:
```text
Bus ... Device ...: ID a69c:8d81 AICSemi ...
```

### 2. Check Wi-Fi Interface
```bash
nmcli device status
```
or
```bash
ip link
```

### 3. Check Bluetooth Controller
```bash
bluetoothctl show
```
or
```bash
hciconfig -a
```
The controller (`hci0`) should be `UP RUNNING` with `HCI Version: 5.4`.

---

## Uninstallation

To completely remove the driver package and DKMS modules:

```bash
sudo apt remove --purge aic8800d80-wifi-bt-driver
```
