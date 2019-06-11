Archlinux Installation
======================

.. _Download: http://archlinux.org/download/

Download_ the base operating system

ISO Installation
----------------

* You may need to start the network, however this is normally done automatically

    ``dhcpcd ens33``

* Partition the Hard Disk

    ``cfdisk /dev/sda``

* Recommended Settings (GPT)

    * 1M (Bios Boot)
    * 2G (Linux Swap)
    * Remaining Disk Space (Linux Filesystem)

* Format Filesystem

    ``mkfs.ext4 /dev/sda3``

* Create Swap

    ``mkswap /dev/sda2 && swapon /dev/sda2``

* Mount Filesystem

    ``mount /dev/sda3 /mnt``

* Install Base & Base-Devel Packages

    ``pacstrap /mnt base base-devel grub``

    | If you have an AMD CPU, install ``amd-ucode``.
    | If you have an Intel CPU, install ``intel-ucode``.

* Create FSTAB

    ``genfstab -U /mnt >> /mnt/etc/fstab``

* CHROOT

    ``arch-chroot /mnt``

* Set Timezone

    ``ln -sf /usr/share/zoneinfo/UTC /etc/localtime``

* Setup the Hardware Clock

    ``hwclock --systohc``

* Configure Locales

    ``nano /etc/locale.gen``

    Uncomment *en_US.UTF-8 UTF-8*

* Generate Locales

    ``locale-gen``

* Set Language

    ``nano /etc/locale.conf``

    Add *LANG=en_US.UTF-8*

* Set Hostname

    ``nano /etc/hostname``

    Add *localhost*

* Install Images

    ``mkinitcpio -p linux``

* Install Grub

    ``grub-install /dev/sda``

    ``nano /etc/default/grub``

    .. note::
        If you want colors when Grub loads, uncomment *GRUB_COLOR_NORMAL* and *GRUB_COLOR_HIGHLIGHT*

    ``grub-mkconfig -o /boot/grub/grub.cfg``

* Set ROOT Password

    ``passwd root``

* Exit CHROOT & Unmount

    | ``exit``
    | ``umount -R /mnt``

* Reboot

    ``reboot``

Initial Configurations
----------------------

* Setup Pacman

    | ``pacman-key --init``
    | ``pacman-key --populate archlinux``

.. note::
    It's easier to setup the system after installing OpenSSH and use Putty to configure the remaining steps.
    If you decide to do this, use the IP Address and *root* user.

* Install OpenSSH

    ``pacman -Sy openssh``

* Configure OpenSSH

    ``nano /etc/ssh/sshd_config``

    Uncomment *Port*, *AddressFamily*, and both *ListenAddress* lines

* Enable & Start OpenSSH

    ``systemctl enable sshd && systemctl start sshd``

* Install Required Packages

    .. code-block:: none

       acl bind-tools cronie git logrotate mlocate ntp openssh python python-pip wget quota-tools

* Setup & Configure Networking

    .. note::
        *ens33* is the name of your ethernet. If you are uncertain what the current name is you can run ``ip addr``

    ``cp /etc/netctl/examples/ethernet-dhcp /etc/netctl/ens33``

    DHCP is used temporarily. This will be changed at a later time. It's only purpose here is to get things setup quickly.

    ``nano ens33``

    Configure as needed and then enable netctl.

    ``netctl enable ens33``

* Date / Time Sync

    | ``ntpd -qg``
    | ``systemctl enable ntpd``

* Configure Quota

    ``nano /etc/fstab``

    Add *usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv1*

    Example

    .. code-block:: none

       /dev/sda3 / ext4 rw,relatime,data=ordered,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv1 0 1

    Remount Filesystem, Check Quota's, and Enable Quota support

    | ``mount -vo remount /``
    | ``quotacheck -vgum /``
    | ``quotaon -av``

* Configure User Settings

    ``nano ~/.bashrc``

    Configure *.bashrc*

    .. code-block:: none

        alias ls='ls --color=auto'
        PS1='\[\e[1;31m\][\u@\h \W]\$\[\e[0m\] '

    .. note::
        | The PS1 variable sets the console prompt to the color *red*.
        | We recommend setting *non root users* to *green* using this:

        ``PS1='\[\e[1;32m\][\u@\h \W]\$\[\e[0m\] '``

* Link *.bashrc* to *.bash_profile*

    ``echo ". $HOME/.bashrc" > ~/.bash_profile``

* Configure Mod Probe Blacklist

    ``nano /etc/modprobe.d/blacklist.conf``

    Add *blacklist i2c_piix4*

.. toctree::
   :hidden:

   /install/os/archlinux/packages

Required Packages
-----------------

.. note::
    If you know how this server will be used, you can preinstall the :doc:`required packages </install/os/archlinux/packages>` for this machine.

.. note::
    It's highly recommend that you setup :doc:`SSH Keys </install/ssh>` on the server and disable the use of passwords to login via SSH.

* Reboot & Enjoy!

    ``reboot``