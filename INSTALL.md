# Static Hands for Linux - Installation Guide

**Keep your hands on the home row. Navigate without arrow keys.**

A keyboard productivity system using CapsLock as a modifier key - Linux port of the Windows AutoHotkey version.

---

## What You Get

Static Hands gives you full navigation control without moving your hands from the home row:

### Navigation Keys

| Keys | Action | Keys | Action |
|------|--------|------|--------|
| `Caps + I` | ↑ Up | `Caps + U` | Home |
| `Caps + J` | ← Left | `Caps + O` | End |
| `Caps + K` | ↓ Down | `Caps + H` | Delete |
| `Caps + L` | → Right | `Caps + ;` | Backspace |

### Special Keys

| Keys | Action | Keys | Action |
|------|--------|------|--------|
| `Caps + Y` | Page Up | `Caps + P` | Insert |
| `Caps + N` | Page Down | | |

### Modifiers (Hold Both Keys)

| Keys | Modifier | Example | Result |
|------|----------|---------|--------|
| `Caps + F` | Ctrl | `Caps+F+L` | Jump word right |
| `Caps + D` | Shift | `Caps+D+L` | Select right |
| `Caps + S` | Alt | `Caps+S+I` | Alt+Up |
| `Caps + W` | Super | `Caps+W+L` | Super+Right |

### Speed Navigation

| Keys | Action | Keys | Action |
|------|--------|------|--------|
| `Caps + 8` | ↑×5 (fast) | `Caps + M` | ←×6 (fast) |
| `Caps + ,` | ↓×5 (fast) | `Caps + .` | →×6 (fast) |

**Total: 19 working keybindings**

---

## System Requirements

- Linux kernel 4.18+
- systemd
- wget (will be installed automatically)
- 10 MB disk space
- Works on: Fedora, Ubuntu, Debian, Arch, Pop!_OS
- Compatible with: X11 and Wayland

---

## Installation Method 1: Automatic (Recommended)

**Fastest way - 5 minutes total:**

1. Extract the downloaded files
2. Open terminal in the extracted folder
3. Run the installer:

```bash
bash install.sh
```

4. When prompted, reboot your computer
5. After reboot, enable the service:

```bash
systemctl --user enable --now kanata.service
```

**Done!** Static Hands is now active and will start automatically on every boot.

Test it: Open any text editor and hold CapsLock while pressing I/J/K/L

---

## Installation Method 2: Manual (Step by Step)

If the automatic installer doesn't work, or you prefer manual setup:

### Step 1: Install Dependencies

```bash
# Debian/Ubuntu
sudo apt update && sudo apt install -y wget

# Fedora/RHEL
sudo dnf install -y wget

# Arch Linux
sudo pacman -Sy --noconfirm wget
```

---

### Step 2: Install Kanata

```bash
cd ~/Downloads
wget https://github.com/jtroo/kanata/releases/download/v1.7.0/kanata
chmod +x kanata
sudo mv kanata /usr/local/bin/
```

Verify: `kanata --version` should show `kanata 1.7.0`

---

### Step 3: Configure Permissions

```bash
# Create group and add yourself
sudo groupadd -f uinput
sudo usermod -aG input $USER
sudo usermod -aG uinput $USER

# Load kernel module
sudo modprobe uinput
```

**Distribution-specific udev rules:**

**For Fedora/RHEL:**
```bash
echo 'KERNEL=="uinput", MODE="0666", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-uinput.rules
```

**For Ubuntu/Debian/Arch:**
```bash
echo 'KERNEL=="uinput", GROUP="uinput", MODE="0660", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-uinput.rules
```

**Then for all distributions:**
```bash
# Auto-load module on boot
echo 'uinput' | sudo tee /etc/modules-load.d/uinput.conf

# Apply changes
sudo udevadm control --reload-rules
sudo udevadm trigger
sudo chmod 0666 /dev/uinput
```

Verify: `ls -l /dev/uinput` should show `crw-rw-rw-`

---

### Step 4: Disable CapsLock System Behavior

**For GNOME:**
```bash
gsettings set org.gnome.desktop.input-sources xkb-options "['caps:none']"
```

**For KDE:**
```bash
kwriteconfig5 --file kxkbrc --group Layout --key Options caps:none
qdbus org.kde.keyboard /Layouts org.kde.KeyboardLayouts.reconfigure
```

**For other desktops:**
```bash
setxkbmap -option caps:none
```

---

### Step 5: Install Configuration

```bash
# Create directory
mkdir -p ~/.config/kanata

# Copy the config file
cp config.kbd ~/.config/kanata/config.kbd
```

---

