# Purge-NVDA
A simple script for Macs that purges the activation of the discrete **NVIDIA** GPUs on **macOS**. This script is a **work-in-progress** for **High Sierra** support - once functional, this will enable **native AMD external graphics support** which was previously not possible on **NVIDIA-based** macs.

## Requirements
This script is for use on **Macs** with integrate graphics + discrete **NVIDIA** GPU. You can see more information about your graphics configuration via **System Information** > **Hardware** > **Graphics/Displays**.

This script supports and has been tested on:
* Mavericks **(10.9.5)**
* Yosemite **(10.10.5)**
* El Capitan **(10.11.6)**
* Sierra **(10.12.6)**

Other minor builds of the operating system will most likely work, but the above builds are confirmed to work. Testing was done on a **Mid-2014 MacBook Pro w/ GeForce GT 750M**.

## Usage
Please ensure that you have a backup of your operating system (or an additional install to browse the modified system's files to revert them manually) before proceeding.

Before using the script, disable **System Integrity Protection** by booting into **Recovery**, opening **Terminal**, and typing in the following commands:
```bash
$ csrutil disable
$ reboot
```

Once booted, you can use the script as follows.

To activate the purge:
```bash
$ sudo ./purge-nvda.sh
```

Your mac will now behave like an iGPU-only device. If you are unable to boot into macOS, boot into recovery, launch **Terminal** and type in the following commands:
```bash
$ nvram -c
$ cd /Volumes/<boot_disk_name>
$ mv Library/Application\ Support/Purge-NVDA/* System/Library/Extensions/
$ reboot
```

After rebooting, **uninstall** the script.

To update the **NVRAM** only:
```bash
$ sudo ./purge-nvda.sh nvram-only
```

This is useful for **iGPU-only** mode for the next boot only. Rebooting again will restore default behavior.

To restore nvram:
```bash
$ sudo ./purge-nvda.sh nvram-restore
```

This restores the NVRAM variables to how they were before running the script.

To completely uninstall changes:
```bash
$ sudo ./purge-nvda.sh uninstall
```

For help with how to use the script in the command line:
```bash
$ ./purge-nvda.sh help
```

Uninstallation recommended before updating macOS.

## The Story
When **Apple** announced native external graphics support for macOS on **Thunderbolt 3** macs, I was ecstatic. Other eGPU users confirmed that it worked on older **Thunderbolt** macs. Being on the Mid-14 MBP w/ 750M, my enthusiasm quickly faded, however, as soon as I plugged in my eGPU and logged out (on High Sierra, of course) - all I could see on the external display was colored lines and glitches. Suspecting that **NVIDIA** drivers were to blame for this, I tried moving kexts associated with the same away from its default location to prevent loading, powered down the Mac, plugged in the eGPU, and booted. It worked (Beta 4)!

I was up and running on my external display - at the cost of no output on the internal display and losing the ability to boot without external graphics connected. So I decided to create a tiny script to help move about the kexts, making it easy to restore the system to its default configuration. Then **@theitsage** on [gpu.io](https://egpu.io) suggested I look into a [macrumors forum](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/page-28#post-24886189) where mac users with failing AMD chips were using the same process to prevent the use of the chip - with one major difference - they were forcing boot on the iGPU. This was what I needed to get the internal display to work and ensure external graphics compatibility. This configuration worked on Beta 4, but does not on Beta 5. How things pan out for later betas remains to be seen.

Due credit goes to the macrumors members (esp. **@nsgr**) on that forum for the **NVRAM** settings that make this possible without requiring a separate **ArchLinux** installation to manually manage these values.

## Disclaimer
This script moves core system files associated with macOS. While any of the potential issues with its application are recoverable, please use this script at your discretion. I will not be liable for any damages to your operating system.

## License
This project is available under the **MIT** license. See the license file for more information.
