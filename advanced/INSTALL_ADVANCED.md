# Static Hands Advanced - Installation Guide

Complete setup instructions for 30 keyboard shortcuts.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Automatic Installation](#automatic-installation)
3. [Manual Installation](#manual-installation)
4. [Post-Installation](#post-installation)
5. [Upgrading from Basic](#upgrading-from-basic-to-advanced)
6. [Troubleshooting](#troubleshooting)
7. [Uninstallation](#uninstallation)

---

## Prerequisites

### System Requirements
- Linux distribution with systemd
- X11 or Wayland display server
- 5 MB free disk space
- Internet connection (for downloads)

### Supported Distributions
- ✅ Fedora 40+
- ✅ Ubuntu 22.04+
- ✅ Debian 12+
- ✅ Arch Linux
- ✅ Manjaro
- ✅ Pop!_OS
- ✅ Linux Mint

---

## Automatic Installation

### Step 1: Download Package

Extract the archive:
```bash
tar -xzf Static_Hands_Advanced_Linux.tar.gz
cd static-hands-advanced
```

### Step 2: Run Installer

```bash
chmod +x install_advanced.sh
./install_advanced.sh
```

The installer will:
1. Install wget dependency
2. Download Kanata binary
3. Set up configuration files
4. Disable CapsLock system behavior
5. Configure permissions (uinput group)
6. Create systemd service
7. Start Kanata

**Installation time:** 3 minutes

### Step 3: Log Out and Log Back In

**IMPORTANT:** Group permissions only apply after logout/login.

```bash
# Option 1: Log out (recommended)
# Close all windows and log out

# Option 2: Reboot (if unsure)
reboot
```

### Step 4: Verify Installation

After logging back in:
```bash
systemctl --user status kanata.service
```

You should see: `Active: active (running)`

Test by pressing `Caps+I` - your cursor should move up.

---

## Manual Installation

If the auto-installer fails, follow these steps:

### Step 1: Install Dependencies

**Debian/Ubuntu:**
```bash
sudo apt update
sudo apt install -y wget
```

**Fedora:**
```bash
sudo dnf install -y wget
```

**Arch:**
```bash
sudo pacman -Sy --noconfirm wget
```

### Step 2: Download and Install Kanata

```bash
cd ~/Downloads
wget https://github.com/jtroo/kanata/releases/latest/download/kanata
chmod +x kanata
sudo mv kanata /usr/local/bin/
```

Verify:
```bash
/usr/local/bin/kanata --version
```

### Step 3: Set Up Configuration

```bash
mkdir -p ~/.config/kanata
cp config_advanced.kbd ~/.config/kanata/config.kbd
```

### Step 4: Disable CapsLock

**For GNOME:**
```bash
gsettings set org.gnome.desktop.input-sources xkb-options "['caps:none']"
```

**For KDE:**
```bash
kwriteconfig5 --file kxkbrc --group Layout --key Options caps:none
qdbus org.kde.keyboard /Layouts org.kde.KeyboardLayouts.reconfigure
```

**For Other DEs:**
```bash
setxkbmap -option caps:none
```

### Step 5: Configure Permissions

Create uinput group and add user:
```bash
sudo groupadd -f uinput
sudo usermod -aG input,uinput $USER
```

Create udev rule:

**For Fedora/RHEL:**
```bash
echo 'KERNEL=="uinput", MODE="0666", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-uinput.rules
```

**For Other Distros:**
```bash
echo 'KERNEL=="uinput", GROUP="uinput", MODE="0660", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-uinput.rules
```

Apply rules:
```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

### Step 6: Create Systemd Service

```bash
mkdir -p ~/.config/systemd/user

cat > ~/.config/systemd/user/kanata.service << 'EOF'
[Unit]
Description=Kanata keyboard remapper
After=graphical-session.target

[Service]
Type=simple
ExecStart=/usr/local/bin/kanata -c %h/.config/kanata/config.kbd
Restart=always

[Install]
WantedBy=default.target
EOF

systemctl --user daemon-reload
systemctl --user enable kanata.service
```

### Step 7: Start Service

**On Fedora (SELinux):**
```bash
sudo setenforce 0
systemctl --user start kanata.service
```

**On Other Distros:**
```bash
systemctl --user start kanata.service
```

### Step 8: Log Out and Log Back In

Required for group permissions to apply.

---

## Post-Installation

### Verify Everything Works

Test basic navigation:
```bash
# Open a text editor
# Press Caps+I/J/K/L to move cursor
```

Test speed navigation:
```bash
# In a long document
# Press Caps+8 - should jump up 5 lines
```

### Check Service Status

```bash
systemctl --user status kanata.service
```

Should show:
- `Active: active (running)`
- No errors in recent logs

### Monitor Live Logs

```bash
journalctl --user -u kanata.service -f
```

Press Ctrl+C to exit.

---

## Upgrading from Basic to Advanced

If you already have the Basic version (19 shortcuts) installed:

```bash
# 1. Stop the current service
systemctl --user stop kanata.service

# 2. Backup your current config (optional)
cp ~/.config/kanata/config.kbd ~/.config/kanata/config_basic_backup.kbd

# 3. Copy new config
cp config_advanced.kbd ~/.config/kanata/config.kbd

# 4. Restart service
systemctl --user start kanata.service
systemctl --user status kanata.service
```

**What changes:**
- Config file: 19 → 30 shortcuts
- New features: Speed navigation, more special keys
- Service: same, just restarted

**To revert to Basic:**
```bash
systemctl --user stop kanata.service
cp ~/.config/kanata/config_basic_backup.kbd ~/.config/kanata/config.kbd
systemctl --user start kanata.service
```

---

## Troubleshooting

### Permission Denied Error

**Symptom:**
```
Failed to open the output uinput device
Permission denied (os error 13)
```

**Fix for Fedora:**
```bash
sudo chmod 0666 /dev/uinput
ls -l /dev/uinput  # Should show: crw-rw-rw-
systemctl --user restart kanata.service
```

**Fix for Other Distros:**
```bash
# Verify you're in uinput group
groups  # Should include: uinput

# If not, reboot and check again
reboot
```

### CapsLock Still Activating

**Symptom:** Pressing CapsLock turns on caps lock LED.

**Fix:**
```bash
# For GNOME
gsettings set org.gnome.desktop.input-sources xkb-options "['caps:none']"

# For KDE
kwriteconfig5 --file kxkbrc --group Layout --key Options caps:none
qdbus org.kde.keyboard /Layouts org.kde.KeyboardLayouts.reconfigure

# For others
setxkbmap -option caps:none
```

Then restart Kanata:
```bash
systemctl --user restart kanata.service
```

### Service Won't Start

**Get detailed error:**
```bash
journalctl --user -u kanata.service -n 50 --no-pager
```

**Common issues:**

1. **Config file not found:**
```bash
ls -l ~/.config/kanata/config.kbd
# If not found, copy from package
```

2. **Kanata binary missing:**
```bash
which kanata
# Should show: /usr/local/bin/kanata
```

3. **SELinux blocking (Fedora):**
```bash
sudo setenforce 0
systemctl --user restart kanata.service
```

### Kanata Not Starting on Boot

**Enable service:**
```bash
systemctl --user enable kanata.service
```

**Check enabled status:**
```bash
systemctl --user is-enabled kanata.service
# Should return: enabled
```

### Speed Navigation Too Fast/Slow

Edit repeat counts in config:
```bash
nano ~/.config/kanata/config.kbd
```

Find these lines:
```lisp
up5   (macro up up up up up)        ;; Change to 3 or 7
down5 (macro down down down down down)
wlft6 (macro C-left C-left ...)      ;; Change count
```

Reload:
```bash
systemctl --user restart kanata.service
```

### Works After Manual Start, Not After Reboot

**Issue:** Permissions not persisting.

**Fix udev rules:**
```bash
# Fedora
echo 'KERNEL=="uinput", MODE="0666", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-uinput.rules

# Other distros  
echo 'KERNEL=="uinput", GROUP="uinput", MODE="0660", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-uinput.rules

sudo udevadm control --reload-rules
sudo udevadm trigger
reboot
```

---

## Uninstallation

### Complete Removal

Remove everything related to Static Hands:

```bash
# 1. Stop and disable service
systemctl --user stop kanata.service
systemctl --user disable kanata.service

# 2. Remove service file
rm ~/.config/systemd/user/kanata.service
systemctl --user daemon-reload

# 3. Remove Kanata binary
sudo rm /usr/local/bin/kanata

# 4. Remove all config files
rm -rf ~/.config/kanata

# 5. Remove udev rules
sudo rm /etc/udev/rules.d/99-uinput.rules
sudo udevadm control --reload-rules
sudo udevadm trigger

# 6. Re-enable CapsLock (optional)
# GNOME:
gsettings reset org.gnome.desktop.input-sources xkb-options

# KDE:
kwriteconfig5 --file kxkbrc --group Layout --key Options --delete
qdbus org.kde.keyboard /Layouts org.kde.KeyboardLayouts.reconfigure

# Others:
setxkbmap -option

# 7. Remove from group (optional)
sudo gpasswd -d $USER uinput
sudo gpasswd -d $USER input
```

### Temporary Disable (Keep Config)

If you want to disable Kanata but keep the configuration:

```bash
# Stop service
systemctl --user stop kanata.service
systemctl --user disable kanata.service
```

To re-enable later:
```bash
systemctl --user enable kanata.service
systemctl --user start kanata.service
```

### Remove Service Only (Keep Kanata Binary)

If you want to remove the service but keep Kanata for manual use:

```bash
systemctl --user stop kanata.service
systemctl --user disable kanata.service
rm ~/.config/systemd/user/kanata.service
systemctl --user daemon-reload

# Config stays in ~/.config/kanata/
# Binary stays in /usr/local/bin/kanata
```

### Downgrade to Basic Version

```bash
# Stop service
systemctl --user stop kanata.service

# Replace config with Basic version
# (You need to download Basic package first)
cp /path/to/basic/config.kbd ~/.config/kanata/config.kbd

# Restart
systemctl --user start kanata.service
```

---

## Advanced Configuration

### Change Repeat Speed

Edit `~/.config/kanata/config.kbd`:
```lisp
;; Find these lines and adjust counts
up5   (macro up up up)           ;; Was: up ×5, now ×3
down5 (macro down down)           ;; Was: down ×5, now ×2
```

### Add Custom Shortcuts

Study the config structure in `config_advanced.kbd` and add your own:
```lisp
(defalias
  mycmd (macro C-S-t)  ;; Example: Ctrl+Shift+T
)

;; Then in deflayer capslock:
  @mycmd  ;; Assign to a key
```

---

## Enabling Case Change (Optional)

Case change (Caps+9/0/-) requires recompiling Kanata with cmd flag:

```bash
# Install Rust toolchain
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env

# Clone and build Kanata
git clone https://github.com/jtroo/kanata
cd kanata
cargo build --release --features cmd

# Replace binary
sudo cp target/release/kanata /usr/local/bin/

# Install case-change scripts
# (Download from Advanced package with cmd support)
cp case-*.sh ~/.config/kanata/
chmod +x ~/.config/kanata/case-*.sh

# Update config to enable case change
# (Use config_advanced_with_cmd.kbd if provided)

# Restart service
systemctl --user restart kanata.service
```

---

## Next Steps

1. **Print cheatsheet:** CHEATSHEET_ADVANCED.md
2. **Read comparison:** COMPARISON_ADVANCED.md
3. **Start practicing:** 10-15 minutes daily for 3 weeks

---

**Support:** See README_ADVANCED.md for questions

