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

### Create an update batch file for easy updating

One of the main reasons to be on Tumbleweed is to get updates constantly.  Especially when using multiple repos, updating "correctly" on Tumbleweed is not as straight forward as it might seem.  The updater in the YaST GUI __should not__ be used, and instead zypper with the "dup" command is preferred, according to [this forum post](https://forums.opensuse.org/showthread.php/528149-Updating-Tumbleweed) and [this article](https://lwn.net/Articles/717489/).  The easiest way is to create a simple batch file with these commands that you can run to update the system.  You can create it in your favorite text editor (vim!) or copy/paste these echo commands to create the file (paste this into your terminal to create the file, not into a text editor):

```
echo "sudo zypper refresh" > ~/bin/zup
echo "sudo zypper dup --no-allow-vendor-change" >> ~/bin/zup
echo "sudo zypper ps -s" >> ~/bin/zup
```

Then make the file executable:

```
chmod +x ~/bin/zup
```

You should now be able to type `zup` to perform your update.  If the zup command isn't recognized, then ~/bin is not in your %PATH% and you'll need to fix that.  If you just created the ~/bin folder, try restarting your shell or rebooting.

### Install codecs for playing multimedia: 

Follow [this post on the OpenSUSE forums](https://forums.opensuse.org/showthread.php/523476-Multimedia-Guide-for-openSUSE-Tumbleweedhttps://forums.opensuse.org/showthread.php/523476-Multimedia-Guide-for-openSUSE-Tumbleweed) for how to install multimedia support.

You should already have the Packman repo installed from earlier, and you will also need to install the libdvdcss repo:

```
sudo zypper addrepo http://opensuse-guide.org/repo/openSUSE_Tumbleweed/ libdvdcss
```

Refresh your repos, then install the needed codecs, allowing them to change repos if needed (option 1):
```
sudo zypper refresh
sudo zypper install -f libxine2-codecs ffmpeg-3  dvdauthor gstreamer-plugins-bad gstreamer-plugins-bad-orig-addon gstreamer-plugins-base  gstreamer-plugins-good gstreamer-plugins-good-extra gstreamer-plugins-libav gstreamer-plugins-qt5 gstreamer-plugins-ugly gstreamer-plugins-ugly-orig-addon vlc smplayer x264 x265 vlc-codecs vlc-codec-gstreamer ogmtools libavcodec58
```

If you feel uneasy about installing so many codecs, you can try with just these few below:
```
sudo zypper install vlc-codecs, libxine2-codecs, flash-player
```

Now, very important!  Follow the last part of the guide, where it says to open YaST > Software Management and switch system packges to the versions in the Packman repo.  YaST > Software Management > Repositories > packman > "Switch system packages" button > confirm changing to packman > Apply button.  [This picture](http://paste.opensuse.org/view//92222495) illustrates the process well.

### Random KDE Desktop Tweaks

Change task switcher (alt-tab) to look more like Windows:
    System Settings - Window Management - Task Switcher - Visualization: Choose "Thumbnails" and uncheck "Show selected window"

NumLock key enabled at startup:
    System Settings - Hardware - Input Devices - Keyboard - Hardware - NumLock on Startup - Turn on

### Install apps

Redshift - Blue-light filter (ala Flux).  The basic redshift is a command line utility, so we want to install redshift-gtk instead.

```
sudo zypper install redshift-gtk
```


Shutter - Screenshot upload tool (ala ShareX).

```
sudo zypper install shutter
```


Albert - Better launcher (ala Alfred on OSX)

```
sudo zypper install albert
```
You will need to enable features in the Albert settings before they will work (otherwise you will type and nothing will happen!).  Set your keybind for Albert, and you can re-map the KDE search keybind by going to KDE System Settings - Shortcuts - Global - System Settings - Search.

Also, if you want the Restart/Shutdown/Logout commands in Albert to work, you will need to change the commands in Settings - Extensions - System to use `qdbus-qt5` instead of just `qdbus`.
