# Dell Optiplex 3020M EFI



This repository contains EFI files for my Dell Optiplex 3020M, tested and confirmed to work with macOS Catalina 10.15.7.

## System Specifications

- **Name:** ![Dell Optiplex 3020M](https://www.hardware-corner.net/desktop-models/Dell-OptiPlex-3020M/)
- **CPU:** [Intel® Core™ i5-4590T](https://www.intel.com/content/www/us/en/products/sku/78928/intel-core-i54590t-processor-6m-cache-up-to-3-00-ghz/specifications.html)
- **GPU:** Intel HD Graphics 4600
- **RAM:** 8GB DDR3 1600Mhz (Dual Channel)
- **SSD:** WDC Blue SATA 500GB
- **Networking:** Generic Intel PCIE NIC (7265)

![About Mac Window](https://github.com/tbwcjw/Dell-Optiplex-3020M-EFI/blob/main/Images/aboutmac.png?raw=true) 

## Pre-installation Steps

Ensure the following BIOS settings are configured:

- **General → Boot Sequence → Boot List Option:** UEFI
- **General → Advance Boot options → Enable Legacy Operation ROMs:** Disabled
- **General → UEFI Boot Path Security:** Never
- **System Configuration → Integrated NIC:** Enabled
- **System Configuration → SATA Operation:** AHCI
- **Security → TPM Security:** Uncheck TPM Security
- **Secure Boot → Secure Boot Enable:** Disabled
- **Power Management → Enable USB Wake Support From Standby:** Uncheck
- **Power Management → Deep Sleep Control:** Disabled
- **Power Management → Wake on LAN/WLAN:** Disabled
- **Virtualization Support → Virtualization:** Enabled

## Important BIOS settings

1. Download [modGRUBShell](https://github.com/datasone/grub-mod-setup_var/releases) and place it in the `EFI/OC/Tools` folder. Add it to the `Misc → Tools` section of `config.plist`.
2. Boot into OpenCore and select the `modGRUBShell` option.
3. Enter the following values:

   - **Disable CFG Lock:**
     ```
     setup_var 0xD9F 0x0
     ```

   - **Set DVMT pre-alloc to 64MB:**
     ```
     setup_var 0x263 0x2
     ```

   - **Enable EHCI hand-off:**
     ```
     setup_var 0x2 0x1
     setup_var 0x144 0x1
     setup_var 0x15A 0x2
     setup_var 0x146 0x0
     setup_var 0x147 0x0
     ```

4. Reboot the system when done.

## Tips

1. VGA is not supported by macOS Catalina. Installer will never go graphical without a DisplayPort display.
2. AirportItlwm.kext is used for networking. It is macOS version specific. To suit your version, you can find compatible kexts [here](https://github.com/OpenIntelWireless/itlwm/releases). Remember to update `config.plist` with the new filename.
3. Press spacebar within the OpenCore loader to show hidden options such as `Clear NVRAM`.
4. To use iCloud, App Store, etc. You must first generate a new SMBIOS and update `config.plist` with the generated values (Refer to the image below). Make sure to generate SMBIOS for `iMac14,3`.
5. For the 'ROM' line in the SMBIOS: Find your machines MAC address, remove the ':' delimiters and [encode](https://www.base64encoder.io/) in base64.
6. When macOS installation is completed. Use a tool like [EFIAgent](https://github.com/benbaker76/EFI-Agent/releases) to mount the EFI partition to the EFI partition of your machine.

![config.plist smbios lines](https://raw.githubusercontent.com/tbwcjw/Dell-Optiplex-3020M-EFI/main/Images/smbios-config-lines.png)

Feel free to contribute or report issues. Happy hacking! 🚀
