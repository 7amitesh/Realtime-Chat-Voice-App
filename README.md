<h1 align="center">🎙️ Real-Time Multilingual ASR System</h1>

<p align="center">
<b>Enterprise-grade low-latency Automatic Speech Recognition platform with multilingual transcription, speaker diarization, Voice Activity Detection, and real-time streaming inference</b>
</p>

<p align="center">
<img src="assets/demo.png" alt="Realtime ASR Demo" width="730">
</p>

<p align="center">
<img alt="Python" src="https://img.shields.io/badge/python-3.11+-dark_green">
<img alt="FastAPI" src="https://img.shields.io/badge/backend-FastAPI-green">
<img alt="WebSocket" src="https://img.shields.io/badge/WebSocket-realtime-blue">
<img alt="PyTorch" src="https://img.shields.io/badge/PyTorch-2.0+-orange">
<img alt="Docker" src="https://img.shields.io/badge/Docker-ready-blue">
<img alt="GPU" src="https://img.shields.io/badge/GPU-supported-success">
<img alt="License" src="https://img.shields.io/badge/license-Apache_2.0-dark_green">
</p>

---

# 🔍 Overview

Real-Time Multilingual ASR System is a production-ready streaming speech recognition platform optimized for ultra-low-latency multilingual transcription and translation workloads.

Unlike traditional batch-based ASR pipelines that process complete utterances, this system implements intelligent streaming inference pipelines using transformer-based speech models with incremental decoding and adaptive buffering strategies.

The architecture supports:
- Real-time speech recognition
- Multilingual transcription
- Simultaneous translation
- Speaker diarization
- WebSocket streaming
- GPU accelerated inference
- Enterprise deployment workflows

---

# 🚀 Key Features

| Feature | Description |
|---|---|
| 🎤 Real-Time ASR | Ultra-low latency speech-to-text inference |
| 🌍 Multilingual Support | 200+ languages with automatic language detection |
| 🧠 Streaming Policies | SimulStreaming + LocalAgreement |
| 🔊 Voice Activity Detection | Silero VAD integration |
| 👥 Speaker Diarization | Streaming Sortformer + Diart |
| ⚡ GPU Acceleration | CUDA / MLX optimized inference |
| 🔌 API Compatibility | OpenAI-compatible REST + WebSocket APIs |
| 🐳 Deployment | Docker, Gunicorn, Nginx |
| 📊 Benchmarking | Multi-backend speed & accuracy evaluation |
| 🌐 Browser Streaming | Real-time frontend transcription interface |

---

# 🏗️ System Architecture

<p align="center">
<img src="assets/architecture.png" alt="System Architecture" width="700">
</p>

<p align="center">
<i>Scalable streaming architecture supporting concurrent users with adaptive buffering and intelligent audio chunk processing.</i>
</p>

```text
Audio Input (Mic / File / Stream)
        │
        ▼
┌────────────────────┐
│  Voice Activity    │
│  Detection (VAD)   │
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│ Intelligent Audio  │
│ Buffering Engine   │
└─────────┬──────────┘
          │
          ▼
┌─────────────────────────┐
│ Transformer ASR Models  │
│ Whisper / FasterWhisper │
│ MLX / Voxtral           │
└─────────┬───────────────┘
          │
          ▼
┌────────────────────┐
│ Speaker            │
│ Diarization        │
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│ FastAPI Backend    │
│ WebSocket + REST   │
└────────────────────┘
```

---

# 🧠 Research & Technologies

This project integrates ideas and techniques inspired by recent advancements in streaming speech recognition and multilingual inference systems.

### Core Technologies

- Transformer-based ASR models
- Simultaneous streaming inference
- Adaptive buffering policies
- Streaming speaker diarization
- Real-time Voice Activity Detection
- GPU optimized inference pipelines
- WebSocket streaming architecture

### Models & Frameworks

- Whisper
- Faster-Whisper
- MLX Whisper
- Voxtral Mini 4B
- Silero VAD
- Streaming Sortformer
- Diart
- FastAPI
- PyTorch
- HuggingFace Transformers

---

# ⚙️ Installation

## Basic Installation

```bash
pip install realtime-asr-system
```

## Development Setup

```bash
git clone https://github.com/7amitesh/Realtime-Multilingual-ASR-System.git

cd Realtime-Multilingual-ASR-System

pip install -r requirements.txt
```

---

# 🚀 Quick Start

## Start Server

```bash
rasr --model base --language en
```

## Multilingual Auto Detection

```bash
rasr --model medium --language auto
```

## Speaker Diarization

```bash
rasr --model medium --language en --diarization
```

## Translation Pipeline

```bash
rasr --model large-v3 --language fr --target-language en
```

## Voxtral Backend

```bash
rasr --backend voxtral
```

---

# 📄 File Transcription

