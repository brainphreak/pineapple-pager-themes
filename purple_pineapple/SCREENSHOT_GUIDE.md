# WiFi Pineapple Pager Screenshot Guide

---
## *** STOP - READ THIS ENTIRE DOCUMENT BEFORE TAKING ANY SCREENSHOTS ***

**CLAUDE: If asked to take screenshots, you MUST:**
1. Read this entire document first
2. Summarize the key points to the user before starting
3. Take ONE test capture and verify it's correct
4. Only then proceed with multiple captures

**USER: Before any screenshot session, ask Claude to:**
1. "Read SCREENSHOT_GUIDE.md and summarize the key points"
2. "Take one test screenshot and verify it before continuing"
3. "Show me the file list after each capture"

---

## PREVENTING FILE OVERWRITE

**After EVERY capture, run this command and show the output to the user:**
```bash
ls -la screenshots/*.png | wc -l && ls -lat screenshots/*.png | head -5
```

This shows:
- Total number of screenshot files (should INCREASE by 1 after each capture)
- Most recent files by timestamp

**Before starting, note the file count. After each capture:**
- File count should be: previous count + 1
- If file count stays the same, YOU ARE OVERWRITING FILES. STOP.

**Example tracking:**
- Before starting: 10 files
- After capture 1: 11 files (alerts.png added)
- After capture 2: 12 files (boot.png added)
- After capture 3: 12 files <-- PROBLEM! File was overwritten, not added

---

## Framebuffer Specifications

- **Dimensions**: 222 x 480 (width x height) - NOT 480x222
- **Format**: RGB565 (16-bit, little-endian)
- **Rotation**: 90 degrees clockwise after capture
- **Final output**: 480 x 222 landscape image

## Capture Command

```bash
ssh root@172.16.52.1 "cat /dev/fb0" > /tmp/fb_raw.bin && python3 -c "
from PIL import Image
import struct
import sys

name = sys.argv[1]

with open('/tmp/fb_raw.bin', 'rb') as f:
    data = f.read()

# IMPORTANT: Width=222, Height=480 (NOT the other way around)
img = Image.new('RGB', (222, 480))
pixels = img.load()

for y in range(480):
    for x in range(222):
        idx = (y * 222 + x) * 2
        if idx + 1 < len(data):
            pixel = struct.unpack('<H', data[idx:idx+2])[0]
            r = ((pixel >> 11) & 0x1F) << 3
            g = ((pixel >> 5) & 0x3F) << 2
            b = (pixel & 0x1F) << 3
            pixels[x, y] = (r, g, b)

# Rotate 90 degrees clockwise
img = img.rotate(90, expand=True)
img.save(f'/Users/brainphreak/wifi-pineapple/themes/purple/github/purple_pineapple/screenshots/{name}.png')
print(f'Saved screenshots/{name}.png')
" "$1"
```

## CRITICAL RULES

1. **UNIQUE FILENAMES**: Every screenshot MUST have a unique filename. NEVER save multiple screenshots to the same file path. Back up to /tmp/ with unique names if needed.

2. **VERIFY EACH CAPTURE**: After EVERY screenshot, verify:
   - File exists at the expected path
   - Image dimensions are 480x222 (landscape)
   - Image is NOT corrupt (no diagonal stripes, garbled pixels, or artifacts)
   - Text is readable and RIGHT-SIDE UP (not sideways, not upside down)
   - Colors look correct

3. **VERIFICATION COMMAND**: Run this after each capture:
   ```bash
   python3 -c "from PIL import Image; img=Image.open('screenshots/NAME.png'); print(f'Size: {img.size}, Mode: {img.mode}')"
   ```
   Expected output: `Size: (480, 222), Mode: RGB`

4. **IF OUTPUT LOOKS WRONG**:
   - Diagonal stripes = WRONG DIMENSIONS (222x480 vs 480x222 swapped)
   - Text sideways = WRONG ROTATION
   - Text upside down = ROTATE WRONG DIRECTION
   - Do NOT make excuses like "matrix-style effect" - it means something is broken
   - STOP immediately and fix before continuing

5. **BACKUP STRATEGY**: Save a backup copy to /tmp/ with timestamp:
   ```bash
   cp screenshots/NAME.png /tmp/NAME_backup_$(date +%s).png
   ```

6. **LIST FILES AFTER EACH CAPTURE**: Run `ls -la screenshots/` to confirm the file was saved with the correct unique name and has a reasonable file size (typically 15-50KB for a proper screenshot).

## Visual Verification

After saving each screenshot, USE THE READ TOOL to visually inspect the image:

```
Read screenshots/NAME.png
```

**What a CORRECT screenshot looks like:**
- Landscape orientation (wider than tall)
- Text is horizontal and readable (e.g., "Alerts", "PineAP", "Settings")
- Status bar visible at top with time, battery icon
- Clear UI elements, menus, buttons

**What a CORRUPT/WRONG screenshot looks like:**
- Diagonal stripes or lines
- Garbled colored pixels
- Text is sideways or upside down
- No recognizable UI elements

**If you see diagonal stripes or garbled output, the capture is WRONG. Do not continue.**

## Checklist Before Taking Multiple Screenshots

- [ ] Capture script uses 222x480 dimensions (NOT 480x222)
- [ ] Capture script rotates 90 degrees clockwise
- [ ] Each capture saves to a UNIQUE filename
- [ ] First capture visually verified as correct before continuing
- [ ] Dimensions verified as 480x222 after rotation
- [ ] File exists in screenshots folder after saving
- [ ] File size is reasonable (15-50KB, not 0 bytes)

## Common Mistakes to Avoid

1. Using 480x222 instead of 222x480 - produces diagonal stripe artifacts
2. Saving all captures to `/tmp/screenshot.png` - overwrites previous captures
3. Claiming garbled output is "matrix-style" or some visual effect
4. Continuing to capture without verifying the first one worked

## Screenshot Naming Convention

Use descriptive names:
- `boot.png` - Boot animation
- `dashboard.png` - Main dashboard
- `alerts.png` - Alerts screen
- `payloads.png` - Payloads menu
- `payloads_category.png` - Payload category list
- `payload_launch.png` - Payload launch dialog
- `recon.png` - Recon screen
- `pineap.png` - PineAP main menu
- `pineap_submenu.png` - PineAP submenu
- `settings.png` - Settings menu
- `keyboard.png` - On-screen keyboard
- `dialog.png` - Dialog box
- `confirmation.png` - Yes/No confirmation dialog
- `wardriving.png` - Wardriving menu
- `geiger.png` - Geiger toggle on dashboard
