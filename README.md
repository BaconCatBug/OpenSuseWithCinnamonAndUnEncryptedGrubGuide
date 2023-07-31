# A step-by-step guide on how to install openSUSE Tumbleweed with up-to-date Cinnamon Desktop Environment, an Unencrypted Grub Bootloader, and Encrypted Root Partition 

# Preamble

openSUSE Tumbleweed is a great distro, but is a little rough around the edges for the average user. For one thing, it encrypts the grub partition. In theory, this is good, encryption is good. In practice this serves no practical purpose, as anyone with physical access to your computer can tamper with the bootloader itself, rendering the encryption of the grub partition meaningless. Due to the fact that doing this results in 30 to 60 second decryption times before the OS even begins to boot, this is not really fit for purpose. 

Instead, what we will do is encrypt the root partition of your installation via LUKS which will prompt you for a password *after* grub has done its thing.

Another issue (which admittedly is a personal issue) is that while it's *technically* possible to install Cinnamon while in the install wizard, the official repositories are hopelessly out of date, and cause issues if you try to upgrade it via the community package after the fact. I like Cinnamon, a lot, thus this guide will show you how to install it. Feel free to skip this part if you want one of the default Gnome, KDE, or XFCE desktop environments.

Finally, we will go over the process of enabling hardware accelerated decoding for AMD GPUs that utilise the mesa driver stack. It's a lot simpler than on something like Fedora and in my personal experience a lot less likely to break.

# The Prep Work

Note: If you want to try this out via a Virtual Machine suite (I recommend gnome-boxes myself), make sure you have the VM set to UEFI mode and not BIOS mode.

Also Note: While I will be comprehensive, I will assume you know how to do some basic things, such as plugging in a USB stick, booting to said USB stick, how to use a mouse and keyboard, etc. :P

Also, Also Note: This guide assumes you have a somewhat modern hardware i.e. it has an x86_64 (aka a 64bit) CPU, supports UEFI and is in UEFI boot mode. 

1) Download the latest openSUSE Tumbleweed **Network Image** if you'll have internet access while installing (e.g. it's got an ethernet cable). Otherwise grab the **Offline Image**. At the time of writing they can be found at https://get.openSUSE.org/tumbleweed/

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

7) Click the **Desktop with Gnome** radio button. Click **Next**. (Why Gnome when we're installing Cinnamon later? Because it makes things easier, trust me. It installs a lot of basic other stuff that will help.)

![][Gnome]

8) Click the drop-down and select **Start with Existing Partitions**.

![][Existing]

9) Delete any partitions from the drive you're going to install on if they exist. Click on the drive you're going to install on, then click **Add Partition**.

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

15) Wait for it to install, then reboot and boot from the hard drive. You'll be prompted for your encryption password. 

![][Password]

# The Return of Cinnamon's Revenge: The Tweakening - Armageddon

16) Close the Welcome Window. Click the Activities button in the top left and open Firefox. Go to https://software.openSUSE.org/package/cinnamon and scroll down to the Tumbleweed header. Click **Show Community Package** and click **Expert Download** for the *home:Dead_Mozay:cinnamon* entry.

![][Software]

17) Click **Add repository and install manually** and copy the first *addrepo* command. Click Activities. Search for Terminal. Open the terminal. Type `sudo{space}` then press Control+Shift+V to paste the text into the terminal and hit enter to run it. It will prompt you for your user password (the short one, and won't show anything when typing).

![][Repo]

18) Run the command in the terminal `sudo zypper ref`, when prompted about the key, press *a* then enter to always trust the key.

![][Refresh]

19) Run the command in the terminal `sudo zypper in -n cinnamon xed blueman opi`

![][InstallCinnamon]

20) Log out back to the login screen. Click the **Power Icon** in the top right, then the **Power Icon** underneath that, then click **Log Out**, then click **Log Out** again (Gnome sure loves doubling up on everything!)

21) At the login screen, click your user name, it will prompt you for your password. In the bottom right, there is a cogwheel button. Click that, then click Cinnamon. Then put in your password, and hit enter to log in.

![][SetDE]

