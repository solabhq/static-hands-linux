# Static Hands Advanced for Linux

**Navigate, edit, and transform text without moving your hands.**

---

## Quick Install

```bash
chmod +x install_advanced.sh
./install_advanced.sh
```

Then **log out and log back in** (or reboot).

---

## What's Included

### Navigation (11 shortcuts)

- Caps + IJKL = Arrow keys
- Caps + UO = Home/End
- Caps + YN = Page Up/Down
- Caps + H = Delete
- Caps + ; = Backspace
- Caps + P = Insert

### Modifiers (4 shortcuts - hold)

- Caps + F = Ctrl
- Caps + D = Shift
- Caps + S = Alt
- Caps + W = Super/Win

**Examples:**
- Caps+F+L = Ctrl+Right (jump word)
- Caps+D+L = Shift+Right (select)
- Caps+F+D+L = Ctrl+Shift+Right (select word)

### Text Editing (6 shortcuts)

- Caps + A = Select All
- Caps + Z = Undo
- Caps + X = Cut
- Caps + C = Copy
- Caps + V = Paste
- Caps + B = Bold

### Speed Navigation (4 shortcuts)

- Caps + 8 = Up ×5 lines
- Caps + , = Down ×5 lines
- Caps + M = Left ×6 words
- Caps + . = Right ×6 words

### Special Keys (5 shortcuts)

- Caps + Q = Escape
- Caps + R = Rename (F2)
- Caps + T = Select line
- Caps + 7 = Tab
- Caps + \` = Close window
- Caps + F7 = Enter

---

## System Requirements

- Linux (any distribution with systemd)
- 5 MB disk space
- X11 or Wayland
- No additional dependencies

---

## What Doesn't Work

### Not Implemented (9 features)

- **Case change (Caps+9/0/-)** - Requires recompiling Kanata with cmd flag
- **Always On Top (Caps+=)** - Configure in your window manager instead
- **Help Window (Caps+F10)** - Use CHEATSHEET_ADVANCED.md
- **Google Search (Alt+G)** - Use browser extensions

See COMPARISON_ADVANCED.md for full details and workarounds.

---

## Files

- **config_advanced.kbd** - Kanata configuration
- **install_advanced.sh** - Auto-installer
- **INSTALL_ADVANCED.md** - Detailed setup guide
- **COMPARISON_ADVANCED.md** - Full feature comparison
- **CHEATSHEET_ADVANCED.md** - Printable reference

---

## Manual Installation

If the auto-installer doesn't work:

1. Install Kanata from https://github.com/jtroo/kanata
2. Copy config_advanced.kbd to `~/.config/kanata/config.kbd`
3. Follow INSTALL_ADVANCED.md for service setup

---

## Troubleshooting

**Kanata won't start:**
```bash
journalctl --user -u kanata.service -n 50
```

**Permission errors:**
```bash
sudo chmod 0666 /dev/uinput
systemctl --user restart kanata.service
```

**Need more help?** See INSTALL_ADVANCED.md

---

## Compatibility

- ✅ Tested: Fedora 43, Ubuntu 22.04+, Arch, Debian 12+
- ✅ DEs: GNOME, KDE, XFCE, i3, sway
- ✅ Features: 30/39 from Windows version (76.9%)

---

## License

Free for personal and commercial use.

---

**Questions?** Check COMPARISON_ADVANCED.md for detailed feature breakdown.

