---
published: true
categories: Engineering
---
## How is your Linux machine started?

Like all of you, when I studied Linux OS, I asked myself that how Linux OS begins. For a while researching this question. I would like to share with all of you some information that I understand.

Firstly, every computer system has two kinds of memory which are ROM and RAM.
So that, the manufacturer will put into ROM software is called BIOS. With this software, we can configure the device priority boot. I assume that it is a hard disk.

When we push the power button, the bios base on the previous configuration will find the **Master Boot Record(MBR)** in the hard disk. In MBR, the boot sector program -a part of boot loader- will be run to locate the boot loader such as **LILO, GRUB**, stored in the hard disk.

Next step, Boot Loader will show to you a menu to choose the Operating System kernel to load to RAM.

In Boot Loader Configuration file, contains the path to kernel and "initrd" files.

After the kernel was loaded into RAM, those "initrd" script files will run to check the architecture system and decide what modules should attach to the kernel.
Then the kernel will run a program is /sbin/init. This program read /etc/inittab file. This file configures the level to run the system. In this file, normally, there have 7 run levels, from 0 to level sixth.

- 0th: Halt system
- 1th: Single user< root>
- 2nd: Multi-user without network
- 3rd: Multi-user with network
- 4th: not use
- 5th: Multi-user with network and GUI
- 6th: reboot system.


As default, normally the 5th level is used default.
For example that the 5th level is used, it will run the scripts in rc.d5 folder. This folder contains a lot of links to a lot of scripts. Those scripts will start the necessary subsystems of the system.
And the system goes to the login phase either the command line or Graphic User Interface.
