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

## After Install

* You will probably need to comment out the entry for Chrome in `/etc/apt/sources.list.d/google.list` to avoid a conflict with `/etc/apt/sources.list.d/google-chrome.list`
   - `sed -e '/chrome/ s/^#*/# /' -i /etc/apt/sources.list.d/google.list`
