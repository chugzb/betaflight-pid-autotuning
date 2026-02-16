# FPVtune — Automatic Betaflight PID Tuning from Blackbox Logs

**Upload your blackbox log. Get optimized PIDs. Fly better.**

🔗 **[fpvtune.com](https://fpvtune.com)**

---

## The Problem

Every FPV pilot knows the pain:

1. Fly a pack, notice oscillations or prop wash
2. Land, plug in USB, open Betaflight Configurator
3. Tweak P, I, D values... but which one? By how much?
4. Flash, fly again, still not right
5. Repeat for hours (or days)

Manual PID tuning is time-consuming, frustrating, and requires deep knowledge of control theory that most pilots don't have. Tools like Blackbox Explorer show you data, but you still have to interpret it yourself.

**Betaflight doesn't have autotune.** Pilots have been [requesting it for years](https://github.com/betaflight/betaflight/issues/6857). FPVtune fills that gap.

## How FPVtune Works

```
Blackbox Log (.bbl/.bfl) → FPVtune Analysis → Optimized PID Values
```

1. **Upload** your Betaflight blackbox log
2. FPVtune **analyzes** gyro data, noise patterns, step response, and filter performance
3. Get **recommended PID values** tailored to your specific quad

No guessing. No trial and error. No PhD in control systems required.

## What It Analyzes

| Analysis | What It Tells You |
|----------|-------------------|
| **Gyro Noise Spectrum** | Motor noise frequencies, vibration issues |
| **Step Response** | Whether P/D are too high or too low |
| **PID Error Tracking** | How well your current PIDs follow stick inputs |
| **Filter Performance** | If your filters are too aggressive or too loose |
| **Prop Wash Detection** | Identifies prop wash events and suggests fixes |

## Supported Configurations

- ✅ **Betaflight** 4.x / 4.5+
- ✅ **INAV**
- ✅ 5" freestyle, racing, cinewhoops, tiny whoops, 7" long range
- ✅ All motor/ESC/FC combinations
- ✅ Both 8K/8K and 4K/4K gyro/PID loop rates

## Quick Start

1. Enable **Blackbox logging** in Betaflight Configurator
2. Fly a normal pack (freestyle, cruising, whatever you usually fly)
3. Download the `.bbl` or `.bfl` log file from your FC
4. Go to **[fpvtune.com](https://fpvtune.com)** and upload
5. Apply the recommended PIDs in Betaflight
6. Fly and feel the difference

## Why Not Just Use Default PIDs?

Betaflight defaults are a compromise — they work "okay" on most quads but are optimized for none. Your specific frame, motors, props, weight, and flying style all affect what the ideal PIDs should be.

**Example improvements pilots see:**
- Reduced prop wash oscillation on quick descents
- Tighter stick feel without introducing oscillations
- Less motor heat from over-tuned D gain
- Smoother HD footage from reduced vibrations

## vs Other Tools

| Feature | FPVtune | PIDtoolbox | Blackbox Explorer |
|---------|---------|------------|-------------------|
| Auto PID recommendations | ✅ | ❌ | ❌ |
| Blackbox analysis | ✅ | ✅ | ✅ |
| Web-based (no install) | ✅ | ❌ (MATLAB) | ❌ (Desktop app) |
| Noise spectrum analysis | ✅ | ✅ | ✅ |
| Step response analysis | ✅ | ✅ | ❌ |
| Filter tuning suggestions | ✅ | ❌ | ❌ |
| Beginner friendly | ✅ | ❌ | ❌ |

## Community

- 🌐 Website: [fpvtune.com](https://fpvtune.com)
- 🐛 Issues & Feature Requests: [GitHub Issues](https://github.com/fpvtune/fpvtune/issues)

## Keywords

`betaflight pid tuning` · `automatic pid tuning` · `betaflight autotune` · `blackbox analyzer` · `fpv drone tuning tool` · `pid optimizer` · `prop wash fix` · `betaflight blackbox log analyzer` · `pidtoolbox alternative`

---

**Stop guessing your PIDs.** → [fpvtune.com](https://fpvtune.com)