22) Open a terminal. Run the command `sudo zypper -n rm gnome-text-editor nautilus`

![][CleanUp]

23) Run the command `opi codecs`. Press *y* then enter for each y/n prompt. When it asks about trusting keys, again press *a* and enter.

![][OpiCodecs]

---

24) ***OPTIONAL BUT RECCOMENDED*** We will now set up a Swap file to act as a Swap partition, to act as overflow if your RAM fills up. Conventional Wisdom is to make the Swap file the same size as the amount of RAM you have, but you can set it to whatever size you want really.

25) Run the command `df / -h` and see how much space you have under the *Avail* column. For obvious reasons, don't set the swap file bigger than this.

26) Run the command `SIZE=4` where 4 is replaced by the number of gigabytes you want the Swap file to be.

27) Run each of these commands below in order, remember you can paste into the terminal with Control+Shift+V. The first `dd` command will take quite a while to complete, be patient and don't panic.

`sudo dd if=/dev/zero of=/swapfile bs=1M count=$(($SIZE * 1024))`

`sudo chmod 0600 /swapfile`

`sudo mkswap /swapfile`

`sudo sed -i '/swap/{s/^/#/}' /etc/fstab`

`sudo tee -a /etc/fstab<<<"/swapfile  none  swap  sw 0  0"`

---

28) Run the command `sudo zypper -n dup`, let it all install. Then reboot.

29) And that should be all. You can now enjoy Cinnamon on openSUSE with a more user friendly encryption scheme. My personal favourite applets are **Panel Launchers** by *panel-launchers<span>@</span>cinnamon.org*, **CobiWindowList** by *windowlist<span>@</span>cobinja.de*, **Collapsible Systray** by *collapsible-systray<span>@</span>feuerfuchs.eu*, and **Bash Sensors**.

Take the time to customise the Nemo interface, the main Menu interface, and the panels. If you wish for a Chromium based browser rather than Firefox, I recommend Vivaldi (which can be installed in the terminal by `opi vivaldi`).

# Save your Eyeballs Here

Assuming you're like me and prefer to not have your eyeballs burnt out when using your PC, you can now set the Cinnamon theme to a darker hue. Open the menu and search for *Themes*. Set each of the options to your liking, then go to the *Settings* tab at the top and set your *Dark Mode*, *Icon*, and *Scrollbar* settings to your liking.

If you are using a dark mode theme, you should also run the command 

`gsettings set org.gnome.desktop.interface color-scheme prefer-dark` 

to allow any explicitly GTK themed apps to be in dark mode.

# Remove useless Power Options

If you wish to remove the Suspend, Sleep, and Hibernate options that may or may not appear when you press the Shut Down button, you can run the following to disable these services.

`sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target`


[Installation]:		https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/1-Installation.png?raw=true 
[Language]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/2-Language.png?raw=true 
[SayNo]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/3-SayNo.png?raw=true 
[Gnome]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/4-Gnome.png?raw=true 
[Existing]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/5-Existing.png?raw=true 
[EmptyDrive]:		https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/6-EmptyDrive.png?raw=true 
[FirstPart]:		https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/7-FirstPart.png?raw=true 
[SecondPart]:		https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/8-SecondPart.png?raw=true 
[ThirdPart]:		https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/9-ThirdPart.png?raw=true 
[Encrypt]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/10-Encrypt.png?raw=true 
[Accept]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/11-Accept.png?raw=true 
[Account]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/12-Account.png?raw=true 
[Install]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/13-Install.png?raw=true 
[Password]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/14-Password.png?raw=true 
[Software]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/15-Software.png?raw=true 
[Repo]:				https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/16-Repo.png?raw=true 
[Refresh]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/17-Refresh.png?raw=true 
[InstallCinnamon]:	https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/18-InstallCinnamon.png?raw=true 
[SetDE]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/19-SetDE.png?raw=true
[CleanUp]:			https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/20-CleanUp.png?raw=true
[OpiCodecs]:		https://github.com/BaconCatBug/openSUSEWithCinnamonAndUnEncryptedGrubGuide/blob/main/Images/21-OpiCodecs.png?raw=true
