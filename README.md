# RTL8852AU Driver - Kernel 6.13-6.17+ Patches

**Tested on TP-Link TX20U Plus (USB ID: `2357:013f`)**

This fork includes patches for **Linux Kernel 6.13-6.17+** compatibility. The original driver was not updated for these kernel versions, which introduced API changes.

## Applied Patches

| Kernel Version | Change |
|----------------|--------|
| **All** | `EXTRA_CFLAGS` → `ccflags-y` (Kernel 6.15+ ignores EXTRA_CFLAGS) |
| **6.13+** | `MODULE_IMPORT_NS` disabled, `set_monitor_channel` net_device parameter |
| **6.14+** | `get_txpower` link_id parameter |
| **6.15+** | `del_timer` → `timer_delete`, compiler warnings suppressed |
| **6.16+** | `from_timer` → `timer_container_of` |
| **6.17+** | `set_wiphy_params/set_txpower/get_txpower` radio_id parameter |

## Quick Installation

```bash
# Build
make -j$(nproc)

# Install
sudo make install

# Load module
sudo modprobe 8852au
```

## Rebuild After Kernel Update

```bash
make clean
make -j$(nproc)
sudo make install
sudo modprobe -r 8852au && sudo modprobe 8852au
```

## Tested System

- **Device:** TP-Link TX20U Plus (USB ID: `2357:013f`)
- **OS:** Linux Mint 22.3
- **Kernel:** 6.17.0-35-generic
- **Status:** Working at 1.2 Gb/s on WiFi 6 (5 GHz)

---

## Original README

This repo is was started with the code from the Realtek USB driver
RTL8852AU_WiFi_linux_v1.15.0.1-0-g487ee886.20210714. The current code improves
on the Realtek code by reworking the debug output to avoid spamming the logs.
In the current settings, messages from RTW_ERR(), RTW_WARNING(), and
RTW_WARNING() will be output.

If you want more output, increase the value of CONFIG_RTW_LOG_LEVEL in Makefile.
This parameter should probably be one that can be set at module load time,
but that is a matter for another time.

The driver supports rtl8832au/rtl8852au chipsets.

This driver currently handles the following devices:

* BUFFALO WI-U3-1200AX2(/N) with USB ID 0411:0312
* ASUS USB-AX56 with USB ID 0b05:1997
* ASUS USB-AX56 with USB ID 0b05:1a62
* EDUP EP-AX1696GS with USB ID 0bda:8832
* Fenvi FU-AX1800P with USB ID 0bda:885c
* Realtek Demo Board with USB ID 0bda:8832
* Realtek Demo Board with USB ID 0bda:885a
* Realtek Demo Board with USB ID 0bda:885c
* D-Link DWA-X1850 with USB ID 2001:3321
* TP-Link AX1800 with USB ID 2357:013f or 2357:0141
* ipTIME AX2000U with USB ID 0bda:8832
* ELECOM WDC-X1201DU3 with USB ID 056e:4020

---

## Original Author

Larry Finger (1940–2024) - GitHub: `lwfinger/rtl8852au`
