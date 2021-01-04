# Ubuntu Migration Checklist

Checklist and guide on how to migrate to a clean install of Ubuntu

## Backups before Install

* Download the latest LTS release ISO of Ubuntu and make a boot thumbdrive
* Backup entire computer on external harddrive
   - This is for an emergency restore, not a transfer
   - Manually copy all files desired to a segment of the external hard drive (use EXT4 formatting)
   - Use `rsync --archive <target> <destination>` for this to preserve permissions
* Make a list of all software to reinstall
   - Only restore files, not applications
* Backup all fonts
   - Under `/usr/share/fonts`
* Make a backup of SessionBuddy
* Make a list of all Docker images to pull again
   - `docker images &> docker_images_manifest.txt`
* Backup SSH keys (optional)

All dotfiles should be restored from [personal dotfiles repo](https://github.com/matthewfeickert/dotfiles), so don't need to be backed up

## Dry Run

Boot into the new OS from the thumb drive with the backup hard drive connected and verify that all files can be read

## Installation Process

* Intall Ubuntu **without** recieving updates over the internet to avoid installation errors

## After Install

### System Stability Checks

* Enable internet
* Run
```
sudo apt-get update -y
sudo apt-get upgrade -y
```
to update the default installed software
* Searching the system for "Additional Drivers" open up Software & Updates and in the Additional Drivers tab update to the stable NVIDIA driver metapackage available
   - For Ubuntu 20.04 this was `nvidia-driver-455` in January 2021 (has a note next to it of "(proprietary, tested)")
* After updating the Nvidia driver install the Nvidia CUDA toolkit (version automatically determined from install Nvidia drivers) with
```
sudo apt-get update -y
sudo apt-get install nvidia-cuda-toolkit
```
* Restart the computer
* Go into Ubuntu Software and install all software in the Updates tab including all firmware updates (which will all require system restarts)
* Verify the Nvidia X Server Settings for the graphics card and dispaly settings and save the X Configuration File out
   - Saved under `/etc/X11/xorg.conf`
* For security reasons, avoid all SSH keys being automatically added to the default ssh-agent at system login by [removing the `gpg-keyring-daemon` from the startup applications](https://unix.stackexchange.com/questions/527693/ssh-keys-cant-be-deleted-from-agent).
To do this search the system for "Startup Applications" and then uncheck the box next to the "GNOME Keyring: SSH Agent" entry.
You can then manually add ssh-keys to the agent like normal.

### Software Reinstallation

* Reinstall all software
* You will probably need to comment out the PPA entry for Chrome in `/etc/apt/sources.list.d/google.list` to avoid a conflict with `/etc/apt/sources.list.d/google-chrome.list`
   - `sed -e '/chrome/ s/^#*/# /' -i /etc/apt/sources.list.d/google.list`
