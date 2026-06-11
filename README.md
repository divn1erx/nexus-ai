# -# NEXUS — MARK XXXIX

### AI-Powered Desktop Assistant

NEXUS is a desktop AI assistant that combines **Claude Sonnet 4** for conversation with **17+ action tools** for full computer control. Voice, vision, files, browser, system settings, code — all through one dark cyberpunk UI.

Built with PyQt6, animated with QML, powered by Anthropic + Google Gemini + OpenRouter.

---

## Features

### AI & Language
- **Claude Sonnet 4** — primary conversation AI with tool-use loop
- **OpenRouter fallback** — 22+ free models for memory extraction, search, vision
- **Gemini 2.5 Flash** — file processing, planning, screen analysis
- **Multi-language** — responds in your language, extracts parameters in English
- **Persistent memory** — auto-extracts user facts (identity, preferences, projects) into structured JSON

### Voice & Audio
- Real-time mic input with voice activity detection (silence-gated)
- Google Speech Recognition (STT) + pyttsx3 offline TTS
- Selectable microphone device from Settings

### Computer Control
- Launch any app (45+ aliases, cross-platform)
- Volume, brightness, WiFi, dark mode, restart/shutdown
- PyAutoGUI mouse/keyboard automation
- Window management (minimize, maximize, close, focus)
- File CRUD, search, disk usage, desktop organization

### Vision & Camera
- Screen capture → Gemini Vision analysis (speaks results directly)
- Webcam capture with selectable camera index
- MediaPipe hand tracking (controls logo position via index finger)

### Web & Browser
- Playwright browser automation (navigate, search, click, type, fill forms)
- Web search (OpenRouter + DuckDuckGo fallback)
- YouTube (play, transcribe, summarize, trending)
- Google Flights search with multi-language date parsing
- Interactive OSM map (in-app tile rendering + browser Leaflet)

### Developer Tools
- Code assistant — write, edit, explain, run, build (auto-fix loop)
- Dev agent — builds multi-file projects from scratch
- Task planner — Gemini-powered, with retry/fix/replan error recovery
- Priority task queue — background execution with status tracking

### File Processing
- Images — describe, OCR, resize, compress, convert
- PDFs — summarize, extract text, convert to Word
- Documents — summarize, fix grammar, reformat
- Data — analyze, filter, sort, convert CSV/Excel/JSON
- Code — explain, review, fix, run, document, test
- Audio — transcribe, trim, convert
- Video — trim, extract audio/frame, compress, transcribe
- Archives — list, extract

### UI
- Dark cyberpunk PyQt6 interface (black background, blue-cyan accents)
- **QML-animated logo** — pulsing rings, concentric rotations, scanning line (GPU-accelerated)
- **Radial menu** — osu!-style circular menu (Settings, Audio, Maps, Cyber)
- **Chat panel** — floating overlay with text I/O
- **In-app map** — CartoDB Dark Matter tiles, drag-to-pan, zoom, markers
- **Grid overlay** — blue-cyan grid at 25% opacity
- **Settings** — CHAT MODE toggle, microphone/webcam selection
- F1 fullscreen, click logo to open menu

---

## Requirements

| Item | Details |
|---|---|
| **OS** | Windows 10/11 (primary), macOS/Linux (partial) |
| **Python** | 3.11+ |
| **Mic** | Required for voice input |
| **Webcam** | Optional — hand tracking & vision |
| **API Keys** | Anthropic (hardcoded), Gemini (free), OpenRouter (free) |

### Dependencies (main)
```
PyQt6, anthropic, sounddevice, speechrecognition, pyttsx3,
numpy, opencv-python, mediapipe, pillow, mss, requests,
playwright, pyautogui, psutil, comtypes, pycaw
```

Full list in `requirements.txt`.

---

## Installation

```powershell
# 1. Clone
git clone https://github.com/FatihMakes/Mark-XXXIX-OR.git
cd Mark-XXXIX-OR

# 2. Install dependencies
pip install -r requirements.txt
pip install anthropic speechrecognition pyttsx3 numpy

# 3. Install Playwright browsers
playwright install

# 4. Run
python main.py
```

### First Run
1. A setup overlay appears — enter your **Gemini API key** and **OpenRouter API key**
2. Select your **OS** (auto-detected)
3. NEXUS connects to Claude and shows "LISTENING" state
4. Click the logo → radial menu → SETTINGS to configure mic/webcam

---

## Build to EXE

```powershell
pip install pyinstaller
pyinstaller --onefile --windowed --name "NEXUS" `
  --add-data "actions;actions" --add-data "memory;memory" `
  --add-data "agent;agent" --add-data "config;config" `
  --add-data "core;core" --add-data "or_client.py;." `
  --add-data "065ab163103b6a9242de51e365dd4c30.png;." `
  --add-data "8c0f4686d12229dea0cddd9a65aa5ca4.png;." `
  --add-data "nexus_logo.qml;." --add-data "nexus_map.html;." `
  --add-data "cybersecurity_tools.txt;." `
  --hidden-import "PyQt6.QtQuick" --hidden-import "PyQt6.QtQuickWidgets" `
  --hidden-import "PyQt6.QtQml" --hidden-import "sounddevice" `
  --hidden-import "numpy" --hidden-import "cv2" --hidden-import "mss" `
  --hidden-import "PIL" --hidden-import "requests" `
  --hidden-import "anthropic" --hidden-import "speech_recognition" `
  --hidden-import "pyttsx3" --hidden-import "psutil" `
  --hidden-import "comtypes" --hidden-import "pycaw" `
  main.py
```

Output: `dist/NEXUS.exe`

---

## API Keys

| Provider | Where | Used For |
|---|---|---|
| Anthropic | `main.py` (hardcoded) | Primary AI (Claude Sonnet 4) |
| Gemini | SetupOverlay → `config/api_keys.json` | File processing, vision, planning |
| OpenRouter | SetupOverlay → `config/api_keys.json` | Memory extraction, web search fallback |

---

## Project Structure

```
├── main.py                  # Entry point — AI loop, tools, hand tracking
├── ui.py                    # PyQt6 UI — all widgets, menus, panels
├── or_client.py             # OpenRouter API client (22+ free models)
├── nexus_logo.qml           # QML animated logo
├── nexus_map.html           # Leaflet fallback map
├── actions/                 # 17 tool modules
├── agent/                   # Planner, executor, task queue, error handler
├── memory/                  # Long-term memory manager
├── config/                  # API key storage
├── core/prompt.txt          # AI system prompt
└── *.png                    # Logo + bar assets
```

---

## Architecture

```
User Input (mic / text)
       │
       ▼
  ┌─────────────┐     ┌──────────────┐
  │  Claude API  │────▶│  Tool Executor │
  │ (anthropic)  │◀────│  (main.py)    │
  └─────────────┘     └──────┬───────┘
                             │
                    ┌────────┴────────┐
                    │  17 Action Tools │
                    │ (actions/*.py)   │
                    └────────┬────────┘
                             │
                    ┌────────┴────────┐
                    │  Gemini / OR    │
                    │ (fallbacks)     │
                    └─────────────────┘

UI (ui.py) ← PyQt6 + QML ← Logo, Map, Chat, Menu, Grid
```

---

## Credits

- **Original project** by [@FatihMakes](https://www.youtube.com/@FatihMakes)
- **Modified & enhanced** by **div1ne_rx**
- AI backend: Anthropic Claude, Google Gemini, OpenRouter
- Map tiles: CartoDB Dark Matter

---

## License

Personal and non-commercial use only.
Licensed under **Creative Commons BY-NC 4.0**.

---

> "NEXUS online. Always watching. Always ready."