### Step 6: Create System Service

```bash
# Create directory
mkdir -p ~/.config/systemd/user/

# Create service file
nano ~/.config/systemd/user/kanata.service
```

**Paste this content:**
```ini
[Unit]
Description=Kanata keyboard remapper
After=graphical-session.target

[Service]
Type=simple
ExecStart=/usr/local/bin/kanata -c %h/.config/kanata/config.kbd
Restart=always

[Install]
WantedBy=default.target
```

Save and exit: `Ctrl+O`, `Enter`, `Ctrl+X`

---

### Step 7: Reboot

**IMPORTANT:** You must reboot for group changes to take effect.

```bash
reboot
```

---

### Step 8: Start Kanata (After Reboot)

```bash
# Verify groups
groups
# Should include: input uinput

# Verify permissions
ls -l /dev/uinput
# Should show: crw-rw-rw-

# Enable and start service
systemctl --user daemon-reload
systemctl --user enable kanata.service
systemctl --user start kanata.service

# Check status
systemctl --user status kanata.service
# Should show: Active: active (running)
```

**Done!** Test in any text editor.

---

## Testing

Open a text editor (gedit, kate, gnome-text-editor) and test:

1. Hold CapsLock and press I → cursor moves up
2. Hold CapsLock and press J → cursor moves left
3. Hold CapsLock and press K → cursor moves down
4. Hold CapsLock and press L → cursor moves right
5. Hold CapsLock and press F, then press L → jumps word right (Ctrl+Right)
6. Hold CapsLock and press D, then press L → selects right (Shift+Right)

If all works - congratulations! 🎉

---

## Troubleshooting

### Problem 1: "Permission denied (error 13)"

**Symptom:** Service fails to start with permission error

**Solution:**
```bash
# Check permissions
ls -l /dev/uinput
# Should be: crw-rw-rw-

# If wrong, fix it:
sudo chmod 0666 /dev/uinput

# Check groups
groups
# Should include: input uinput

# If missing groups, you didn't reboot!
reboot
```

**After reboot:**
```bash
systemctl --user restart kanata.service
```

---

### Problem 2: CapsLock Still Works as Caps Lock

**Symptom:** CapsLock toggles CAPS instead of acting as modifier

**Solution:**

**For GNOME:**
```bash
gsettings set org.gnome.desktop.input-sources xkb-options "['caps:none']"
systemctl --user restart kanata.service
```

**For KDE:**
```bash
kwriteconfig5 --file kxkbrc --group Layout --key Options caps:none
qdbus org.kde.keyboard /Layouts org.kde.KeyboardLayouts.reconfigure
systemctl --user restart kanata.service
```

**For other desktops:**
```bash
setxkbmap -option caps:none
systemctl --user restart kanata.service
```

---

### Problem 3: Service Won't Start

**Check the logs:**
```bash
journalctl --user -u kanata.service -n 50
```

Look for error messages. Common issues:
- Config file missing: Copy `config.kbd` to `~/.config/kanata/config.kbd`
- Binary missing: Install Kanata (Step 2)
- Permissions: See Problem 1
- wget missing: Install with your package manager

---

### Problem 4: Stops Working After Sleep/Suspend

**Symptom:** Kanata doesn't work after waking computer

**Solution:**
```bash
systemctl --user restart kanata.service
```

The service has `Restart=always` configured, so it should restart automatically, but you can force it manually if needed.

---

### Problem 5: SELinux Blocking (Fedora/RHEL)

**Symptom:** Permission denied despite correct permissions

**Temporary solution:**
```bash
sudo setenforce 0
systemctl --user restart kanata.service
```

**Note:** This disables SELinux temporarily. It will re-enable after reboot.

---

### Problem 6: Works Manually but Not as Service

**Symptom:** `sudo kanata -c ~/.config/kanata/config.kbd` works, but service doesn't

**Solution:**

1. Check if you rebooted after adding groups:
```bash
groups
# Must include: input uinput
```

2. Check /dev/uinput permissions:
```bash
ls -l /dev/uinput
# Must be: crw-rw-rw- (0666)
```

3. If still failing, check service logs:
```bash
journalctl --user -u kanata.service -n 50
```

---

### Problem 7: Wrong udev Rule for Your Distribution

**Symptom:** Permissions keep resetting after reboot

**Fedora/RHEL specific fix:**
```bash
# Remove old rule
sudo rm /etc/udev/rules.d/99-uinput.rules

# Create Fedora-specific rule
echo 'KERNEL=="uinput", MODE="0666", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-uinput.rules

# Apply
sudo udevadm control --reload-rules
sudo udevadm trigger
sudo chmod 0666 /dev/uinput
reboot
```

