# riscvboot
<br>
I'm so sick of old installs, u-boot, sbi all that bullshit. I want to install what I want when I want. Making my own custom boot to do that!
<br>
https://github.com/ultraembedded/biriscv
(https://github.com/ultraembedded/riscv-linux-boot
<br>
https://github.com/ultraembedded/riscv<br>
https://www.sifive.com/blog/all-aboard-part-6-booting-a-risc-v-linux-kernel)
<br>
https://popovicu.com/posts/bare-metal-programming-risc-v/
<br>

# How it boots
<br>
https://github.com/u-boot/u-boot/blob/master/doc/arch/riscv.rst
<br>
Compiling u-boot by default does not compile with many options. Ideally you want u-boot to scan for all devices on your machine.  and then have a menu to select where you want to boot from.
<br>
The default config file for u-boot is in include/env_default.h and items should be added like below.<br>
CONFIG_BOOTDELAY=30<br>
CONFIG_AUTOBOOT_MENU_SHOW=y<br>
CONFIG_USE_PREBOOT=y<br>
CONFIG_PREBOOT="pci enum; usb start; scsi scan; nvme scan; virtio scan"<br>
CONFIG_SPL_PCI=y<br>
CONFIG_SPL_PCI_PNP=y<br>
CONFIG_SPL_NVME=y<br>
CONFIG_SPL_NVME_PCI=y<br>
CONFIG_SPL_NVME_BOOT_DEVICE (number of the NVMe device)<br>
CONFIG_SYS_NVME_BOOT_PARTITION (partition to read from)<br>
To load from a file system use:<br>

CONFIG_SPL_FS_FAT=y or CONFIG_SPL_FS_EXT=y<br>
CONFIG_SPL_FS_LOAD_PAYLOAD_NAME=”<filepath>”<br>
