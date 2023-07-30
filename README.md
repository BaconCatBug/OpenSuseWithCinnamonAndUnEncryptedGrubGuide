# openSUSEWithCinnamonAndUnEncryptedGrubGuide
A guide on how to install openSUSE Tumbleweed with up-to-date Cinnamon Desktop Environment, an Unencrypted Grub Bootloader, and Encrypted Root Partition

# Preamble

openSUSE Tumbleweed is a great distro, but is a little rough around the edges for the average user. For one thing, it encrypts the grub partition. In theory, this is good, encryption is good. In practice this serves no practical purpose, as anyone with physical access to your computer can tamper with the bootloader itself, rendering the encryption of the grub partition meaningless. Due to the fact that doing this results in 30 to 60 second decryption times before the OS even begins to boot, this is not really fit for purpose. 

Instead, what we will do is encrypt the root partition of your installation via LUKS which will prompt you for a password *after* grub has done its thing.

One consequence of this is that

Another issue (which admittedly is a personal issue) is that while it's *technically* possible to install Cinnamon while in the install wizard, the official repositories are hopelessly out of date, and cause issues if you try to upgrade it via the community package after the fact. I like Cinnamon, a lot, thus this guide will show you how to install it. Feel free to skip this part if you want one of the default Gnome, KDE, or XFCE desktop environments.

Finally, we will go over the process of enabling hardware accelerated decoding for AMD GPUs that utilise the mesa driver stack. It's a lot simpler than on something like Fedora and in my personal experience a lot less likely to break.

# The Prep Work

Note: If you want to try this out via a Virtual Machine suite (I recommend gnome-boxes myself), make sure you have the VM set to UEFI mode and not BIOS mode.

Also Note: While I will be comprehensive, I will assume you know how to do some basic things, such as plugging in a USB stick, booting to said USB stick, how to use a mouse and keyboard, etc. :P

Also, Also Note: This guide assumes you have a somewhat modern hardware i.e. it has a 64bit CPU and supports and is in UEFI boot mode. 

1. Download the latest openSUSE Tumbleweed **Network Image** if you'll have internet access while installing (e.g. it's got an ethernet cable). Otherwise grab the **Offline Image**. At the time of writing they can be found at https://get.opensuse.org/tumbleweed/

   You want the *Intel or AMD 64-bit desktops, laptops, and servers (x86_64)* images. If you don't need this one, you'll know, and probably won't need this guide.
   
1. Grab a USB stick of some description that you can afford to format and put the ISO onto the USB stuck using your favourite method of doing so. I personally recommend Ventoy https://www.ventoy.net/
1. Boot to the USB Stick, select the ISO, and wait for the installer to load.
1. Select Installation

![alt text][install]

1. dawdawd

[install]: https://i.imgur.com/cMJQZpy.png 