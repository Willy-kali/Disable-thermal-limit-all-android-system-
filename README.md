# ⚠️ THERMAL THROTTLING DISABLER ⚠️
## Force Disable Thermal Protection (Non-Root)

---

## 🚨 **EXTREME DANGER WARNING** 🚨

**READ THIS ENTIRE SECTION BEFORE PROCEEDING!**

### What This Does:
This script **COMPLETELY DISABLES** your device's thermal protection and CPU/GPU throttling. Your device will **NO LONGER PROTECT ITSELF** from overheating.

### Potential Consequences:
- 🔥 **Device overheating** (can reach 60°C+ easily)
- 💥 **Hardware damage** (CPU, GPU, battery, display)
- 🔋 **Battery swelling** or **battery damage**
- 📱 **Permanent performance degradation**
- ⚡ **Shortened device lifespan**
- 🔌 **Fire hazard** (in extreme cases)
- 💸 **Voided warranty**

### Who Should Use This:
- ❌ **NOT for daily use**
- ❌ **NOT for beginners**
- ✅ Benchmark testing (monitor temperature!)
- ✅ Gaming (short sessions with cooling)
- ✅ Temporary performance boost (< 30 min)
- ✅ Advanced users who understand the risks

### You Are Responsible For:
- Any hardware damage
- Fire or safety hazards
- Loss of device functionality
- Warranty issues

**IF YOU DON'T UNDERSTAND THE RISKS, DO NOT USE THIS!**

---

## 📦 Files Included

| File | Purpose | Use Case |
|------|---------|----------|
| `disable_thermal.sh` | Main script (device) | Direct execution |
| `disable_thermal_termux.sh` | Termux version | On-device via Termux |
| `restore_thermal.sh` | Safety restore | Re-enable protection |
| `adb_disable_thermal.sh` | PC ADB version | Most reliable method |
| `adb_restore_thermal.sh` | PC restore | Re-enable via ADB |

---

## 🛠️ Installation Methods

### Method 1: ADB from PC (RECOMMENDED)

**Why This Method:**
- ✅ Most reliable
- ✅ Best permission access
- ✅ Easiest to restore
- ✅ Can monitor from PC

**Requirements:**
- PC with ADB installed
- USB cable
- USB Debugging enabled

**Steps:**

