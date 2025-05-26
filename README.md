markdown
# � Arch Linux Plymouth Splash Screen (systemd-boot + LUKS)

Add a graphical Plymouth splash screen to Arch Linux for a polished boot experience. This guide covers setup for systems using:

- systemd-boot
- LUKS full-disk encryption
- mkinitcpio

Replaces text output with a graphical screen during startup and password prompts.

## 📥 Installation

### 1. Install Plymouth
```bash
sudo pacman -S plymouth
```

### 2. Configure mkinitcpio
Edit the configuration file:
```bash
sudo nano /etc/mkinitcpio.conf
```

Modify the `HOOKS` line to include `plymouth`:
```diff
- HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block encrypt filesystems fsck)
+ HOOKS=(base udev plymouth autodetect microcode modconf kms keyboard keymap consolefont block encrypt filesystems fsck)
```

### 3. Rebuild Initramfs
```bash
sudo mkinitcpio -P
```

### 4. Configure systemd-boot
Edit your boot entry:
```bash
sudo nano /boot/loader/entries/arch.conf
```

Append these parameters to the `options` line:
```
splash quiet loglevel=3
```

Example:
```
options cryptdevice=PARTUUID=xxxxxxxxx:root root=/dev/mapper/root zswap.enabled=0 rw rootfstype=ext4 splash quiet loglevel=3
```

## 🎨 Themes

### Install a Plymouth Theme

**Option A: From AUR (recommended)**
```bash
yay -S plymouth-themes
```
or search for specific themes:
```bash
yay -Ss plymouth-theme
```

**Option B: Manual Download**  
Get themes from GitHub (e.g., [adi1090x/plymouth-themes](https://github.com/adi1090x/plymouth-themes)) and follow their installation instructions.

### Apply the Theme
```bash
sudo plymouth-set-default-theme -R themename
```
Replace `themename` with your chosen theme. List available themes with:
```bash
plymouth-set-default-theme --list
```

## 🚀 Final Steps

### Reboot your system
```bash
sudo reboot
```

### Optional: Hide Bootloader Menu
Edit `/boot/loader/loader.conf` for a seamless boot:
```ini
default arch
timeout 0
editor no
console-mode max
```

## ✅ Done!
Enjoy your new graphical boot experience with Plymouth!
