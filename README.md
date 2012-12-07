arch-install - Simple Arch Linux Install Script
===============================================

 * Author: Tom Wambold <tom5760@gmail.com>
 * Copyright (c) 2012 Tom Wambold

This is just a simple shell script I use to install my own personal systems
with [Arch Linux][arch], with some options to customize the install a bit.

This hasn't been extensively tested (yet), and is tailored to the particular
setup I prefer, which might not necessarily be what you prefer, so please
read through the script (the whole thing!) before installing.

[arch]: https://www.archlinux.org/

If you improve this script in any way, or have any suggestions, please don't
hesitate to let me know.  Though, I'm not trying to turn this into a universal
Arch installer, just something to install a very specific (desktop system
targetted) setup.

Process
-------

 1. READ THE [ARCH INSTALL DOCS][docs].  YOU SHOULD STILL KNOW HOW THIS SCRIPT
    IS WORKING AT LEAST AT A BASIC LEVEL.

 2. READ THE WHOLE SCRIPT.  In particular, the `install_packages()`
    function.  Make sure to set the variables at the top of the script to what
    you want.

 3. Download an [Arch Linux installer ISO][iso] and boot it on the system you
    want to install.
    
 4. Copy the `arch_install.sh` script to the live system.

    With ssh, start the ssh daemon first:

        systemctl start sshd.service

 5. Make the `arch_install.sh` script executable.

        chmod +x arch_install.sh

 6. Run the script.

        ./arch_install.sh

 7. If there were no errors, reboot and enjoy!

[docs]: https://wiki.archlinux.org/index.php/Official_Arch_Linux_Install_Guide
[iso]: https://www.archlinux.org/download/

Details
-------

By default, the script will create two physical partitions, a 100MB /boot
partition, and the rest a LUKS encrypted partition with LVM on top of that, and
root and swap on top of that.  Encryption can be disabled, but it has pretty
minimal overhead, so I'd use it on a system like a laptop or something.

After partitioning, the base and base-devel package groups are installed, along
with a bunch of other useful packages that I usually end up installing.  This
is done in the `install_packages()` function, so please read it to see what
packages will be installed.  Feel free to remove any you won't use, or add any
you need.

After installing packages, the script sets up all the common configuration
files based on the values of the variables at the top of the script.  You
should also at least browse through the config files here to make sure they are
what you want to do.

By default, no desktop environment or login manager is installed (although X
and a bunch of other graphical programs are).  So once you reboot into your
running system, you'll need to pick one.  The [Arch wiki][de-wiki] has articles
on a lot of them.

[de-wiki]: https://wiki.archlinux.org/index.php/Category:Desktop_environments

FAQ
---

### Something went wrong in the script and I need to start over!

First, see if you can figure out what went wrong, then either send me a mail or
(preferably), create an issue on the GitHub project page (and maybe attach a
patch?).

Then, when you want to re-run the install script, do this in the live system:

    umount /mnt/boot
    umount /mnt
    swapoff /dev/vg00/swap
    vgchange -an
    vgremove vg00

Now, you should be able to re-run the install script cleanly.

### I can't seem to override the ZSH config!

Uninstall the `grml-zsh-config` package, it installs a global ZSH config.

    pacman -Rsc grml-zsh-config
