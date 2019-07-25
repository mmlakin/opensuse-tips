# opensuse-tips

Tips, tricks, and apps for getting started on OpenSUSE Linux after coming from Windows (at home) and Mac OSX (at work).

### Add external package repositories 

Extend the number of apps you can install via zypper by adding the Packman repo.  Follow [these instructions](https://en.opensuse.org/Additional_package_repositories), and make sure to choose Tumbleweed.  Here are the commands to add the Packman repo for Tumbleweed, and to switch your system packages to those in Packman, as shown in the article:

```
sudo zypper addrepo -cfp 90 http://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/ packman
sudo zypper dup --from packman --allow-vendor-change
```

### Install Nvidia drivers

Use [these instructions](https://en.opensuse.org/SDB:NVIDIA_drivers) to install the official Nvidia drivers for your video card.  The instructions show you how to identify the model of your card, so you can pick the right driver.  As of 2019 there are only two options; so if you have a GeForce 600 series or newer, use the G05 driver.  Here's the commands I used to install the driver for my GeForce 1070 Ti:

```
sudo zypper addrepo --refresh https://download.nvidia.com/opensuse/tumbleweed NVIDIA
sudo zypper install x11-video-nvidiaG05
```

### Mount your existing file systems in /mnt

Dolphin file manager will easily mount your external storage devices for you, but you'll probably want to use a more memorable mount point than the default.  To do this, you will first need to find the UUID of your device, and then add an entry to your /etc/fstab file so it auto-mounts at system startup.  Follow steps 1 and 2 from [these instructions](https://en.opensuse.org/SDB:Mount_additional_disk), but use 0 0 instead of 1 2 in the fstab entry to match your other.  My fstab entry for reference:

```
UUID=2096678B96675FF0                      /mnt/storage            ntfs   defaults                      0  0
```

Your drive will be mounted at startup from now on, and to mount it immediately (without rebooting) use this command:

```
sudo mount -av
```

### Install codecs for playing videos & flash

Use zypper to install vlc-codecs, libxine2-codecs, and flash-player.  These require the packman repository, and when installing vlc-codecs, zypper might ask you about changing vendor for a few dependencies, to which you can answer the first option (1) for each. 

```
sudo zypper install vlc-codecs, libxine2-codecs, flash-player
```

### Install apps

Redshift - Blue-light filter (ala Flux).  The basic redshift is a command line utility, so we want to install redshift-gtk instead.

```
sudo zypper install redshift-gtk
```

Shutter - Screenshot upload tool (ala ShareX).

```
sudo zypper install shutter
```

