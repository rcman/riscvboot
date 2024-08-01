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
