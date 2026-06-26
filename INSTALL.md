# RTL8852AU Driver - Installation Guide

## Overview

This is the **patched rtl8852au driver** for Kernel 6.13–6.17+, adapted for the **TP-Link TX20U Plus** (Realtek 8832AU/8852AU chipset).

**Device:** TP-Link TX20U Plus (USB-ID: `2357:013f`)
**Chipset:** Realtek RTL8832AU / RTL8852AU
**Support:** WiFi 6 (802.11AX), 2.4 GHz & 5 GHz

## Prerequisites

```bash
# For Ubuntu/Debian/Linux Mint
sudo apt-get update
sudo apt-get install make gcc linux-headers-$(uname -r) build-essential git
```

## First Installation

**IMPORTANT:** This driver already includes all patches for Kernel 6.13–6.17. You don't need to apply any patches manually.

1. Navigate to the driver directory:
```bash
cd /media/reddeb/home/Downloads/ArcherRTL8832AU-main/rtl8852au
```

2. Build the driver:
```bash
make -j$(nproc)
```

3. Install the driver:
```bash
sudo make install
```

4. Load the module:
```bash
sudo modprobe 8852au
```

5. Check the interface:
```bash
ip link show
# You should see a wlxf0a7314ab340 (or similar) interface
```

## Reinstallation After Kernel Update

After a kernel update, the driver **MUST** be rebuilt:

```bash
cd /media/reddeb/home/Downloads/ArcherRTL8832AU-main/rtl8852au
make clean
make -j$(nproc)
sudo make install
sudo modprobe -r 8852au && sudo modprobe 8852au
```

## Applied Patches

This driver has been patched with the following kernel compatibility fixes:

| Kernel Version | Change |
|----------------|--------|
| **All** | `EXTRA_CFLAGS` → `ccflags-y` (Kernel 6.15+ ignores EXTRA_CFLAGS) |
| **6.13+** | `MODULE_IMPORT_NS` disabled, `set_monitor_channel` net_device parameter |
| **6.14+** | `get_txpower` link_id parameter |
| **6.15+** | `del_timer` → `timer_delete`, compiler warnings suppressed |
| **6.16+** | `from_timer` → `timer_container_of` |
| **6.17+** | `set_wiphy_params/set_txpower/get_txpower` radio_id parameter |

## Directories

- **Driver source:** `/media/reddeb/home/Downloads/ArcherRTL8832AU-main/rtl8852au`
- **Backup of original files:** `/media/reddeb/home/Downloads/ArcherRTL8832AU-main/rtl8852au.bak.original`
- **Installed module:** `/lib/modules/$(uname -r)/kernel/drivers/net/wireless/realtek/rtw89/8852au.ko`

## Troubleshooting

### Module not loading:
```bash
sudo dmesg | tail -30
# Check for error messages
```

### Interface not appearing:
```bash
sudo modprobe -r 8852au
sudo modprobe 8852au
ip link set wlxf0a7314ab340 up
```

### WLAN scan not working:
```bash
sudo ip link set wlxf0a7314ab340 up
sudo iwlist wlxf0a7314ab340 scan
```

## Original Authors

- **Original driver author:** Larry Finger (1940–2024) - GitHub: `lwfinger/rtl8852au`
- **Patches for Kernel 6.13–6.16:** Soham Nandy, Zenm Chen (GitHub PR #115)
- **Patch for Kernel 6.17:** Andreas Patsalos
- **Currently maintained forks:** `natimerry/rtl8852au`, `pulponair/rtl8852au`

## Test Results on This System

- **OS:** Linux Mint 22.3
- **Kernel:** 6.17.0-35-generic
- **Interface:** wlxf0a7314ab340
- **Mode:** IEEE 802.11AX (WiFi 6)
- **Bitrate:** 1.201 Gb/s
- **Signal:** 71/100
- **Status:** Connected, Internet working

---

**Note:** This driver is "out-of-tree" and must be rebuilt after every kernel update. If you want to automate this, DKMS can be set up.
