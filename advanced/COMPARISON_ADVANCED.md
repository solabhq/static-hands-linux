# Static Hands Advanced - Complete Feature Comparison

## Windows (AHK) vs Linux (Kanata)

**Compatibility: 30 out of 39 features (76.9%)**

---

## ✅ FULLY IMPLEMENTED (30 features)

### Navigation (8 features)

| Shortcut | Function | Windows | Linux | Notes |
|----------|----------|---------|-------|-------|
| Caps+I | Up arrow | ✅ | ✅ | Perfect |
| Caps+J | Left arrow | ✅ | ✅ | Perfect |
| Caps+K | Down arrow | ✅ | ✅ | Perfect |
| Caps+L | Right arrow | ✅ | ✅ | Perfect |
| Caps+U | Home | ✅ | ✅ | Perfect |
| Caps+O | End | ✅ | ✅ | Perfect |
| Caps+Y | Page Up | ✅ | ✅ | Perfect |
| Caps+N | Page Down | ✅ | ✅ | Perfect |

### Deletion & Insertion (3 features)

| Shortcut | Function | Windows | Linux | Notes |
|----------|----------|---------|-------|-------|
| Caps+H | Delete | ✅ | ✅ | Perfect |
| Caps+; | Backspace | ✅ | ✅ | Perfect |
| Caps+P | Insert | ✅ | ✅ | Perfect |

### Modifiers (4 features)

| Shortcut | Function | Windows | Linux | Notes |
|----------|----------|---------|-------|-------|
| Caps+F | Ctrl (hold) | ✅ | ✅ | Works with navigation |
| Caps+D | Shift (hold) | ✅ | ✅ | Works with navigation |
| Caps+S | Alt (hold) | ✅ | ✅ | Works with navigation |
| Caps+W | Super/Win (hold) | ✅ | ✅ | Works with navigation |

### Text Editing (6 features)

| Shortcut | Function | Windows | Linux | Notes |
|----------|----------|---------|-------|-------|
| Caps+A | Select All (Ctrl+A) | ✅ | ✅ | Perfect |
| Caps+Z | Undo (Ctrl+Z) | ✅ | ✅ | Perfect |
| Caps+X | Cut (Ctrl+X) | ✅ | ✅ | Perfect |
| Caps+C | Copy (Ctrl+C) | ✅ | ✅ | Perfect |
| Caps+V | Paste (Ctrl+V) | ✅ | ✅ | Perfect |
| Caps+B | Bold (Ctrl+B) | ✅ | ✅ | Perfect |

### Speed Navigation (4 features)

| Shortcut | Function | Windows | Linux | Notes |
|----------|----------|---------|-------|-------|
| Caps+8 | Up ×5 | ✅ | ✅ | Macro in Kanata |
| Caps+, | Down ×5 | ✅ | ✅ | Macro in Kanata |
| Caps+M | Left ×6 words | ✅ | ✅ | Ctrl+Left ×6 macro |
| Caps+. | Right ×6 words | ✅ | ✅ | Ctrl+Right ×6 macro |

### Special Keys (5 features)

