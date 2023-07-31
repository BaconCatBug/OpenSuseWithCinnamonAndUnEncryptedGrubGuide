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

1) Download the latest openSUSE Tumbleweed **Network Image** if you'll have internet access while installing (e.g. it's got an ethernet cable). Otherwise grab the **Offline Image**. At the time of writing they can be found at https://get.opensuse.org/tumbleweed/

   You want the *Intel or AMD 64-bit desktops, laptops, and servers (x86_64)* images. If you don't need this one, you'll know, and probably won't need this guide.
   
2) Grab a USB stick of some description that you can afford to format and put the ISO onto the USB stuck using your favourite method of doing so. I personally recommend Ventoy https://www.ventoy.net/
3) Boot to the USB Stick, select the ISO, and wait for the installer to load.


# The Installation

4) Select Installation

![][Installation]

5) Choose your Language and test your keyboard layout. Click **Next**.

![][Language]

6) Click **No**. We'll update everything later.

![][SayNo]

7) Click the **Desktop with Gnome** radio button. Click **Next**.

![][Gnome]

8) Click the drop-down and select **Start with Existing Partitions**.

![][Existing]

9) Delete any partitions from the drive you're going to install on if they exist. Click on the drive you're going to install on, then 

![][EmptyDrive]

10) Set a **Custom Size** of **0.5 GiB**. Click **Next**. Select **Raw Volume (unformatted)**. Click **Next**. Click **Format Device**. Select **FAT**. Click **Mount Device**. Set the Mount Point to **/boot/efi**. Leave Encrypt Device **UNSELECTED**.

![][FirstPart]

11) Click **Add Partition**. Set a **Custom Size** of **1 GiB**. Click **Next**. Select **Raw Volume (unformatted)**. Click **Next**. Click **Format Device**. Select **Ext4**. Click **Mount Device**. Set the Mount Point to **/boot**. Leave Encrypt Device **UNSELECTED**.

![][SecondPart]

11) Click **Add Partition**. Select **Maximum Size**. Click **Next**. Select **Raw Volume (unformatted)**. Click **Next**. Click **Format Device**. Select **Ext4**. Click **Mount Device**. Set the Mount Point to **/**. Set Encrypt Device **SELECTED**. (In all my testing, it looks like btrfs breaks this when trying to do this, you can try it but expect to have to reinstall).

![][ThirdPart]

12) Set your encryption password. Make it a good one. Keep in mind that the language/keyboard locale you set in step 5 will affect the boot menu (at least for me it gets set to GB keyboard), so try avoiding special characters that aren't the same on a US Keyboard for safety.

![][Encrypt]

13) Click **Accept**, then click **Next**, then set your Region and Time Zone, then click **Next**.

![][Accept]

14) Set your Name, Username, and account password. Since you have boot encryption, it should be ok to set a short (even a single character) user password. It will complain about it being too short, just click **Yes**. Leave **Use this password for system administrator** on, and have Automatic Login set to your liking (I say leave it on, as you have on boot encryption). Now click **Next**, then click **Install** (twice).

![][Account]
![][Install]

[Installation]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/1-Installation.png?raw=true 
[Language]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/2-Language.png?raw=true 
[SayNo]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/3-SayNo.png?raw=true 
[Gnome]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/4-Gnome.png?raw=true 
[Existing]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/5-Existing.png?raw=true 
[EmptyDrive]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/6-EmptyDrive.png?raw=true 
[FirstPart]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/7-FirstPart?raw=true 
[SecondPart]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/8-SecondPart?raw=true 
[ThirdPart]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/9-ThirdPart?raw=true 
[Encrypt]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/10-Encrypt?raw=true 
[Accept]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/11-Accept?raw=true 
[Account]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/12-Account?raw=true 
[Install]: https://github.com/BaconCatBug/OpenSuseWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/13-Install?raw=true 