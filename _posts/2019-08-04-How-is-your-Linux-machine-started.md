---
published: true
categories: Engineering
---
## How is your Linux machine started ?

Like all of you, when i studied about Linux OS, i asked myself that how Linux OS begins. For a while investigating about this question. I would like to share to all of you some information that i understand.

Firstly, every computer system have two kinds of memory which are ROM and RAM.
So that, the manufacturer will put into ROM a software is called BIOS. By this software we can configure the device priority boot. I assume that it is hard disk.

When we push the power button, the bios base on previous configuration, will find the **Master Boot Record(MBR)** in hard disk. In MBR, the boot sector program -a part of boot loader- will be run to locate the boot loader such as **LILO, GRUB**, stored in hard disk.

Next step, Boot Loader will show to you a menu to choose the Operating System kernel to load to RAM.

In Boot Loader Configuration file, contains the path to kernel and "initrd" files.

After the kernel was loaded into RAM, those "initrd" script files will run to check the architecture system and decide what modules should attach to kernel.
Then the kernel will run a program is /sbin/init. This program read /etc/inittab file. This file configure the level to run the system. In this file, normally, there have 7 run level, from 0 to level sixth.

- 0th: Halt system
- 1th: Single user< root>
- 2th: Multi-user without network
- 3th: Multi-user with network
- 4th: not use
- 5th: Multi-user with network and GUI
- 6th: reboot system.


As default, normally the 5th level is used default.
Example that the 5th level is used, it will run the scripts in rc.d5 folder. This folder contains a lot of links to a lot scripts. Those scripts, will start the necessary subsystems of system.
And the system go to login phase either command line or Graphic User Interface.
