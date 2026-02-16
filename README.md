# FPVtune — Automatic Betaflight PID Tuning from Blackbox Logs

**Upload your blackbox log. Get optimized PIDs. Fly better.**

🔗 **[fpvtune.com](https://fpvtune.com)** — Neural network-powered Betaflight PID tuning in under 30 seconds.

---

## The Problem

Every FPV pilot knows the pain of manual PID tuning:

1. Fly a pack, notice oscillations or prop wash
2. Land, plug in USB, open Betaflight Configurator
3. Tweak P, I, D values... but which one? By how much?
4. Flash, fly again, still not right
5. Repeat for hours (or days)

Manual PID tuning is time-consuming, frustrating, and requires deep knowledge of control theory that most pilots don't have. Tools like Blackbox Explorer and PIDtoolbox show you raw data — gyro traces, FFT plots, step responses — but you still have to interpret it all yourself and decide what to change.

**Betaflight doesn't have autotune.** Pilots have been [requesting it for years](https://github.com/betaflight/betaflight/issues/6857), but the firmware team hasn't implemented it. FPVtune fills that gap with a neural network trained on thousands of real-world blackbox logs.

## How FPVtune Works

```
Blackbox Log (.bbl/.bfl) → Neural Network Analysis → Optimized PID + Filter Settings
```

1. **Upload** your Betaflight blackbox log (`.bbl` or `.bfl` file)
2. FPVtune's neural network **analyzes** gyro noise spectrum, step response, PID error tracking, and filter performance
3. Get **recommended PID values, filter settings, and feedforward gains** tailored to your specific quad
4. **Copy CLI commands** directly into Betaflight Configurator — done

No guessing. No trial and error. No PhD in control systems required.

### Example CLI Output

```bash
# Betaflight PID Tuning by FPVtune
set p_pitch = 45
set i_pitch = 80
set d_pitch = 35
set f_pitch = 120
set p_roll = 42
set i_roll = 75
set d_roll = 30
set f_roll = 110
set p_yaw = 35
set i_yaw = 90
set d_min_roll = 22
set d_min_pitch = 25
set dyn_notch_count = 2
set dyn_notch_q = 350
set simplified_gyro_filter_multiplier = 120
save
```

## What It Analyzes

FPVtune performs deep analysis on your blackbox data across multiple dimensions:

| Analysis | What It Does | Why It Matters |
|----------|-------------|----------------|
| **Gyro Noise Spectrum** | FFT analysis of raw gyro data to identify motor noise frequencies and vibration patterns | Determines optimal filter cutoff frequencies and dynamic notch settings |
| **Step Response** | Measures how quickly and accurately your quad responds to stick inputs | Reveals if P and D gains are too high or too low for your setup |
| **PID Error Tracking** | Analyzes the difference between commanded and actual rotation rates | Shows how well your current PIDs follow stick inputs across all axes |
| **Filter Performance** | Evaluates gyro lowpass, D-term lowpass, and dynamic notch filter effectiveness | Identifies if filters are too aggressive (adding delay) or too loose (passing noise) |
| **Prop Wash Detection** | Identifies prop wash oscillation events during descents and quick direction changes | Optimizes D_min, D_max, and feedforward to eliminate prop wash bounce |
| **Motor Thermal Prediction** | Estimates motor temperature based on noise profile and D-gain levels | Prevents motor overheating from aggressive tuning |

## Understanding PID Tuning — A Quick Guide

If you're new to Betaflight PID tuning, here's what each parameter does:

### P Gain (Proportional)

The P gain controls how aggressively your drone corrects errors between the commanded and actual rotation rate. Think of it as a spring — higher P makes the quad snap to your stick inputs faster.

- **Too high**: Fast oscillations (visible jello in HD footage, audible buzz)
- **Too low**: Sluggish, mushy stick feel — the quad drifts and feels unresponsive
- **Sweet spot**: Crisp, locked-in response without vibrations

### I Gain (Integral)

The I gain corrects long-term drift and helps your quad hold attitude against external forces like wind. It accumulates error over time and pushes the quad back to where it should be.

- **Too high**: Slow, bouncy oscillations — the quad overshoots and wobbles back
- **Too low**: The quad drifts in hover, doesn't hold angle well in wind
- **Sweet spot**: Rock-solid hover with no drift, clean tracking in wind

### D Gain (Derivative)

The D gain dampens oscillations by predicting future error. It's the most sensitive parameter — too much D causes motor heat, too little allows prop wash and bounce-back.

- **Too high**: Hot motors, high-pitched motor whine, potential motor damage
- **Too low**: Prop wash oscillation on descents, bounce-back after flips/rolls
- **Sweet spot**: Clean descents, no bounce-back, motors stay cool

### Feedforward

Feedforward anticipates stick movements and adds an immediate response before the PID loop even detects an error. It reduces latency and makes the quad feel more connected to your inputs.

- **Too high**: Overshoot on quick stick movements, twitchy feel
- **Too low**: Slight delay between stick input and quad response
- **Sweet spot**: Instant response that matches your flying style

### Filters

Betaflight uses multiple filter stages to clean up gyro noise before it reaches the PID controller:

- **Gyro Lowpass**: Removes high-frequency noise from the gyro signal
- **D-term Lowpass**: Extra filtering on the D-term (which amplifies noise)
- **Dynamic Notch**: Automatically tracks and removes motor resonance frequencies
- **RPM Filter**: Uses bidirectional DShot to precisely filter motor harmonics

Over-filtering adds delay and makes the quad feel sluggish. Under-filtering passes noise to the motors, causing heat and vibrations. FPVtune finds the optimal balance for your specific noise profile.

## Supported Configurations

### Flight Controllers & Firmware

- ✅ **Betaflight** 4.3 / 4.4 / 4.5+
- ✅ **INAV** (experimental)
- ✅ All STM32 F4, F7, G4, H7 flight controllers
- ✅ Popular FCs: SpeedyBee F405, HAKRC F722, Mamba F405, Diatone Mamba, etc.

### Gyro Chips

- ✅ MPU6000
- ✅ ICM20689 / ICM20602
- ✅ ICM42688-P
- ✅ BMI270
- ✅ All other common MEMS gyroscopes

### ESC Protocols

- ✅ DShot150 / DShot300 / DShot600
- ✅ Bidirectional DShot (for RPM filtering)
- ✅ All BLHeli_S / BLHeli_32 / AM32 ESCs

### Drone Types

| Type | Size | Typical Use |
|------|------|-------------|
| Tiny Whoop | 65-85mm | Indoor flying, proximity |
| Toothpick | 2.5-3" | Light freestyle, cruising |
| Freestyle | 5" | Freestyle, bando, park flying |
| Racing | 5" | FPV racing, time trials |
| Cinewhoop | 3-3.5" | Cinematic, indoor video |
| Long Range | 7" | Long range cruising, cinematic |
| Cinelifter | 8-10"+ | Heavy camera rigs, cinema |
| X-Class | 10"+ | Large format racing |

### Loop Rates

- ✅ 8K/8K gyro/PID
- ✅ 4K/4K gyro/PID
- ✅ 8K/4K gyro/PID
- ✅ Any custom loop rate configuration

## Quick Start

### Step 1: Enable Blackbox Logging

In Betaflight Configurator, go to the **Blackbox** tab:

1. Set **Logging rate** to at least 2K (higher is better for analysis)
2. Choose your storage: onboard flash or SD card
3. Set **Debug mode** to `GYRO_SCALED` for best results
4. Save and reboot

### Step 2: Fly and Record

Fly a normal pack — freestyle, cruising, whatever you usually fly. Include a mix of:

- Hover (5-10 seconds)
- Moderate throttle cruising
- Some aggressive maneuvers (rolls, flips, split-S)
- A few quick descents (to capture prop wash behavior)

A 2-3 minute flight is ideal. You don't need a special test flight.

### Step 3: Upload to FPVtune

1. Download the `.bbl` or `.bfl` log file from your FC (via USB or SD card)
2. Go to **[fpvtune.com](https://fpvtune.com)**
3. Upload your log file
4. Wait ~30 seconds for analysis

### Step 4: Apply and Fly

1. Copy the generated CLI commands
2. Open Betaflight Configurator → CLI tab
3. Paste the commands and type `save`
4. Fly and feel the difference

## Why Not Just Use Default PIDs?

Betaflight defaults are a compromise — they work "okay" on most quads but are optimized for none. Your specific frame, motors, props, weight, and flying style all affect what the ideal PIDs should be.

**Real improvements pilots see after using FPVtune:**

- ✅ Reduced prop wash oscillation on quick descents
- ✅ Tighter, more locked-in stick feel
- ✅ Less motor heat from properly tuned D gain
- ✅ Smoother HD footage from reduced vibrations
- ✅ Better hover stability in wind
- ✅ Cleaner audio from reduced motor noise
- ✅ Longer flight times from more efficient motor operation

## vs Other Tools

| Feature | FPVtune | PIDtoolbox | Blackbox Explorer |
|---------|---------|------------|-------------------|
| **Auto PID recommendations** | ✅ Neural network | ❌ Manual only | ❌ Manual only |
| **Blackbox log analysis** | ✅ | ✅ | ✅ |
| **Web-based (no install)** | ✅ | ❌ (MATLAB runtime) | ✅ (PWA) |
| **Noise spectrum analysis** | ✅ | ✅ | ✅ |
| **Step response analysis** | ✅ | ✅ | ❌ |
| **Filter tuning suggestions** | ✅ Automatic | ❌ | ❌ |
| **Feedforward optimization** | ✅ | ❌ | ❌ |
| **Motor thermal prediction** | ✅ | ❌ | ❌ |
| **CLI command export** | ✅ One-click copy | ❌ | ❌ |
| **Beginner friendly** | ✅ | ❌ | ❌ |
| **Active development** | ✅ | ❌ (ended May 2024) | ✅ |

### Why Not PIDtoolbox?

[PIDtoolbox](https://github.com/bw1129/PIDtoolbox) was an excellent tool created by Brian White for analyzing blackbox logs. However, as of May 2024, development has ended. It also requires the MATLAB runtime (~2GB download), only runs on Windows and Mac, and — most importantly — it shows you data but doesn't tell you what to change. You still need to interpret the graphs yourself.

FPVtune takes the next step: it analyzes the same data and automatically generates optimized PID values, filter settings, and feedforward gains.

### Why Not Blackbox Explorer?

[Betaflight Blackbox Explorer](https://github.com/betaflight/blackbox-log-viewer) is the official log viewer from the Betaflight team. It's great for visualizing flight data and syncing logs with flight video. But like PIDtoolbox, it's a visualization tool — it shows you what happened, not what to do about it.

FPVtune complements Blackbox Explorer. Use Blackbox Explorer to review your flights visually, and FPVtune to get actionable PID recommendations.

## Common PID Tuning Problems (and How FPVtune Fixes Them)

### Prop Wash Oscillation

**Symptom**: Vibrations and oscillations during quick descents, power loops, or sharp direction changes.

**Cause**: Turbulent air from the propellers hits the frame and confuses the gyro. The PID controller overcorrects, creating a feedback loop.

**FPVtune fix**: Optimizes the D_min/D_max ratio and feedforward settings to dampen prop wash without making the quad feel sluggish. Also adjusts filter settings to ensure the D-term can react quickly enough.

### Hot Motors

**Symptom**: Motors are too hot to touch after a flight. Reduced motor lifespan.

**Cause**: Excessive D-gain amplifies gyro noise and sends it to the motors as rapid current fluctuations. Inadequate filtering makes this worse.

**FPVtune fix**: Our motor thermal prediction model estimates peak motor temperature and recommends safe D-gain limits. Filter settings are optimized to remove noise without adding excessive delay.

### Mid-Throttle Oscillation

**Symptom**: Vibrations or buzzing at certain throttle positions, usually around 40-60% throttle.

**Cause**: Motor resonance frequencies align with the PID loop at specific RPMs, creating a feedback loop.

**FPVtune fix**: Identifies the resonance frequencies from your gyro noise spectrum and configures dynamic notch filters to track and eliminate them. If you have bidirectional DShot, RPM filter settings are also optimized.

### Sluggish Stick Response

**Symptom**: The quad feels mushy, slow to respond, like flying through syrup.

**Cause**: P-gain too low, filters too aggressive (adding delay), or feedforward not configured properly.

**FPVtune fix**: Balances P-gain for crisp response, minimizes filter delay while maintaining clean signals, and optimizes feedforward for your flying style.

## Frequently Asked Questions

### How long does the analysis take?

Typically under 30 seconds. Upload your blackbox log and get results almost instantly.

### Do I need a special test flight?

No. Any normal flight works — freestyle, cruising, racing. A 2-3 minute flight with a mix of hover, cruising, and some maneuvers gives the best results. You don't need to do any special test patterns.

### What Betaflight versions are supported?

FPVtune supports Betaflight 4.3, 4.4, and 4.5+. The neural network understands the differences between firmware versions and generates appropriate settings for your specific installation.

### Does it work with RPM filtering?

Yes. FPVtune detects whether RPM filtering is enabled in your blackbox log and adjusts recommendations accordingly. With RPM filtering, you can typically run higher PID gains with less software filtering.

### Can I use it for racing drones?

Absolutely. The neural network has been trained on logs from freestyle, racing, cinematic, and long-range builds. It understands the different requirements — racing quads need low-latency, high-response tunes, while cinematic builds prioritize smooth footage.

### Is it safe? Will it damage my quad?

FPVtune includes motor thermal prediction to prevent recommending settings that could overheat your motors. The recommended PIDs are always within safe operating ranges. That said, always do a brief hover test after applying new PIDs before flying aggressively.

### How is this different from Betaflight's built-in presets?

Betaflight presets are generic starting points based on frame size. FPVtune analyzes YOUR specific quad's actual flight data — your exact motors, props, frame, weight, and vibration characteristics — to generate truly personalized settings.

## Roadmap

- [x] Betaflight 4.5+ blackbox log support
- [x] Neural network PID optimization
- [x] Gyro noise spectrum analysis
- [x] Step response analysis
- [x] One-click CLI export
- [x] Motor thermal prediction
- [x] Dynamic filter optimization
- [ ] INAV full support
- [ ] Batch analysis (multiple logs)
- [ ] Historical tune comparison
- [ ] Community tune sharing
- [ ] Mobile-friendly upload
- [ ] Betaflight Configurator plugin

## Community

- 🌐 Website: [fpvtune.com](https://fpvtune.com)
- 🐛 Issues & Feature Requests: [GitHub Issues](https://github.com/chugzb/betaflight-pid-autotuning/issues)

## Contributing

We welcome contributions! If you're interested in:

- **Blackbox log samples**: Share your logs (with permission) to help improve the neural network
- **Bug reports**: Found an issue? Open a GitHub issue with your log file and Betaflight version
- **Feature requests**: Have an idea? We'd love to hear it
- **Documentation**: Help improve this README or write tutorials

## License

This project is licensed under the [MIT License](LICENSE).

---

## Keywords

`betaflight pid tuning` · `automatic pid tuning` · `betaflight autotune` · `blackbox analyzer` · `fpv drone tuning tool` · `pid optimizer` · `prop wash fix` · `betaflight blackbox log analyzer` · `pidtoolbox alternative` · `betaflight filter tuning` · `fpv drone oscillation fix` · `betaflight cli commands` · `drone pid settings` · `blackbox log analysis` · `betaflight noise analysis` · `fpv tuning tool` · `betaflight step response` · `pid controller drone` · `betaflight d-term filter` · `rpm filter betaflight`

---

**Stop guessing your PIDs.** → [fpvtune.com](https://fpvtune.com)
