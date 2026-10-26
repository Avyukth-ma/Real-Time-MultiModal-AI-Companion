# Real-Time MultiModal AI Companion

[![Python 3.11](https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev/)
[![Three.js](https://img.shields.io/badge/Three.js-black?style=for-the-badge&logo=three.js&logoColor=white)](https://threejs.org/)
[![Groq LPU](https://img.shields.io/badge/Groq-LPU%20Inference-F55036?style=for-the-badge)](https://groq.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

An interactive, real-time multimodal AI companion that **sees**, **listens**, **reasons**, and **speaks** with ultra-low latency directly in your web browser. Featuring live vision presence detection, real-time hand gesture interruption, streaming speech synthesis, a responsive 3D interactive hologram bust, and mathematical typesetting.

---

## Architecture Overview

```
                      +------------------------------------------+
                      |           React + Vite Client            |
                      |        (Three.js 3D Avatar / UI)         |
                      +------------------------------------------+
                                           |
                 WebRTC MediaStream        |   Full-Duplex WebSocket
                (Video & Audio Feeds)      |   (Events & Control Stream)
                                           v
                      +------------------------------------------+
                      |             FastAPI Backend              |
                      |          (Async Event Router)            |
                      +------------------------------------------+
                                           |
         +---------------------------------+---------------------------------+
         |                                 |                                 |
         v                                 v                                 v
+------------------+              +------------------+              +------------------+
|    Perception    |              |    Cognition     |              |    Synthesis     |
|     Pipeline     |              |     Pipeline     |              |     Pipeline     |
+------------------+              +------------------+              +------------------+
| * YOLOv8         |              | * Groq LPU       |              | * Edge-TTS       |
|   (Presence)     |              |   (gpt-oss-120b) |              |   Streaming      |
| * MediaPipe      |              | * Gemini Fallback|              | * Base64 Chunked |
|   (Gestures)     |              | * LangChain      |              |   Audio Queue    |
+------------------+              +------------------+              +------------------+
```

---

## Key Features

- **Real-Time Vision & Perception**:
  - Continuous user presence tracking powered by **YOLOv8** (`backend/models/yolov8n.pt`).
  - Real-time hand gesture recognition via **MediaPipe** (`backend/models/gesture_recognizer.task`).
    - **Thumbs Up**: Instant positive affirmation mode with verbal confirmation.
    - **Closed Fist (hold 2s)**: Enters low-power standby sleep state.
- **Sub-Second Cognition via Groq LPU**:
  - Streaming token-level reasoning utilizing ultra-fast LPU inference (`openai/gpt-oss-120b`).
  - Resilient fallback cascade to Google Gemini (`gemini-3.5-flash-lite`), NVIDIA Vision, or OpenAI.
- **Low-Latency Sentence-Boundary Audio Streaming**:
  - Asynchronous pipeline synthesizes speech per sentence boundary via **Edge-TTS** (`en-IN-PrabhatNeural` / `en-US-ChristopherNeural`).
  - Near-instant audio chunk playback while subsequent sentences buffer continuously.
- **Interactive 3D Holographic Avatar**:
  - Screen-anchored geometric humanoid bust built with **Three.js** and React Three Fiber.
  - Dynamic 6-state emissive telemetry reacting instantly to companion operational states.
- **Flexible Audio & Push-To-Talk**:
  - Push-to-Talk (PTT), Voice Activity Detection (VAD), and Web Speech API integration.

---

## Dynamic 6-State Hologram Telemetry

The 3D holographic avatar modulates its particle cloud, core lighting, and shader colors across 6 distinct states:

| State | Color | Hex Code | Description |
| :--- | :--- | :--- | :--- |
| **Idle** | Arc Cyan | `#00f2fe` / `#06b6d4` | Default standby state waiting for user interaction. |
| **Thinking** | Processing Amber | `#f59e0b` | Active cognitive reasoning and LLM token generation. |
| **Speaking** | High-Energy Stark Blue | `#38bdf8` | Real-time audio playback and sentence speech synthesis. |
| **Affirmation** | Systems Nominal Green | `#10b981` | Thumbs-up gesture positive verification and acknowledgment. |
| **Sleep** | Standby Slate Grey | `#475569` / `#334155` | Low-power resting state entered after holding a Closed Fist for 2s. |
| **Error** | Alert Crimson | `#ef4444` | Visual warning for network drops, timeouts, or pipeline exceptions. |

---

## Repository Structure

```text
Real-Time-MultiModal-AI-Companion/
├── .env.example                       # Root template for environment variables
├── .gitignore                          # Exclusion rules (keeps credentials, binaries, caches untracked)
├── LICENSE                             # MIT License
├── README.md                           # Master project documentation
├── pyrightconfig.json                  # Python type-checker configuration
│
├── backend/
│   ├── .env                            # [Local] API keys (Groq, Gemini, etc.)
│   ├── .env.example                    # Backend environment template
│   ├── main.py                         # FastAPI application bootstrap & WebSocket mount
│   ├── requirements.txt                # Python backend dependencies
│   ├── models/                         # [Local] Machine learning binary weights
│   │   ├── yolov8n.pt                  # YOLOv8 nano object detection weights
│   │   └── gesture_recognizer.task     # MediaPipe hand gesture model bundle
│   ├── app/
│   │   ├── api/
│   │   │   └── websocket.py            # WebSocket connection manager & stream dispatcher
│   │   ├── cognition/
│   │   │   ├── engine.py               # Cognitive engine orchestration (Groq / Gemini / Prompts)
│   │   │   └── avatar_engine.py        # Avatar mood, personality, and contextual state logic
│   │   ├── core/
│   │   │   └── config.py               # Pydantic Settings, model definitions, and API URLs
│   │   ├── perception/
│   │   │   └── detector.py             # YOLOv8 + MediaPipe real-time inference loop
│   │   ├── synthesis/                  # Speech synthesis modules & audio streaming
│   │   └── utils/                      # Helper routines, encoding, and timing utilities
│   └── tests/
│       └── fixtures/                   # [Local] Test audio fixtures
│
├── frontend/
│   ├── index.html                      # HTML5 entry point
│   ├── package.json                    # Node.js dependencies
│   ├── package-lock.json
│   ├── vite.config.js                  # Vite dev server configuration
│   ├── public/
│   │   ├── BrainStem.glb               # 3D robotic holographic model loaded by Three.js
│   │   └── jarvis_core.png             # Emissive particle core texture
│   └── src/
│       ├── main.jsx                    # React DOM root mounting
│       ├── App.jsx                     # Primary UI container and state manager
│       ├── App.css                     # Component styling & animations
│       ├── index.css                   # Global styling and glassmorphic utilities
│       ├── components/
│       │   ├── Hologram.jsx            # 3D Three.js holographic canvas with 6-color shaders
│       │   ├── AvatarVisualizer.jsx    # Audio waveform and frequency visualizer
│       │   ├── VideoPreview.jsx        # Webcam overlay with real-time detection boxes
│       │   ├── MediaControls.jsx       # Camera, mic, and push-to-talk controls
│       │   └── StatusBadge.jsx         # Connection & latency status indicators
│       └── hooks/
│           ├── useWebSocket.js         # WebSocket lifecycle and message dispatching
│           ├── useFrameExtractor.js    # Canvas video frame capture and base64 serialization
│           ├── useMediaStream.js       # WebRTC getUserMedia camera & microphone stream handler
│           └── usePushToTalk.js        # Audio recording & PTT state management
│
├── docs/
│   └── AIML Report.pdf                 # Comprehensive project review & academic report
│
└── scripts/
    ├── start_companion.bat             # 1-Click Windows Batch launcher (starts backend + frontend)
    └── start_companion.ps1             # 1-Click PowerShell launcher (starts backend + frontend)
```

---

## Quick Start Guide

### Prerequisites

- **Python**: `3.10` or `3.11`
- **Node.js**: `18.x` or `20.x` (with `npm`)
- **Hardware**: Webcam and microphone enabled in your browser

---

### Option 1: One-Click Launch (Recommended for Windows)

From the project root or the `scripts/` directory:

* **Double-click** `scripts/start_companion.bat`
  *OR*
* **Run in PowerShell**:
  ```powershell
  .\scripts\start_companion.ps1
  ```

This automatically:
1. Starts the FastAPI backend on `http://127.0.0.1:8000`.
2. Starts the Vite frontend on `http://127.0.0.1:5173`.
3. Opens your default web browser to the companion interface.

---

### Option 2: Manual Terminal Setup

#### 1. Backend Setup

```bash
# 1. Navigate to backend
cd backend

# 2. Create and activate a virtual environment
# Windows PowerShell:
python -m venv venv
.\venv\Scripts\Activate.ps1

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure environment variables
copy .env.example .env
# Edit .env and supply your GROQ_API_KEY (https://console.groq.com/keys)

# 5. Start FastAPI server
python -m uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```
*Backend runs at `http://127.0.0.1:8000` with WebSocket at `ws://127.0.0.1:8000/ws/companion`.*

#### 2. Frontend Setup

```bash
# 1. In a separate terminal, navigate to frontend
cd frontend

# 2. Install npm dependencies
npm install

# 3. Launch Vite development server
npm run dev
```
*Frontend runs at `http://127.0.0.1:5173`.*

---

## Environment Variables Reference

Configure these in `backend/.env` or root `.env`:

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `GROQ_API_KEY` | `""` | Groq API Key for ultra-low latency LLM inference. |
| `GROQ_MODEL_NAME` | `openai/gpt-oss-120b` | Model identifier on Groq LPU endpoint. |
| `GEMINI_API_KEY` | `""` | Google Gemini API Key for conversational fallback. |
| `GEMINI_MODEL_NAME` | `gemini-3.5-flash-lite` | Google Gemini model identifier. |
| `NVIDIA_API_KEY` | `""` | NVIDIA NIM API Key. |
| `NVIDIA_MODEL_NAME` | `meta/llama-3.2-11b-vision-instruct` | NVIDIA Vision model identifier. |
| `EDGE_TTS_VOICE` | `en-IN-PrabhatNeural` | Microsoft Edge TTS voice persona. |
| `WHISPER_MODEL_SIZE` | `tiny.en` | Faster-Whisper local model size. |
| `PORT` | `8000` | FastAPI server port. |
| `HOST` | `0.0.0.0` | FastAPI host binding address. |

---

## WebSocket Event Protocol

All real-time communication flows over `ws://127.0.0.1:8000/ws/companion`:

| Direction | Event Type | Payload / Description |
| :--- | :--- | :--- |
| **Client -> Server** | `user_presence_frame` | Base64 JPEG frame for YOLOv8 presence & MediaPipe gesture recognition. |
| **Client -> Server** | `user_speech_transcription` | Spoken user prompt text and audio metadata. |
| **Client -> Server** | `user_interrupt` | Immediate gesture or button signal to abort active speech. |
| **Server -> Client** | `ai_state` | State transition event (`idle`, `listening`, `thinking`, `speaking`, `affirmation`, `sleep`, `error`). |
| **Server -> Client** | `ai_audio_response` | Streamed sentence audio chunk (Base64 MP3) + accumulated subtitle text. |
| **Server -> Client** | `perception_results` | YOLOv8 bounding boxes and recognized hand gestures with confidence scores. |

---

## Academic Report & Documentation

A complete academic review and project report detailing the theoretical foundations, architecture, and empirical latency benchmarks is available in [docs/AIML Report.pdf](docs/AIML%20Report.pdf).

---

## License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for more information.