```bash
# Transcribe audio
rasr transcribe meeting.wav

# Generate subtitles
rasr transcribe --format srt podcast.mp3 -o subtitles.srt
```

---

# 🔌 API Usage

## OpenAI-Compatible REST API

```bash
curl http://localhost:8000/v1/audio/transcriptions -F file=@audio.wav
```

---

## Python Client

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:8000/v1",
    api_key="unused"
)

with open("audio.wav", "rb") as f:
    transcript = client.audio.transcriptions.create(
        model="whisper-1",
        file=f
    )

print(transcript.text)
```

---

## WebSocket Streaming

```python
import asyncio
import websockets

async def stream_audio():

    async with websockets.connect(
        "ws://localhost:8000/asr"
    ) as ws:

        with open("audio.wav", "rb") as f:
            while chunk := f.read(4096):
                await ws.send(chunk)

        async for message in ws:
            print(message)

asyncio.run(stream_audio())
```

---

# ⚡ FastAPI Integration

```python
from whisperlivekit import AudioProcessor, TranscriptionEngine
from fastapi import FastAPI, WebSocket

app = FastAPI()

engine = TranscriptionEngine(
    model_size="medium",
    diarization=True,
    lan="en"
)

@app.websocket("/asr")
async def websocket_endpoint(websocket: WebSocket):

    processor = AudioProcessor(
        transcription_engine=engine
    )

    results = await processor.create_tasks()

    await websocket.accept()

    while True:
        audio = await websocket.receive_bytes()
        await processor.process_audio(audio)
```

---

# 📊 Benchmarks

<p align="center">
<img src="assets/benchmark_scatter_en_aware.png" alt="English Benchmark" width="700">
</p>

<p align="center">
<img src="assets/benchmark_scatter_fr_aware.png" alt="French Benchmark" width="700">
</p>

<p align="center">
<i>Speed vs accuracy benchmark comparisons across multilingual streaming inference backends.</i>
</p>

## Run Benchmarks

```bash
rasr bench

rasr bench --backend faster-whisper --model large-v3

rasr bench --languages all --json results.json
```

---

# 🐳 Docker Deployment

## GPU Deployment

```bash
docker build -t realtime-asr .

docker run \
  --gpus all \
  -p 8000:8000 \
  realtime-asr
```

---

## CPU Deployment

```bash
docker build -f Dockerfile.cpu -t realtime-asr .

docker run -p 8000:8000 realtime-asr
```

---

# 🌐 Production Deployment

## Gunicorn + Uvicorn

```bash
pip install uvicorn gunicorn

gunicorn \
  -k uvicorn.workers.UvicornWorker \
  -w 4 \
  your_app:app
```

---

## Nginx Reverse Proxy

```nginx
server {

    listen 80;
    server_name your-domain.com;

    location / {

        proxy_pass http://localhost:8000;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
}
```

---

# 📦 Configuration Parameters

| Parameter | Description |
|---|---|
| `--model` | Whisper model size |
| `--language` | Language or auto detection |
| `--target-language` | Translation target |
| `--diarization` | Enable speaker diarization |
| `--backend` | ASR backend selection |
| `--backend-policy` | Streaming inference policy |
| `--frame-threshold` | Latency/accuracy tradeoff |
| `--host` | Server host |
| `--port` | Server port |

---

# 🎯 Use Cases

- 🎤 Live meeting transcription
- 🌍 Multilingual conferencing
- ♿ Accessibility captioning systems
- 🎙️ Podcast/video subtitle generation
- 📞 Customer support call analytics
- 🧠 Voice AI systems
- 📡 Streaming ASR infrastructure
- 🏢 Enterprise transcription platforms

---

# 🛠️ Tech Stack

## AI / Deep Learning
- Whisper
- Faster-Whisper
- MLX Whisper
- Voxtral 4B
- PyTorch
- HuggingFace Transformers

## Backend
- FastAPI
- WebSockets
- Gunicorn
- Uvicorn

## Infrastructure
- Docker
- Nginx
- CUDA
- MLX

---

# 🔄 Current Development

- Real-time translation optimization
- GPU memory optimization
- Streaming latency reduction
- Distributed inference pipelines
- Advanced speaker-aware transcription
- Kubernetes deployment workflows
- Multi-GPU scaling
- Browser-native streaming SDK

---

# 👨‍💻 Author

## Amitesh Kumar

<p align="left">
<a href="https://github.com/7amitesh">
<img src="https://img.shields.io/badge/GitHub-7amitesh-black?logo=github">
</a>

<a href="https://linkedin.com">
<img src="https://img.shields.io/badge/LinkedIn-Profile-blue?logo=linkedin">
</a>

<a href="mailto:your-email@gmail.com">
<img src="https://img.shields.io/badge/Email-Contact-red?logo=gmail">
</a>
</p>

---

# 📄 License

Apache 2.0 License

See the LICENSE file for details.
