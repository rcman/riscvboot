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
bootmenu_delay=5<br>
bootmenu_0="-------- Boot Options --------"=run boot_default<br>
bootmenu_1="Boot from Nor"=run nor_boot<br>
bootmenu_2="Boot from Nand"=run nand_boot<br>
bootmenu_3="Boot from MMC"=run mmc_boot<br>
bootmenu_4="Autoboot"=run autoboot<br>
bootmenu_5="Show current Boot Device"=run boot_default<br>
bootmenu_6="-------- Flash Options --------"=run flash_default<br>
bootmenu_7="recovery from usb"=run flash_from_usb<br>
bootmenu_8="recovery from mmc"=run flash_from_mmc<br>

**Example of menu option**

<br>
// Nor+ssd boot combo<br>
set_nor_args=setenv bootargs ${bootargs} mtdparts=${mtdparts} root=${nvme_root} rootfstype=ext4<br>
nor_boot=echo "Try to boot from NVMe ..."; \<br>
         run commonargs; \<br>
         run set_nvme_root; \<br>
         run set_nor_args; \<br>
         run detect_dtb; \<br>
         run loadknl; \<br>
         run loaddtb; \<br>
         run loadramdisk; \<br>
         bootm ${kernel_addr_r} ${ramdisk_combo} ${dtb_addr}; \<br>
         echo "########### boot kernel failed by default config, check your boot config #############"<br>


bootmenu_9="recovery from net"=run flash_from_net<br>

// Variable "boot_device" is set during board_late_init()<br>
autoboot=if test ${boot_device} = nand; then \<br>
                run nand_boot; \<br>
        elif test ${boot_device} = nor; then \<br>
                run nor_boot; \<br>
        elif test ${boot_device} = mmc; then \<br>
                run mmc_boot; \<br>
        fi;<br>

bootcmd=run autoboot; echo "run autoboot"<br>
