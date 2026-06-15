# 📘 Introduction to SSDT — ACPI 自定义入门

<p align="center">
  <img src="https://img.shields.io/badge/主题-ACPI_SSDT-00A3E0" alt="SSDT" />
  <img src="https://img.shields.io/badge/平台-macOS_|_Hackintosh-0071C5?logo=apple&logoColor=white" alt="macOS" />
  <img src="https://img.shields.io/badge/语言-English-blue" alt="Language" />
</p>

<div align="center">
  <i>Understanding SSDT (Secondary System Description Table) and how to create and use SSDTs in Hackintosh.</i>
</div>

---

## Table of Contents

- [What is SSDT?](#what-is-ssdt)
- [The Role of SSDT](#the-role-of-ssdt)
- [Tools Needed](#tools-needed)
- [Creating SSDTs](#basic-steps-to-create-ssdts)
- [Common SSDTs](#common-ssdts)
- [Example: SSDT-EC](#example-creating-ssdt-ec)
- [References](#reference-resources)

---

## What is SSDT?

**SSDT** (Secondary System Description Table) is part of the **ACPI** (Advanced Configuration and Power Interface) specification. ACPI is an open standard used by operating systems to discover and configure computer hardware, manage power, and control devices. SSDTs are provided by the BIOS/UEFI at startup and describe hardware configuration and device information.

---

## The Role of SSDT

| Function | Description |
|:--------:|:------------|
| **Extend DSDT** | Add or modify hardware definitions in the main DSDT table |
| **Describe Devices** | Define additional hardware not included in DSDT |
| **Fix macOS Support** | Optimize and fix macOS compatibility on non-Apple hardware |

---

## Tools Needed

| Tool | Purpose | Link |
|:----:|:-------:|:----:|
| **MaciASL** | Edit and compile ACPI tables | [GitHub](https://github.com/acidanthera/MaciASL) |
| **iASL** | ACPI Source Language compiler | [ACPICA](https://acpica.org/downloads/iasl-compiler) |
| **SSDTTime** | Auto-generate common SSDTs | [GitHub](https://github.com/corpnewt/SSDTTime) |

---

## Creating SSDTs

### Step 1: Extract Original ACPI Tables
- Use OpenCore or Clover to dump ACPI tables (Dump ACPI option)

### Step 2: Decompile
```bash
iasl -d DSDT.aml SSDT*.aml
```

### Step 3: Edit
- Open `.dsl` files with MaciASL and make necessary edits

### Step 4: Compile
```bash
iasl SSDT.dsl
```

### Step 5: Install
- Place compiled `.aml` files in Clover/OpenCore ACPI directory

---

## Common SSDTs

| SSDT | Purpose |
|:----:|:--------|
| **SSDT-EC** | Fix Embedded Controller to avoid conflicts |
| **SSDT-USBX** | Define USB power properties |
| **SSDT-PLUG** | Enable CPU power management (Skylake+) |
| **SSDT-PNLF** | Fix backlight control for laptops |

---

## Example: Creating SSDT-EC

1. Run [SSDTTime](https://github.com/corpnewt/SSDTTime)
2. Select `SSDT-EC` option
3. Place generated `SSDT-EC.aml` in ACPI folder
4. Add reference in `config.plist`

---

## Reference Resources

- [ACPI Specification](https://uefi.org/specifications)
- [OpenCore](https://github.com/acidanthera/OpenCorePkg)
- [Clover Bootloader](https://github.com/CloverHackyColor/CloverBootloader)
- [SSDTTime](https://github.com/corpnewt/SSDTTime)
- [MaciASL](https://github.com/acidanthera/MaciASL)

---

## License

Licensed under the [MIT License](LICENSE).

---

<div align="center">
  <img src="https://img.shields.io/badge/Built_with_❤️_for_the_Hackintosh_Community-FF6B6B" alt="Built with love">
</div>
