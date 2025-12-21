# CPU Load OSD Element Configuration Guide

## Overview

The CPU Load OSD element displays the current system CPU usage as a percentage. This helps monitor flight controller performance and identify potential overload conditions.

## Display Format

- **Format:** `CPU XXX%`
- **Examples:**
  - Normal load: `CPU  45%`
  - High load: `CPU  89%`
  - Overload: `CPU 125%` (blinking)

## Visual Feedback

- **Normal (<100%):** Steady display
- **Overload (≥100%):** Text blinks as a warning indicator

## Configuration Methods

### Method 1: CLI Configuration

Connect to your flight controller via CLI and use the `osd_layout` command:

#### Enable at Default Position (Bottom-Right: Column 23, Row 13)
```bash
osd_layout 0 169 23 13 V
save
```

#### Enable at Custom Position (Example: Column 10, Row 1)
```bash
osd_layout 0 169 10 1 V
save
```

#### Hide the Element
```bash
osd_layout 0 169 23 13 H
save
```

#### Command Syntax
```
osd_layout <layout_id> <element_id> <column> <row> <V|H>
```

- `layout_id`: 0-3 (0 = default, 1-3 = alternate layouts)
- `element_id`: 169 (OSD_CPU_LOAD)
- `column`: 0-29 (horizontal position)
- `row`: 0-15 (vertical position)
- `V` = Visible, `H` = Hidden

#### View Current Configuration
```bash
osd_layout 0 169
```

### Method 2: CMS (On-Screen Stick Menu)

1. **Enter OSD Menu**
   - Power on your flight controller with video transmitter connected
   - Access the CMS menu using your radio sticks

2. **Navigate to OSD Elements**
   - OSD → Layouts → Default → CPU LOAD

3. **Configure the Element**
   - **ENABLED:** Set to ON/OFF
   - **ROW:** Set vertical position (0-15)
   - **COLUMN:** Set horizontal position (0-29)
   - **PREVIEW:** View the element on screen

4. **Save and Exit**
   - Navigate back and the settings will be saved automatically

### Method 3: INAV Configurator (Web UI)

> **Note:** The INAV Configurator requires a separate update to display this element in the OSD tab.

Once the configurator is updated:

1. **Open INAV Configurator**
   - Connect your flight controller
   - Go to the **OSD** tab

2. **Find CPU_LOAD Element**
   - Scroll through the available OSD elements
   - Look for "CPU_LOAD" or "CPU LOAD"

3. **Configure Position**
   - **Drag and drop** the element on the preview screen
   - **Enable/disable** by clicking the element checkbox

4. **Save Settings**
   - Click "Save and Reboot" at the bottom of the page

## Default Configuration

- **Position:** Column 23, Row 13 (bottom-right corner)
- **Visibility:** Hidden by default
- **Layout:** Available in all 4 layouts (0-3)

## Use Cases

### Performance Monitoring
Monitor CPU load during flight to ensure the flight controller isn't overloaded:
- **0-70%:** Normal operation
- **70-90%:** High load, still acceptable
- **90-100%:** Near capacity, consider reducing features
- **>100%:** System overload - tasks are being delayed

### Troubleshooting
Use CPU load to diagnose performance issues:
- Sudden spikes may indicate excessive filtering or logging
- Persistent high load suggests the flight controller is undersized for your configuration
- Helps identify which features to disable for better performance

### Feature Tuning
When enabling new features (GPS, OSD elements, blackbox logging):
- Monitor CPU load to understand the performance impact
- Adjust OSD refresh rates if needed
- Balance features vs. performance

## Technical Details

- **Data Source:** `averageSystemLoadPercent` from scheduler
- **Update Rate:** Matches OSD refresh rate
- **Precision:** 1% increments
- **Range:** 0-999% (typical range 0-150%)
- **Overload Threshold:** 100%

## Recommended Positions

### Bottom-Right (Default)
```bash
osd_layout 0 169 23 13 V
```
Good for keeping critical flight data unobstructed.

### Top-Right
```bash
osd_layout 0 169 23 0 V
```
Visible with other system info like battery voltage.

### Bottom-Center
```bash
osd_layout 0 169 12 13 V
```
Central location for easy monitoring.

## Multiple Layout Support

Configure different positions for each layout:

```bash
# Layout 0 (Default) - Bottom-right
osd_layout 0 169 23 13 V

# Layout 1 (Alternate) - Top-right
osd_layout 1 169 23 0 V

# Layout 2 (Alternate) - Hidden
osd_layout 2 169 0 0 H

# Layout 3 (Alternate) - Bottom-left
osd_layout 3 169 1 13 V

save
```

Switch between layouts using flight modes (BOXOSDALT1, BOXOSDALT2, BOXOSDALT3).

## Troubleshooting

### Element Not Showing
1. Verify it's enabled: `osd_layout 0 169`
2. Check the position is within screen bounds
3. Ensure OSD is working (other elements visible)
4. Reboot after saving changes

### Always Shows 0%
1. Check scheduler is running (flight controller booted properly)
2. Verify firmware compiled correctly
3. Try different position on screen

### Configurator Doesn't Show CPU_LOAD
The INAV Configurator is a separate project and needs its own update. Use CLI or CMS configuration instead until the configurator is updated.

## Related Settings

- **OSD Framerate:** `osd_framerate_hz` - Affects how often CPU load updates
- **Video System:** `osd_video_system` - Affects available screen positions
- **OSD Unit:** `osd_units` - Doesn't affect CPU load (always percentage)

## Example Configurations

### Minimal Performance Impact
```bash
# Show CPU load only, hide non-critical elements
osd_layout 0 169 23 0 V
set osd_framerate_hz = 10  # Lower refresh rate
save
```

### Debug/Development Setup
```bash
# Show CPU load prominently for development
osd_layout 0 169 12 1 V  # Top-center position
save
```

### Competition/Racing
```bash
# Hide CPU load to reduce OSD clutter
osd_layout 0 169 0 0 H
save
```

## Firmware Information

- **Added in:** INAV 9.x (custom build)
- **Element ID:** 169 (OSD_CPU_LOAD)
- **Files Modified:**
  - `src/main/io/osd.h`
  - `src/main/io/osd.c`
  - `src/main/cms/cms_menu_osd.c`

## Additional Resources

- [INAV OSD Documentation](https://github.com/iNavFlight/inav/wiki/OSD)
- [INAV CLI Commands](https://github.com/iNavFlight/inav/wiki/CLI)
- [INAV Configurator](https://github.com/iNavFlight/inav-configurator)

---

**Last Updated:** 2025-12-21