**Ubuntu/Debian/Arch specific fix:**
```bash
# Remove old rule
sudo rm /etc/udev/rules.d/99-uinput.rules

# Create proper rule with GROUP
echo 'KERNEL=="uinput", GROUP="uinput", MODE="0660", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-uinput.rules

# Apply
sudo udevadm control --reload-rules
sudo udevadm trigger
reboot
```

---

## Useful Commands

**Service management:**
```bash
systemctl --user start kanata.service      # Start
systemctl --user stop kanata.service       # Stop
systemctl --user restart kanata.service    # Restart
systemctl --user status kanata.service     # Check status
```

**View logs:**
```bash
journalctl --user -u kanata.service -n 50    # Last 50 lines
journalctl --user -u kanata.service -f       # Live logs
```

**Check configuration:**
```bash
cat ~/.config/kanata/config.kbd              # View config
nano ~/.config/kanata/config.kbd             # Edit config
```

**After editing config:**
```bash
systemctl --user restart kanata.service      # Apply changes
```

---

## Uninstallation

If you want to remove Static Hands:

```bash
# Stop and disable service
systemctl --user stop kanata.service
systemctl --user disable kanata.service

# Remove files
rm -rf ~/.config/kanata
rm ~/.config/systemd/user/kanata.service
sudo rm /usr/local/bin/kanata
sudo rm /etc/udev/rules.d/99-uinput.rules
sudo rm /etc/modules-load.d/uinput.conf

# Remove from groups
sudo gpasswd -d $USER uinput
sudo gpasswd -d $USER input

# Restore CapsLock (GNOME)
gsettings reset org.gnome.desktop.input-sources xkb-options

# Reboot
reboot
```

---

## Distribution-Specific Notes

### Fedora/RHEL
- ✅ Use `MODE="0666"` in udev rule (installer does this automatically)
- ⚠️ SELinux may need temporary disabling: `sudo setenforce 0`
- ✅ Installer detects Fedora and applies correct settings

### Ubuntu/Debian
- ✅ Use `MODE="0660"` + `GROUP="uinput"` (installer does this automatically)
- ✅ Install wget if missing: `sudo apt install wget`
- ✅ Works perfectly with default settings

### Arch Linux
- ✅ Use `MODE="0660"` + `GROUP="uinput"` (installer handles this)
- ✅ Consider AUR package: `yay -S kanata-bin`
- ✅ Or follow manual/automatic installation above

---

## Quick Reference

**Most common actions:**

| Task | Command |
|------|---------|
| Start Kanata | `systemctl --user start kanata.service` |
| Stop Kanata | `systemctl --user stop kanata.service` |
| Restart Kanata | `systemctl --user restart kanata.service` |
| Check if running | `systemctl --user status kanata.service` |
| View errors | `journalctl --user -u kanata.service -n 50` |
| Fix permissions | `sudo chmod 0666 /dev/uinput` |

**If nothing works:**
1. Reboot
2. Check: `ls -l /dev/uinput` → should be `crw-rw-rw-`
3. Check: `groups` → should include `input uinput`
4. Run: `systemctl --user restart kanata.service`

---

## Support

**Still having issues?**

1. Check troubleshooting section above
2. Verify you completed ALL steps (especially reboot!)
3. Check logs: `journalctl --user -u kanata.service -n 50`
4. Verify your distribution-specific udev rule is correct

**For more information:**
- Kanata documentation: https://github.com/jtroo/kanata
- Kanata config guide: https://github.com/jtroo/kanata/blob/main/docs/config.adoc

---

## Learning Tips

**First week:** 
- Practice basic arrows (IJKL)
- Add two modifiers (F for Ctrl and D for Shift)
- Add Home and End (U/O)
- Add Backspace (;)
- Open a text editor and just navigate around
- Force yourself not to use arrow keys
- Practice Caps+F to jump words, Caps+D to select whole words
- 10-15 minutes daily practice

**Second week:** 
- Add modifiers (S/W for Alt/Super)
- Speed navigation (8/,/M/.)
- Delete (H)
- PageUp/PageDown (Y/N)
- Use Caps+S with arrows for text selection
- Use Caps+8 to jump up 5 lines quickly
- Use Caps+, to jump down 5 lines

**Third week:** 
- Add everything else
- Practice combinations

**Full muscle memory:** 3-4 weeks of daily use

**Pro tip:** Use the keybinding wallpaper and keep it visible for the first week!

---

**Thank you for using Static Hands!** 🚀

Your productivity is about to level up. Give it 3-4 weeks and you'll never want to reach for arrow keys again.
