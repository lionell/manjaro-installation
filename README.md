# Manjaro installation guide

Notes to reproduce my [Manjaro](https://manjaro.org) setup.

## Preinstall

1.  Do a FULL BACKUP.
1.  Download Manjaro i3 community edition from [here](https://manjaro.org/community-editions/).
1.  Write `.iso` image to the USB drive. Here is how to do it from Linux:
    1.  Find out the name of your USB drive with `lsblk`. If it's mounted run `umount /where/it/is/mounted`.
    1.  Run the following command, replacing `/dev/sdx` with your drive, e.g. `/dev/sdb/`. (Do **not** apppend a partition number).
        ```shell
        sudo dd bs=4M if=/path/to/image.iso of=/dev/sdx status=progress && sync
        ```
    1.  Wait for `sync` to complete before unplugging USB drive.

## Install

1.  Select US-Colemak keyboard layout during installation.
1.  Partition table should look like this:

    | size | fs    | path      |
    |------|-------|-----------|
    | 500M | FAT32 | /boot/efi |
    | 20G  | SWAP  |           |
    | 100G | EXT4  | /         |
    | 350G | EXT4  | /home     |

## Setup

### Update kernel
1.  List all available kernels `mhwd-kernel -l`
    
    **ISSUE!** If you see `error: GPGME error: No data` try running `sudo pacman-key --init`.

1.  Look for latest stable kernel and run `sudo mhwd-kernel -i linuxYY` to install.

Find more information [here](https://wiki.manjaro.org/index.php/Manjaro_Kernels).

### Update pacman mirrors

1.  To generate mirrorlist run `sudo pacman-mirrors -g`. 
1.  Sync pacman local database with new mirrorlist by running `sudo pacman -Syy`.

Find more information [here](https://wiki.manjaro.org/index.php?title=Pacman-mirrors)

### Upgrade your system

Run `sudo pacman -Suy` to upgrade all installed software.

### Remap CTRL -> ALT -> WIN -> CTRL

1.  Open `/usr/share/X11/xkb/symbols/pc` for writing.
1.  Substitute `include "altwin(alt_meta)"` with `include "altwin(ctrl_alt_win)"`.
1.  Open `~/.i3/config` for writing.
1.  Change `set $mod Mod4` to `set $mod Mod1`.
1.  Restart the system `sudo reboot`.

For more options see `/usr/share/X11/xkb/symbols/ctrl` and `/usr/share/X11/xkb/symbols/altwin`.

### Tune power usage

1.  Install TLP `sudo pacman -S tlp`.
1.  Start TLP `sudo tlp start`.
1.  Mask unnecessarry services.
    ```shell
    sudo systemctl mask systemd-rfkill.service
    sudo systemctl mask systemd-rfkill.socket
    ```
1.  Install ThinkPad specific kernel modules.
    ```shell
    sudo pacman -S linuxYY-tp_smapi
    sudo pacman -S linuxYY-acpi_call
    ```
1.  Install PowerTOP `sudo pacman -S powertop`.
1.  Calibrate PowerTOP `sudo powertop --calibrate`.
1.  Navigate to "Tunables" tab and fix all the remaining issues by pressing enter staying on it.

Find more information about [TLP](https://wiki.archlinux.org/index.php/TLP) and [PowerTOP](https://wiki.archlinux.org/index.php/powertop).

### Install Google Chrome

Run `yaourt google-chrome` and select stable chanel.

### Setup git and generate new SSH keypair

1.  Run `sudo pacman -S git` to install.
1.  Generate new SSH keypair by running `ssh-keygen -t rsa -b 4096 -C 'xlionell@gmail.com'`.
1.  Start SSH agent in background `eval $(ssh-agent -s)`.
1.  Add key to SSH agent `ssh-add ~/.ssh/id_rsa`.
1.  Install xclip `sudo pacman -S xclip`.
1.  Copy public key to clipboard `xclip -sel clip < ~/.ssh/id_rsa.pub`.
1.  Go to [github.com/settings/keys](https://github.com/settings/keys) and add new key.

### Install Apache Stow

Run `sudo pacman -S stow`.