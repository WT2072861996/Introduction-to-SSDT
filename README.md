# Introduction to SSDT

A guide to ACPI SSDT tables and their use in Hackintosh.

![ACPI](https://img.shields.io/badge/Topic-ACPI_SSDT-00A3E0)
![Platform](https://img.shields.io/badge/Platform-macOS_|_Hackintosh-0071BC)
![License](https://img.shields.io/badge/License-MIT-green)

---

## Table of Contents

- [What is SSDT?](#what-is-ssdt)
- [The Role of SSDT](#the-role-of-ssdt)
- [Tools](#tools-needed)
- [Creating SSDTs](#creating-ssdts)
- [Common SSDTs](#common-ssdts)
- [Example: SSDT-EC](#example-creating-ssdt-ec)

---

## What is SSDT?

SSDT (Secondary System Description Table) is part of the ACPI (Advanced Configuration and Power Interface) specification. It is used by the OS to discover and configure hardware at boot.

## The Role of SSDT

- Extend or modify the main DSDT (Differentiated System Description Table)
- Define additional hardware devices not present in DSDT
- Fix macOS compatibility on non-Apple hardware

## Tools Needed

| Tool | Purpose | Link |
|:-----|:--------|:-----|
| **MaciASL** | Edit & compile ACPI tables | [acidanthera/MaciASL](https://github.com/acidanthera/MaciASL) |
| **iASL** | ACPI compiler | [ACPICA](https://acpica.org/downloads/iasl-compiler) |
| **SSDTTime** | Auto-generate SSDTs | [corpnewt/SSDTTime](https://github.com/corpnewt/SSDTTime) |

## Creating SSDTs

**1. Extract ACPI tables** — Use OpenCore/Clover Dump ACPI option

**2. Decompile:**
```bash
iasl -d DSDT.aml SSDT*.aml
```

**3. Edit** — Open `.dsl` files with MaciASL

**4. Compile:**
```bash
iasl SSDT.dsl
```

**5. Install** — Place `.aml` files in bootloader's ACPI directory

## Common SSDTs

| SSDT | Purpose |
|:-----|:--------|
| **SSDT-EC** | Fix Embedded Controller conflicts |
| **SSDT-USBX** | USB power properties |
| **SSDT-PLUG** | CPU power management (Skylake+) |
| **SSDT-PNLF** | Backlight control (laptops) |

## Example: Creating SSDT-EC

1. Run [SSDTTime](https://github.com/corpnewt/SSDTTime)
2. Select `SSDT-EC`
3. Place generated `SSDT-EC.aml` in ACPI folder
4. Reference it in `config.plist`

---

## References

- [ACPI Specification](https://uefi.org/specifications)
- [OpenCore](https://github.com/acidanthera/OpenCorePkg)
- [Clover Bootloader](https://github.com/CloverHackyColor/CloverBootloader)
- [SSDTTime](https://github.com/corpnewt/SSDTTime)
- [MaciASL](https://github.com/acidanthera/MaciASL)

---

*Licensed under the [MIT License](LICENSE).*
