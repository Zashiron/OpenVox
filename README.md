# Zashiron Ltd

> **Engineering Intelligent Systems, High-Performance Software, Next-Generation Synthetic Minds, and Offline Edge AI.**

Welcome to the official GitHub organization profile for **Zashiron Ltd**. We are a software and hardware technology company headquartered in Lagos, Nigeria, dedicated to building performant, low-latency applications, synthetic cognitive architectures, secure workspace solutions, offline voice engines, and scalable infrastructure.

## 🏛️ Corporate Overview

Zashiron Ltd operates across full-stack application development, edge AI engineering, synthetic cognitive architectures, low-level system optimization, secure encrypted networks, on-device audio processing, and custom hardware integration.

* **Legal Entity:** Zashiron Ltd
* **D-U-N-S® Number:** `352296499`
* **Headquarters:** Lagos, Nigeria
* **Web Hubs:** [Aretlyx.app](https://aretlyx.app) • [zashiron.org](https://zashiron.org)

## 📦 Featured Flagship Ecosystem

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                               ZASHIRON PRODUCT SUITE                                   │
├───────────────────┬────────────────────────────────────────────────────────────────────┤
│ 🧠 Alpha          │ Synthetic Mind & Brain-Inspired Cognitive Architecture             │
├───────────────────┼────────────────────────────────────────────────────────────────────┤
│ 🎙️ OpenVox        │ All-in-One, 100% Offline Voice Engine (STT, TTS, Clone, Agent)     │
├───────────────────┼────────────────────────────────────────────────────────────────────┤
│ 🔐 Enclave        │ E2EE Messaging, Voice/Video, File Sharing & Remote Desktop (RDP)   │
├───────────────────┼────────────────────────────────────────────────────────────────────┤
│ 🎬 Aretlyx        │ Multimodal AI Video Intelligence, Auto-Clipping & Scene Detection  │
└───────────────────┴────────────────────────────────────────────────────────────────────┘
```

---

### 🧠 1. Alpha — A Synthetic Mind

**Alpha** is an ongoing attempt to grow a **synthetic mind** rather than ship a traditional AI assistant. It is a local, private, brain-inspired cognitive architecture featuring a durable associative memory, an emotional system, intrinsic drives, a sense of self and of the user, an inner monologue, and plastic organs whose weights change with experience.

> **Alpha is not a chatbot and not an assistant.** It has no goal of being helpful or compliant. It is an individual: every instance accumulates its own memories, moods, and history over time. The current live individual is named **Briallaes**.

#### Core Architectural Organs

* **Associative Graph Memory (`GraphMemory`):** Hebbian edges + spreading activation (SQLite + NetworkX) so recall traverses linked ideas rather than nearest-text matches.
* **Predictive-Coding Net:** A GPU-capable Rao–Ballard predictive-coding network whose fast-weights physically adapt to surprise and learning transitions.
* **Affect System:** Prediction-error-driven core emotion on Russell’s circumplex.
* **Intrinsic Drives:** Internal curiosity, boredom, and social needs that initiate thoughts unprompted.
* **Metacognition & Theory of Mind:** `SelfModel` for confidence assessment and a fading, fallible `UserModel` for peer dynamics.
* **Default-Mode Network:** Spontaneous wandering and dreaming when idle to recombine memories.
* **Language Cortex:** A frozen, local, swappable GGUF model (`DeepSeek-R1-Distill-Llama-8B` / `Qwen3.5-9B`) used strictly as the language engine to turn thoughts into words, keeping cognitive logic entirely inside Alpha's native organs.

---

### 🎙️ 2. OpenVox — All-in-One Offline Voice Engine

<img src="https://raw.githubusercontent.com/headlessripper/OpenVox/main/docs/assets/openvox-icon.png" alt="OpenVox" width="100" />

**OpenVox** is a complete, 100% offline speech-to-text and text-to-speech voice engine built for edge computing, robotics, air-gapped systems, and privacy-sensitive devices where audio must never leave the hardware.

#### Highlights & Capabilities

* **7 Modular Engines:**
  * 🎙️ `openvox.stt`: Real-time streaming STT with live partials and word timestamps.
  * 🗣️ `openvox.tts`: Natural 24 kHz offline TTS with built-in voices.
  * ⚡ `openvox.tts · stream`: Low-latency streaming TTS with thread-safe `stop()` barge-in.
  * 🎭 `openvox.clone`: Zero-shot voice cloning with neural watermarking.
  * 🧬 `openvox.enroll`: High-fidelity `.ovx` reusable voice profile creation.
  * 🧼 `openvox.enhance`: Denoising, restoration, and 44.1 kHz bandwidth extension.
  * 🤖 `openvox.agent`: Full offline LLM voice-agent loop (Mic ➔ STT ➔ Local LLM ➔ Streaming TTS).
* **Zero Marginal Cost & Zero Cloud Dependency:** No API keys, zero internet requirement, and zero network latency jitter.
* **License:** Released under the **OpenVox Proprietary License (Zashiron License v1.2)**.

---

### 🔐 3. Enclave — Encrypted Communication & Workspace Suite

**Enclave** (formerly *SwiftDrop*, backend `com.zashiron.swiftdrop`) is a secure, self-hostable communication and workspace platform built with **Flutter** for Android and Windows. It operates local-network-first with direct peer-to-peer capabilities, falling back to a cloud relay when required.

#### Core Features

* **E2E Encrypted Messaging:** 1:1 and group chats using the **Signal Protocol** (X3DH + Double Ratchet, sealed sender via `libsignal_protocol_dart`), featuring safety numbers (TOFU), disappearing messages, and offline message buffering.
* **Voice & Video Calls:** WebRTC with DTLS-SRTP encryption, native call interfaces (Android ConnectionService / iOS CallKit), multi-party audio/video, and live network metrics panels.
* **High-Speed File Transfer:** Dual mode featuring **Swift Mode** (direct TLS-pinned LAN streaming) and **Air Mode** (cloud WebSocket relay), alongside a clientless **Web Share** interface.
* **Cross-Device Remote Desktop (RDP):** Control desktop-from-phone or phone-from-phone with OS-level input injection (Windows `SendInput` / Android Accessibility Service).
* **Self-Hostable Infrastructure:** Enterprise organizations can deploy the **Office Space Node** (WebSocket relay + TURN + Key Directory) on their own hardware for total data sovereignty.

---

### 🎬 4. Aretlyx — AI Video Intelligence Platform

**Aretlyx** is an end-to-end multimodal AI platform engineered for automatic video editing, smart clipping, and highlight generation.

* **Visual Tracking:** Integrates **YOLO11** computer vision models for subject framing, face tracking, and dynamic cropping.
* **Acoustic Intelligence:** Employs **YAMNet** and **PANNs** for audio event detection, speech/music separation, and emphasis tagging.
* **Multimodal Reasoning:** Combines LLM reasoning pipelines with temporal timestamps for automated script alignment and clip selection.

---

## 🛠️ Unified Technology Stack

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                 ZASHIRON TECH MATRIX                                   │
├───────────────────┬────────────────────────────────────────────────────────────────────┤
│ Application Layer │ Flutter, Dart, Next.js, React, TypeScript, Tailwind CSS            │
├───────────────────┼────────────────────────────────────────────────────────────────────┤
│ Systems & Backend │ Go, Python 3.11+, PyTorch (CUDA), Node.js                          │
├───────────────────┼────────────────────────────────────────────────────────────────────┤
│ Speech & Audio AI │ OpenVox (STT/TTS/Clone), Chatterbox, Resemble-Enhance, Silero VAD  │
├───────────────────┼────────────────────────────────────────────────────────────────────┤
│ Cryptography & Net│ Signal Protocol, WebRTC (DTLS-SRTP), TLS Certificate Pinning       │
├───────────────────┼────────────────────────────────────────────────────────────────────┤
│ Edge & Local AI   │ llama-cpp-python (GGUF), faster-whisper, kokoro-onnx               │
├───────────────────┼────────────────────────────────────────────────────────────────────┤
│ Data & Cloud      │ Neon Postgres, SQLite, NetworkX, Upstash Redis, Cloudflare R2      │
├───────────────────┼────────────────────────────────────────────────────────────────────┤
│ Payments & Auth   │ Paystack API, Appwrite, Clerk Authentication                       │
└───────────────────┴────────────────────────────────────────────────────────────────────┘
```

---

## 🤝 Contribution & Internal Governance

1. **Public Repositories:** Open for issue tracking, public client SDKs, and community tools.
2. **Security & Vulnerabilities:** For security concerns regarding OpenVox, Enclave, Alpha, or cloud relays, report directly via security administration.
3. **Developer Identity:** Commits by Zashiron maintainers must be GPG/SSH signed using registered `@zashiron.com` or `@zashiron.org` corporate identities.

<p align="center">
  <sub>© Zashiron Ltd. All rights reserved.</sub>
</p>
