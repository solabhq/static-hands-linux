# Static Hands Advanced - Cheatsheet

**Print this page for quick reference**

---

## NAVIGATION (8)

| Key | Action |
|-----|--------|
| Caps + I | ↑ Up |
| Caps + J | ← Left |
| Caps + K | ↓ Down |
| Caps + L | → Right |
| Caps + U | Home |
| Caps + O | End |
| Caps + Y | Page Up |
| Caps + N | Page Down |

---

## DELETION & INSERTION (3)

| Key | Action |
|-----|--------|
| Caps + H | Delete |
| Caps + ; | Backspace |
| Caps + P | Insert |

---

## MODIFIERS - Hold (4)

| Key | Modifier |
|-----|----------|
| Caps + F | Ctrl |
| Caps + D | Shift |
| Caps + S | Alt |
| Caps + W | Super/Win |

**Examples:**
- `Caps+F+L` = Ctrl+Right (jump word)
- `Caps+D+L` = Shift+Right (select)
- `Caps+F+U` = Ctrl+Home (start of doc)
- `Caps+F+D+L` = Ctrl+Shift+Right (select word)

---

## TEXT EDITING (6)

| Key | Action |
|-----|--------|
| Caps + A | Select All (Ctrl+A) |
| Caps + Z | Undo (Ctrl+Z) |
| Caps + X | Cut (Ctrl+X) |
| Caps + C | Copy (Ctrl+C) |
| Caps + V | Paste (Ctrl+V) |
| Caps + B | Bold (Ctrl+B) |

---

## SPEED NAVIGATION (4)

| Key | Action |
|-----|--------|
| Caps + 8 | Up ×5 |
| Caps + , | Down ×5 |
| Caps + M | Left ×6 words |
| Caps + . | Right ×6 words |

---

## SPECIAL KEYS (5)

| Key | Action |
|-----|--------|
| Caps + Q | Escape |
| Caps + R | Rename (F2) |
| Caps + T | Select line |
| Caps + 7 | Tab |
| Caps + \` | Close window (Alt+F4) |
| Caps + F7 | Enter |

---

## TROUBLESHOOTING

**Kanata not working?**
```bash
systemctl --user status kanata.service
```

**Check logs:**
```bash
journalctl --user -u kanata.service -n 20
```

**Restart Kanata:**
```bash
systemctl --user restart kanata.service
```

---

## NOT INCLUDED

These features from Windows version don't work on Linux:

- **Caps+9/0/-** (Case change) - Requires recompiling Kanata with cmd flag
- **Caps+=** (Always On Top) - Configure in window manager
- **Caps+F10** (Help) - Use this cheatsheet instead
- **Alt+G** (Google Search) - Use browser extensions

See COMPARISON_ADVANCED.md for details and workarounds.

---

**🖨️ Print this page for quick reference!**
