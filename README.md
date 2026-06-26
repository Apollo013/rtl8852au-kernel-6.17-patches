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