| Shortcut | Function | Windows | Linux | Notes |
|----------|----------|---------|-------|-------|
| Caps+Q | Escape | ✅ | ✅ | Perfect |
| Caps+R | Rename (F2) | ✅ | ✅ | Perfect |
| Caps+T | Select line | ✅ | ✅ | Home+Shift+Down |
| Caps+7 | Tab | ✅ | ✅ | Perfect |
| Caps+\` | Close window (Alt+F4) | ✅ | ✅ | Perfect |
| Caps+F7 | Enter | ✅ | ✅ | Perfect |

---

## ❌ NOT IMPLEMENTED (9 features)

### Case Change (3 features)

| Shortcut | Function | Windows | Linux | Why Not |
|----------|----------|---------|-------|---------|
| Caps+9 | UPPERCASE | ✅ | ❌ | Requires cmd flag |
| Caps+0 | lowercase | ✅ | ❌ | Requires cmd flag |
| Caps+- | Title Case | ✅ | ❌ | Requires cmd flag |

**Explanation:**
- Case change requires external scripts via `(cmd ...)` in Kanata
- Default Kanata binary is compiled without `cmd` support for security
- To enable: Recompile Kanata with `cargo build --release --features cmd`
- Alternative: Use built-in editor shortcuts (e.g., Ctrl+Shift+U in some apps)

### System Integration (2 features)

| Shortcut | Function | Windows | Linux | Workaround |
|----------|----------|---------|-------|------------|
| Caps+= | Always On Top | ✅ | ❌ | Configure in WM/DE settings |
| Caps+F10 | Help window | ✅ | ❌ | Use CHEATSHEET_ADVANCED.md |

**Always On Top alternatives:**
- **GNOME:** Right-click titlebar → "Always on Top"
- **KDE:** Right-click titlebar → "More Actions" → "Keep Above Others"
- **i3/sway:** Use window rules in config
- **Create custom shortcut:** `wmctrl -r :ACTIVE: -b toggle,above`

### Complex Functions (2 features)

| Shortcut | Function | Windows | Linux | Workaround |
|----------|----------|---------|-------|------------|
| Alt+G | Google Search | ✅ | ❌ | Use browser shortcuts |
| Language switch | Commented out | 🔵 | 🔵 | Not in original script |

**Google Search alternative:**
- Select text → Ctrl+C
- Open browser → Ctrl+T (new tab)
- Ctrl+K or Ctrl+L (focus search)
- Ctrl+V → Enter

**Or use browser extensions:**
- "Context Menu Search" (Chrome/Edge)
- "Swift Selection Search" (Firefox)

### Help System (1 feature)

The help window (F10) shows a formatted GUI with all shortcuts. This is not possible in Kanata as it only handles keyboard events, not GUI rendering.

**Alternative:**
Print or save the cheatsheet (CHEATSHEET_ADVANCED.md) for reference.

### Icon & System Tray (1 feature)

| Feature | Windows | Linux | Notes |
|---------|---------|-------|-------|
| Tray icon | ✅ | ❌ | Kanata runs as systemd service |

**Status check:**
```bash
systemctl --user status kanata.service  # Check if running
journalctl --user -u kanata.service -f  # Live logs
```

---

## 📊 Feature Summary

| Category | Total | Implemented | Percentage |
|----------|-------|-------------|------------|
| Core Navigation | 11 | 11 | 100% |
| Modifiers | 4 | 4 | 100% |
| Text Editing | 6 | 6 | 100% |
| Speed Navigation | 4 | 4 | 100% |
| Special Keys | 6 | 5 | 83.3% |
| Case Change | 3 | 0 | 0% (cmd required) |
| System Integration | 2 | 0 | 0% (WM-dependent) |
| External Tools | 1 | 0 | 0% (browser-dependent) |
| GUI/Tray | 2 | 0 | 0% (not applicable) |
| **TOTAL** | **39** | **30** | **76.9%** |

---

## 🎯 What This Means

### Perfect Parity (30 features)
All core keyboard remapping works identically:
- Navigation, modifiers, text editing, special keys, speed navigation
- Same muscle memory as Windows version
- Zero behavior differences

### Requires Recompilation (3 features)
Case change requires Kanata built with cmd flag:
```bash
# Optional: Enable case change
git clone https://github.com/jtroo/kanata
cd kanata
cargo build --release --features cmd
sudo cp target/release/kanata /usr/local/bin/
systemctl --user restart kanata.service
```

### OS/DE Dependent (6 features)
These never worked in pure Kanata anyway:
- Always On Top → WM feature, not keyboard emulation
- Help window → GUI rendering, not possible in Kanata
- Google Search → Browser integration, better handled by extensions
- System tray → Status monitoring, use `systemctl` instead

---

## 💡 Recommendations

### Use Case 1: Pure Keyboard Remapping
**Install:** Advanced version (as-is)
**Reason:** 100% keyboard functionality for all core features

### Use Case 2: Need Case Change
**Install:** Advanced version + recompile Kanata
**Reason:** Adds case transformation (33/39 = 84.6%)

### Use Case 3: Minimalist Setup
**Install:** Basic version
**Reason:** Fewer features (19/39), faster setup

### Use Case 4: Feature Parity with Windows
**Install:** Advanced + WM shortcuts + recompile
**Reason:** Closest match to AHK script (95%+ equivalent)

---

## 📄 Migration Path from Windows

1. **Day 1:** Install Advanced version (30 shortcuts)
2. **Week 1:** Test all core shortcuts
3. **Week 2:** Configure WM-specific features (Always On Top, etc.)
4. **Week 3:** Install browser extensions for search integration
5. **Week 4:** Print cheatsheet to replace help window (F10)
6. **Optional:** Recompile Kanata for case change if needed

**Expected adjustment time:** 2-3 weeks for full muscle memory transfer

---

## 📖 Related Documents

- **README_ADVANCED.md** - Quick start guide
- **INSTALL_ADVANCED.md** - Detailed installation & uninstallation
- **CHEATSHEET_ADVANCED.md** - Printable reference
- **config_advanced.kbd** - Full Kanata configuration

