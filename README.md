# NEXUS — AI Desktop Interface

> **Originally: MARK XLVI (46)** by **FatihMakes** ([YouTube](https://youtube.com/@FatihMakes) · [GitHub](https://github.com/FatihMakes/Mark-XLVI))
> This version is a heavily modified & expanded fork with a completely rewritten UI and additional features.

A dark, immersive desktop AI interface with voice control, camera integration, remote access, real‑time animations, and smart‑home‑grade interactivity.

---

## ⚡ Features

| Category | Feature |
|----------|---------|
| **Voice** | Always‑listening microphone (sounddevice + Google Speech Recognition) |
| | "Hey Nexus" wake word (optional, toggle in Settings) |
| | Adjustable mic sensitivity slider |
| | Text‑to‑Speech (pyttsx3) |
| **Camera** | 1‑frame snapshot (built‑in webcam or phone IP camera) |
| | Touch anywhere to close |
| | Motion detection toggle (F4) |
| **UI / Animations** | Floating particle background |
| | Firefly sparkle system |
| | Scan‑line CRT overlay |
| | Breathing glow (8 rings + radial gradient) |
| | Ripple effect on Nexus click |
| | Audio visualizer rings (pulse with speech) |
| | Boot animation sequence |
| **Chat** | Floating, draggable, resizable chat window |
| | Logs all voice & text commands |
| **Settings** | Slide‑in panel from right (OutCubic 280ms) |
| | Mic sensitivity, camera, phone cam, wake word |
| **Remote** | QR code + 6‑char session key (FastAPI backend) |
| | Phone audio streaming via WebSocket |
| | Encrypted commands (AES‑256‑CBC) |
| **Productivity** | Screenshot capture (F3) |
| | Timer / countdown (voice "timer 5 minutes") |
| | Live weather display (wttr.in) |
| | System tray (minimize to tray) |
| **Window** | Full‑screen mode (F2) |
| | Ctrl+Space to focus chat input |
| | Ctrl+Q to quit |

---

## ⌨️ Shortcuts

| Key | Action |
|-----|--------|
| `F2` | Toggle full‑screen window |
| `F3` | Take a screenshot → saves to `~/Pictures/` |
| `F4` | Toggle motion detection (opens camera) |
| `F5` | Test Text‑to‑Speech |
| `Ctrl+Space` | Focus chat input field |
| `Ctrl+Q` | Quit application |
| Settings ⚙ | Slide panel open/close |

## 🎤 Voice Commands

- "Camera" / "كاميرا" — take a snapshot
- "Phone camera" / "كاميرا التلفون" — use phone IP camera
- "Timer 5 minutes" / "مؤقت" — start countdown
- "Weather" / "طقس" — refresh weather display
- "Screenshot" / "تصوير" — capture screen
- "Motion" / "حركة" — toggle motion detection
- "Hello" / "مرحبا" — Nexus responds
- Any other text → forwarded to `on_text_command` (Gemini, etc.)

---

## 🔧 Architecture

```
main.py  ──→  JarvisUI  ──→  MainWindow
                │                  ├── CenterGlow (face + glow + ripple)
                │                  ├── ParticleBG (floating dots)
                │                  ├── SparkleBG (fireflies)
                │                  ├── ScanLines (CRT overlay)
                │                  ├── AudioVis (pulsing rings)
                │                  ├── CameraWidget (1‑frame snapshot)
                │                  ├── FloatChat (draggable chat)
                │                  ├── SettingsPanel (slide‑in)
                │                  ├── RemoteOverlay (QR + key)
                │                  └── BootOverlay (startup anim)
                │
                ├── VoiceListener (sounddevice + SR)
                └── TTS (pyttsx3)
```

### Key integrations

- **`main.py`** (`NexusCore`) → connects `on_text_command` & `on_remote_clicked`
- **`dashboard/`** → FastAPI remote control server (port 8000)
- **`1aedc0181c3685891f68f5ff9bff0dcf.png`** — face image (center)
- **`8c0f4686d12229dea0cddd9a65aa5ca4.png`** — secondary image

---

## 📁 File layout

```
Mark-XLVI-main/
├── main.py                  # Entry point, NexusCore integration
├── ui.py                    # Full desktop UI (552 lines → rewritten)
├── dashboard/
│   ├── server.py            # FastAPI remote server
│   ├── __init__.py
│   └── static/
│       ├── app.html         # Dashboard web page (mobile‑responsive)
│       ├── login.html       # Login page (mobile‑responsive)
│       └── crypto-js.min.js # AES‑256 client lib
├── 1aedc0181c3685891f68f5ff9bff0dcf.png
├── 8c0f4686d12229dea0cddd9a65aa5ca4.png
└── README.md
```

---

## 🚀 Getting started

```bash
pip install PyQt6 opencv-python sounddevice SpeechRecognition numpy
pip install requests qrcode[pil] pyttsx3
pip install fastapi "uvicorn[standard]" cryptography python-multipart

python main.py
```

Open **Settings** → **Remote Control** to get the QR code and connect from your phone.

---

## Credits

- **Original project — MARK XLVI:** [FatihMakes](https://github.com/FatihMakes/Mark-XLVI) ([YouTube](https://youtube.com/@FatihMakes) · [Instagram](https://instagram.com/fatihmakes))
- **UI rewrite, animations, new features:** [div1ne_rx](discord.com/users/1405659284293029980)
- **Icons & visual style:** Minimal dark theme with blue‑accent glow
