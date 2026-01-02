# DR-HHM009S Testing Procedure

## Prerequisites
- Home Assistant with this updated integration installed
- DR-HHM009S humidifier connected and authenticated

## Test Cases

### 1. Current Humidity Reading
**Expected:** Current humidity sensor should display actual room humidity
- Navigate to humidifier entity in Home Assistant
- Verify "Current Humidity" value matches device display
- **Success if:** Value updates and matches reality (was showing incorrectly before)

### 2. View Target Humidity Setpoint
**Expected:** Target humidity slider shows correct value for current mode
- Verify device is in Auto mode
- Check target humidity slider value
- **Success if:** Shows 45% (matching the `rhautolevel` value from diagnostic data)

### 3. Change Target Humidity in Auto Mode
**Expected:** Humidity setpoint changes when adjusted
- Set humidifier to Auto mode
- Change target humidity slider to 50%
- Wait 2-3 seconds for command to process
- **Success if:** 
  - No error messages
  - Slider stays at 50%
  - Device responds to change

### 4. Change Target Humidity in Sleep Mode
**Expected:** Humidity setpoint changes in Sleep mode
- Set humidifier to Sleep mode
- Change target humidity slider to 65%
- Wait 2-3 seconds
- **Success if:**
  - No error messages
  - Slider stays at 65%
  - Device responds to change

### 5. Manual Mode (Fog Level)
**Expected:** Manual mode shows fog level as percentage
- Set humidifier to Manual mode
- Observe target humidity range changes to 1-100
- Adjust slider
- **Success if:**
  - Slider works without errors
  - Device fog level responds

### 6. Mode Switching
**Expected:** Modes switch correctly between Auto/Sleep/Manual
- Try switching: Auto → Sleep → Manual → Auto
- **Success if:**
  - No errors during mode changes
  - Target humidity values persist correctly for each mode
  - Device mode indicator changes

### 7. RGB Light On/Off
**Expected:** RGB light entity appears and can be toggled
- Locate "RGB Light" entity for the humidifier
- Turn RGB light ON
- Turn RGB light OFF
- **Success if:**
  - Entity exists (new feature!)
  - Commands execute without errors
  - Physical device RGB light responds

### 8. RGB Light Color Change
**Expected:** Can change RGB light color
- Turn RGB light ON
- Open color picker
- Select different colors (red, green, blue, etc.)
- **Success if:**
  - Color changes are reflected on device
  - No error messages

### 9. RGB Light Brightness
**Expected:** Can adjust RGB light brightness
- Turn RGB light ON
- Adjust brightness slider
- Try both low (10%) and high (100%) values
- **Success if:**
  - Brightness changes on device
  - No error messages

### 10. RGB Light Effects
**Expected:** Can change RGB light mode/effect (if supported)
- Turn RGB light ON
- Look for "Effect" dropdown in light controls
- Try changing effects (Scene, Custom, etc.)
- **Success if:**
  - Effect changes work
  - Device responds to effect changes

## Troubleshooting

### If Current Humidity Not Updating
- Check Home Assistant logs for coordinator errors
- Verify device is online in Dreo app
- Try restarting Home Assistant

### If Humidity Setpoint Not Changing
- Check logs for "Directive name not found" errors
- Verify you're not in Manual mode when trying to set humidity %
- Check Dreo app to see if change was applied

### If RGB Light Not Appearing
- Verify `entitySupports` in device config includes "light"
- Check Home Assistant logs during startup
- Try reloading the integration

### If RGB Commands Fail
- Verify device model supports RGB (DR-HHM009S does)
- Check if device config has `rgb_light_entity_config` section
- Review logs for directive errors

## Validation

After testing, verify in logs that:
- No errors about "field not found"
- Commands use correct field names (`rhautolevel`, not `rh_auto`)
- Mode changes use integers (1, 2, 3) not strings
- RGB commands use `rgbon`, `rgbcolor`, etc.

## Log Monitoring

Enable debug logging for detailed output:
```yaml
logger:
  default: info
  logs:
    custom_components.dreo: debug
```

Look for these patterns indicating success:
- `"Using rhautolevel for target humidity"` (or similar)
- No `"Directive name not found"` errors
- No `"field not in state"` warnings