1. **Install ADB:**
   - Windows: Download [Platform Tools](https://developer.android.com/studio/releases/platform-tools)
   - Mac: `brew install android-platform-tools`
   - Linux: `sudo apt install android-tools-adb`

2. **Enable USB Debugging:**
   - Settings → About Phone → Tap "Build Number" 7 times
   - Settings → Developer Options → Enable "USB Debugging"

3. **Connect Device:**
   ```bash
   # Check connection
   adb devices
   
   # Should show: <device_id>    device
   ```

4. **Run Script:**
   ```bash
   chmod +x adb_disable_thermal.sh
   ./adb_disable_thermal.sh
   ```

5. **Monitor Temperature:**
   ```bash
   # Check CPU temp via ADB
   adb shell cat /sys/class/thermal/thermal_zone0/temp
   ```

---

### Method 2: Termux (On-Device)

**Requirements:**
- Termux app installed
- Shizuku (recommended) or root access

**Steps:**

**With Shizuku (No Root):**
1. Install Shizuku from Play Store
2. Start Shizuku (wireless ADB or pairing)
3. Grant Termux permission in Shizuku
4. In Termux:
   ```bash
   cd /sdcard
   chmod +x disable_thermal_termux.sh
   sh disable_thermal_termux.sh
   ```

**With Root:**
1. Grant Termux root access
2. Run:
   ```bash
   su
   sh disable_thermal_termux.sh
   ```

---

### Method 3: Manual Execution (Advanced)

**Execute commands one by one:**

```bash
# Stop thermal services
setprop debug.svc.thermal_manager stopped
setprop debug.svc.thermald stopped

# Disable throttling
settings put global thermal.manager.enabled 0
settings put global thermal.throttle.disabled 1
settings put global power.throttling.disable 1

# Disable CPU/GPU throttle
settings put global thermal.cpu_thermal_throttle.disable 1
settings put global thermal.gpu_thermal_throttle.disable 1

# Device config
cmd device_config put hardware_properties_manager thermal_throttling_enabled false
cmd device_config put hardware_properties_manager cpu_throttling_disabled true
cmd device_config put hardware_properties_manager gpu_throttling_disabled true

# Performance mode
cmd power set-mode 1
setprop sys.perf.profile 2
```

---

## 🌡️ Temperature Monitoring (CRITICAL!)

### ALWAYS Monitor Temperature!

**Install Temperature Monitor Apps:**
- CPU-Z (free, accurate)
- DevCheck (detailed info)
- AIDA64 (comprehensive)
- Simple System Monitor (lightweight)

### Temperature Safety Zones:

| Temperature | Status | Action |
|-------------|--------|--------|
| 30-38°C | ✅ SAFE | Normal operation |
| 39-42°C | 🟡 WARM | Monitor closely |
| 43-47°C | 🟠 HOT | Reduce load immediately |
| 48-52°C | 🔴 DANGER | **STOP EVERYTHING!** |
| 53°C+ | ☢️ CRITICAL | **SHUT DOWN NOW!** |

### ADB Temperature Monitoring:

```bash
# Real-time temperature monitoring (from PC)
while true; do
  adb shell cat /sys/class/thermal/thermal_zone0/temp
  sleep 2
done

# Or create a monitoring script:
watch -n 2 'adb shell cat /sys/class/thermal/thermal_zone*/temp'
```

---

## 🔄 Restoring Thermal Protection

### Quick Restore Methods:

**1. Restart Device (Easiest):**
- Most settings reset on reboot
- Safest method
- Takes 1-2 minutes

**2. Run Restore Script:**

**Via ADB (from PC):**
```bash
./adb_restore_thermal.sh
```

**Via Termux (on device):**
```bash
sh restore_thermal.sh
```

**3. Manual Restore:**
```bash
# Re-enable thermal manager
settings put global thermal.manager.enabled 1
settings put global thermal.throttle.disabled 0

# Re-enable throttling
settings put global power.throttling.disable 0
settings put global thermal.cpu_thermal_throttle.disable 0
settings put global thermal.gpu_thermal_throttle.disable 0

# Restart services
setprop debug.svc.thermal_manager running
setprop debug.svc.thermald running
```

---

## ⚙️ Verification

### Check if Thermal is Disabled:

**Via ADB:**
```bash
adb shell settings get global thermal.manager.enabled
# Should return: 0 (disabled)

adb shell settings get global thermal.throttle.disabled
# Should return: 1 (disabled)
```

**Via Termux:**
```bash
settings get global thermal.manager.enabled
settings get global thermal.throttle.disabled
```

### Check Current Temperature:
```bash
# Via ADB
adb shell cat /sys/class/thermal/thermal_zone*/temp

# Via Termux
cat /sys/class/thermal/thermal_zone*/temp
```

---

## 🎮 Use Case Examples

### Gaming (Recommended Setup):

1. **Before Gaming:**
   - Enable airplane mode (reduce heat from modem)
   - Close all background apps
   - Lower screen brightness
   - Remove phone case (better cooling)
   - Place near fan or AC

2. **During Gaming:**
   - Monitor temperature every 5 minutes
   - Take breaks every 15-20 minutes
   - If temp > 45°C, stop immediately

3. **After Gaming:**
   - Run restore script
   - Let device cool down
   - Don't charge while hot

### Benchmarking:

1. Run benchmark in short bursts (< 5 min)
2. Monitor temperature constantly
3. Stop if temperature exceeds 48°C
4. Allow cooling between runs
5. Restore thermal protection after testing

---

## 🚫 What NOT to Do

❌ **NEVER:**
- Leave device unattended while thermal is disabled
- Use while charging (generates extra heat)
- Use for extended periods (> 30 min)
- Use in hot environments
- Use with phone case on (traps heat)
- Play graphics-intensive games for hours
- Ignore temperature warnings
- Let device reach 50°C+

---

## 🆘 Emergency Procedures

### If Device Gets Too Hot (> 48°C):

1. **IMMEDIATELY:**
   - Stop all apps
   - Lock screen
   - Remove from charger
   - Remove case

2. **Cool Down:**
   - Place in cool (not cold!) area
   - Near fan or AC (not directly on it)
   - Don't put in refrigerator! (condensation damage)

3. **Restore Protection:**
   ```bash
   # Via ADB
   ./adb_restore_thermal.sh
   
   # Via Termux
   sh restore_thermal.sh
   
   # Or just restart device
   ```

4. **Wait:**
   - Let device cool to < 35°C before use
   - Check for any damage (battery swelling, screen issues)

---

## 🔧 Troubleshooting

### Settings Not Applying:

**Problem:** Commands fail or revert
**Solution:**
- Try with Shizuku (Termux)
- Use ADB from PC instead
- Grant WRITE_SECURE_SETTINGS:
  ```bash
  adb shell pm grant com.android.shell android.permission.WRITE_SECURE_SETTINGS
  ```

### Device Still Throttling:

**Problem:** Performance still limited
**Solution:**
- Some devices have hardware thermal limits
- Check if manufacturer (Samsung, Xiaomi) has additional protections
- May need root for full control
- Try enabling performance mode separately

### Temperature Sensors Not Working:

**Problem:** Can't read temperature
**Solution:**
```bash
# Try different thermal zones
cat /sys/class/thermal/thermal_zone0/temp
cat /sys/class/thermal/thermal_zone1/temp
cat /sys/devices/virtual/thermal/thermal_zone*/temp
```

---

## 📱 Device-Specific Notes

### Samsung (OneUI):
- Has Game Optimizing Service (GOS)
- May need to disable GOS separately
- Additional protections in Knox

### Xiaomi (MIUI/HyperOS):
- Has MIUI optimization
- Disable "Memory extension" and "MIUI optimization"
- May have vendor-specific thermal controls

### OnePlus (OxygenOS):
- Enable "High performance mode"
- Disable "Optimized charging"

### Google Pixel:
- Tensor chips run hot naturally
- Use extra caution
- Monitor more frequently

---

## 📊 Performance Impact

### Expected Gains:
- 10-25% better sustained performance
- Higher FPS in games
- Faster app loading
- Better benchmark scores

### Expected Costs:
- 5-15°C higher temperatures
- 20-40% faster battery drain
- Shorter device lifespan
- Risk of thermal damage

---

## ⚖️ Legal Disclaimer

**BY USING THESE SCRIPTS YOU AGREE:**

1. You understand the risks of disabling thermal protection
2. You accept full responsibility for any damage
3. The script author is not liable for any damages
4. You will monitor temperature during use
5. You will restore protection after use
6. You understand warranty may be voided
7. You are using at your own risk

**This is provided for educational purposes only.**

---

## 🔗 Recommended Apps

### Temperature Monitoring:
- CPU-Z (best free option)
- DevCheck Hardware and System Info
- AIDA64
- Simple System Monitor

### Performance Testing:
- Geekbench 6
- 3DMark
- AnTuTu Benchmark

### Game Enhancement:
- Game Booster (vendor-specific)
- Game Turbo (Xiaomi)
- Game Launcher (Samsung)

---

## 📞 Support & Safety

**If something goes wrong:**
1. Immediately restore thermal protection
2. Restart device
3. Let it cool down completely
4. Check for physical damage
5. If battery is swollen, stop using immediately

**Remember:**
- Hardware damage is permanent
- Temperature damage accumulates over time
- Each use without protection shortens lifespan
- **When in doubt, don't disable thermal protection!**

---

## ✅ Final Checklist Before Using

Before running the script, ensure:

- [ ] I understand the risks
- [ ] Temperature monitoring app installed
- [ ] Restore script ready
- [ ] Device is not charging
- [ ] Phone case removed
- [ ] In cool environment
- [ ] Know safe temperature limits
- [ ] Will monitor every 5-10 minutes
- [ ] Will restore protection after use
- [ ] Understand this voids warranty

**If you can't check all boxes, DO NOT PROCEED!**

---

**USE RESPONSIBLY. MONITOR TEMPERATURE. RESTORE PROTECTION AFTER USE.**

**🔥 Your device's safety is in your hands! 🔥**
