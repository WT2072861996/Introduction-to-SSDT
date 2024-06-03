# Introduction to SSDT

## What is SSDT?

SSDT, or Static System Description Table, is part of the ACPI (Advanced Configuration and Power Interface) specification. ACPI is an open standard used by operating systems to discover and configure computer hardware, manage power usage, and control hardware devices.

SSDTs are provided by the BIOS/UEFI at system startup and describe the hardware configuration and related information of the system. They are often used to define and extend the hardware devices in a system, including CPUs, memory, PCI devices, and more.

## The Role of SSDT

1. **Extending and Modifying DSDT**: The DSDT (Differentiated System Description Table) is one of the primary ACPI tables that define most of the system hardware devices. SSDTs can be used to extend or modify the contents of the DSDT.

2. **Describing Additional Devices**: SSDTs can define additional hardware devices that are not included in the DSDT at system startup.

3. **Optimizing and Fixing Hardware Support**: In the Hackintosh community, SSDTs are commonly used to fix and optimize macOS compatibility on non-Apple hardware.

## Creating and Using SSDTs

### Tools Needed

To create and edit SSDT files, you will need some tools:

- [MaciASL](https://github.com/acidanthera/MaciASL): A macOS application for editing and compiling ACPI tables.
- [iASL](https://acpica.org/downloads/iasl-compiler): ACPI Source Language compiler.
- [SSDTTime](https://github.com/corpnewt/SSDTTime): A tool for automatically generating common SSDT files.

### Basic Steps to Create SSDTs

1. **Extract Original ACPI Tables**:
   - Use Clover or OpenCore bootloader to extract the original ACPI tables of your system. You can do this by selecting the Dump ACPI option during bootloader startup.

2. **Decompile DSDT and SSDTs**:
   - Use iASL to decompile the extracted DSDT and SSDT files. Command:
     ```bash
     iasl -d DSDT.aml SSDT*.aml
     ```

3. **Edit SSDTs**:
   - Open the decompiled `.dsl` files with MaciASL or any text editor and make the necessary edits.

4. **Compile SSDTs**:
   - After editing, compile the SSDT files using iASL. Command:
     ```bash
     iasl SSDT.dsl
     ```

5. **Place SSDT Files**:
   - Place the compiled SSDT files (.aml format) into the ACPI directory of Clover or OpenCore and reference them in the configuration file.

### Common SSDTs

1. **SSDT-EC**: Used to fix Embedded Controller (EC) devices to avoid conflicts.
2. **SSDT-USBX**: Defines USB power properties to optimize power management for USB devices.
3. **SSDT-PLUG**: Enables CPU power management, especially for Skylake and newer CPUs.
4. **SSDT-PNLF**: Fixes and enables backlight control, suitable for laptop displays.

## Example: Creating SSDT-EC

1. **Using SSDTTime**:
   - Download and run [SSDTTime](https://github.com/corpnewt/SSDTTime).
   - Select the `SSDT-EC` option to automatically generate the SSDT-EC file.

2. **Compile SSDT-EC**:
   - The generated `.dsl` file will be automatically compiled into a `.aml` file.

3. **Place SSDT-EC**:
   - Place the generated `SSDT-EC.aml` file into the ACPI directory of Clover or OpenCore.

4. **Configure Bootloader**:
   - Add a reference to SSDT-EC in the `config.plist` of Clover or OpenCore.

## Conclusion

SSDT is an important part of the ACPI specification, widely used to define and extend hardware device information. In the Hackintosh world, SSDT files are especially important as they help optimize and fix macOS compatibility on non-Apple hardware. By understanding and using SSDTs, you can better control and optimize your system hardware configuration, achieving more efficient hardware management and more stable system performance.

## Reference Resources

- [ACPI Specification](https://uefi.org/specifications)
- [Clover Bootloader](https://github.com/CloverHackyColor/CloverBootloader)
- [OpenCore Bootloader](https://github.com/acidanthera/OpenCorePkg)
- [SSDTTime](https://github.com/corpnewt/SSDTTime)
- [MaciASL](https://github.com/acidanthera/MaciASL)

## License

This project is licensed under the MIT License. For more details, see the [LICENSE](LICENSE) file.
