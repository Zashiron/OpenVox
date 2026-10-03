<img src="https://raw.githubusercontent.com/headlessripper/OpenVox/main/docs/assets/openvox-icon.png" alt="OpenVox" width="120" /># OpenVox

### The all‑in‑one, fully offline voice engine

Speech‑to‑text and text‑to‑speech that runs **entirely on your own hardware**.\
No cloud. No API keys. No per‑minute fees.

![Offline](https://img.shields.io/badge/100%25-Offline-1E6FB8?style=for-the-badge)

![Python 3.11+](https://img.shields.io/badge/Python-3.11+-16264F?style=for-the-badge)

![7 engines](https://img.shields.io/badge/Engines-7-2F86CF?style=for-the-badge)

![License](https://img.shields.io/badge/License-Zashiron%20v1.2-6FC0F0?style=for-the-badge)

[**Engines**](#-engines)  ·  [**Quickstart**](#-quickstart)  ·  [**Barge‑in**](#-streaming-tts-with-barge-in)  ·  [**Voice Agent**](#-llm-voice-agent)  ·  [**Roadmap**](#-roadmap)  ·  [**GitHub ↗**](https://github.com/headlessripper/OpenVox)

---

OpenVox is a complete voice stack, speech‑to‑text *and* text‑to‑speech, built for the places cloud voice services can't go: robots, embedded and edge devices, air‑gapped systems, and any product where audio must never leave the machine.

The goal is simple and ambitious: **match the quality of cloud services like ElevenLabs, but 100% offline**, then beat them on the things a cloud API structurally can't do, namely zero latency jitter, zero marginal cost, total privacy, and deep on‑device integration.

### Why OpenVox

|  | Cloud voice APIs | **OpenVox** |
| --- | --- | --- |
| **Connectivity** | Requires internet | **Fully offline / air-gapped** |
| **Cost** | Per-minute / per-character fees | **Zero marginal cost**, run it all day for free |
| **Privacy** | Audio leaves your device | **Audio never leaves the machine** |
| **Latency** | Network round-trip + jitter | **On-device, deterministic** |
| **Rate limits** | Throttled | **None** |
| **Deployment** | Someone else's servers | **Your robot, your edge box, your terms** |

A natural fit for **robotics, defense, medical, industrial, maritime, and privacy‑sensitive** applications, anywhere a device needs to hear and speak without phoning home.

---

## 🧩 Engines

OpenVox is one package, `openvox`, with each engine kept modular and independently installable so you only ship what a given device needs. Every engine sits behind a **swappable backend interface**, so the model underneath can be upgraded or replaced without touching your code.

| Engine | What it does | Status |
| --- | --- | --- |
| 🎙️ `openvox.stt` | Streaming speech‑to‑text: live partials, finals, word timestamps | ✅ Available |
| 🗣️ `openvox.tts` | Natural, human‑sounding text‑to‑speech | ✅ Available |
| ⚡ `openvox.tts` **· stream** | Real‑time streaming with instant `stop()` barge‑in | ✅ Available |
| 🎭 `openvox.clone` | Zero‑shot voice cloning from a short sample | ✅ Available |
| 🧬 `openvox.enroll` | A reusable, higher‑fidelity voice profile from several clips | ✅ Available |
| 🧼 `openvox.enhance` | Denoise, restore, and bandwidth‑extend a poor recording | ✅ Available |
| 🤖 `openvox.agent` | LLM voice‑agent loop: mic to STT to your LLM to streaming TTS, with barge‑in | ✅ Available |

---

## 🚀 Quickstart

```bash
git clone https://github.com/headlessripper/OpenVox.git
cd OpenVox

# speech-to-text + text-to-speech (CPU)
pip install -e ".[stt,stt-demo,tts]"

# stream a line and interrupt it after 1.5s to see barge-in
python -m openvox.tts.demo --text "This gets cut off partway through." --stream --interrupt-after 1.5
```

Each capability is an optional extra, so you install only what you need: `stt`, `stt-demo`, `tts`, `tts-gpu`, `clone`, `enroll`, `enhance`, `dev`.

---

## 🎙️ Speech‑to‑Text

Real‑time streaming transcription with live partials that firm up into finals, plus word‑level timestamps. Neural voice‑activity detection keeps it robust in noise, a sliding window bounds latency on long speech, and it runs CUDA‑accelerated with an automatic CPU fallback.

```bash
pip install -e ".[stt,stt-demo]"
python -m openvox.stt.demo                                  # live mic, clean transcript
python -m openvox.stt.demo --model base --file clip.mp3 --full   # any file, show live partials
```

```python
from openvox.stt import STTEngine

engine = STTEngine(model="distil-large-v3", device="cuda", language="en")

for event in engine.stream():                 # live from the microphone
    if event.is_partial:
        print("~", event.text, end="\r")       # firms up as you speak
    else:
        print("OK", event.text)                 # finalized line

result = engine.transcribe_file("clip.wav")   # or a whole file with timestamps
for word in result.words:
    print(f"  {word.word}: [{word.start:.2f}s, p={word.probability:.2f}]")
```

**Flags:** `--model` (`tiny`/`base`/`small`/`distil-large-v3`/`large-v3`) · `--device` (`cuda`/`cpu`) · `--language` · `--file` (any format/rate) · `--full`.

---

## 🗣️ Text‑to‑Speech

Genuinely human‑sounding speech, fully offline, with 28 built‑in English voices at 24 kHz. The engine auto‑selects the GPU when available and falls back to CPU.

```bash
pip install -e ".[tts]"       # CPU
pip install -e ".[tts-gpu]"   # NVIDIA GPU (bundles the CUDA 12 / cuDNN 9 runtime; no system CUDA needed)
```

```python
from openvox.tts import TTSEngine

engine = TTSEngine(voice="af_heart", device="cuda")   # falls back to CPU
engine.say("This runs entirely offline.")             # synthesize and speak
engine.synthesize("Save me to a file.").save_wav("out.wav")
engine.voices()                                       # list built-in voices
```

**Flags:** `--text` (required) · `--voice` · `--device` · `--speed` · `--out PATH` · `--no-play`.

---

## ⚡ Streaming TTS with Barge‑in

Speech should start almost immediately and be **interruptible the instant the user speaks**, which is essential for robots and interactive agents. OpenVox streams synthesized audio segment by segment and exposes a `stop()` that cuts playback within a single audio block.

The same call works for a built‑in voice **or a cloned voice profile**: pass `voice="af_heart"` or `voice="alice.ovx"`.

```python
from openvox.tts import TTSEngine

engine = TTSEngine(voice="af_heart", device="cuda")

# Stream chunks yourself (robot, socket, custom sink):
for chunk in engine.stream("Streamed as it is synthesized."):
    send_to_speaker(chunk.audio, chunk.sample_rate)

# Or play with barge-in support:
handle = engine.say_stream("I can be interrupted at any moment.")
handle.stop()     # cut audio within ~one audio block, safe to call from any thread
handle.wait()     # block until done (or already stopped)

# Stream in a cloned voice, same call:
engine.say_stream("Now in a cloned voice.", voice="alice.ovx")
```

`stop()` is signal‑driven and thread‑safe, so a future full‑duplex loop (listening while OpenVox speaks) simply calls `handle.stop()` when it hears the user.

**New flags:** `--stream` · `--interrupt-after SECONDS` · `--voice` also accepts an `.ovx` profile path.

---

## 🎭 Voice Cloning

Zero‑shot voice cloning: give a short reference clip and speak any text in that voice, fully offline (via [Chatterbox](https://github.com/resemble-ai/chatterbox), MIT). Every generated clip carries an imperceptible neural watermark for traceability.

```bash
pip install -e ".[clone]"
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121   # for NVIDIA GPU
```

```python
from openvox.clone import VoiceCloneEngine

engine = VoiceCloneEngine(device="cuda")
engine.clone("Speak this in my voice.", reference_audio="myvoice.mp3").save_wav("cloned.wav")

# Or clone from a saved profile (see Voice Enrollment), no reference clip needed:
engine.clone("Speak this in the enrolled voice.", profile="alice.ovx").save_wav("out.wav")
```

**Flags:** `--text` · `--ref PATH` · `--profile PATH` · `--exaggeration` · `--cfg` · `--device` · `--out` · `--no-play`.

---

## 🧬 Voice Enrollment

Zero‑shot cloning is only as good as one reference clip. **Enrollment** turns *several* clips of a voice into a saved, reusable **voice profile** (`.ovx`) that clones with materially higher, more consistent fidelity, and needs no reference clip at generation time.

Under the hood it builds a robust speaker representation from all the clips, then runs a speaker‑similarity‑guided search that optimizes the cloning conditioning to sound as close to the real voice as possible. No transcripts required.

```bash
pip install -e ".[enroll]"                # composes the clone + enhance engines
pip install resemble-enhance --no-deps
```

```python
from openvox.enroll import VoiceEnrollEngine

eng = VoiceEnrollEngine(device="cuda")
profile = eng.enroll(["clipA.wav", "clipB.wav", "long.m4a"])
print(profile.score)                      # achieved speaker-similarity
profile.save("alice.ovx")

# Use the profile anywhere a voice is accepted, cloning or streaming TTS.
```

**Flags:** `--in PATH [PATH ...]` · `--out PATH` · `--quality` (`fast`/`balanced`/`thorough`) · `--device` · `--no-enhance`. The optimization search runs on GPU; on a CPU‑only machine enrollment uses the robust‑baseline stage only.

---

## 🧼 Speech Enhancement

Restore a poorly‑recorded clip, denoise, enhance, and extend bandwidth (16 kHz to 44.1 kHz), fully offline (via [resemble‑enhance](https://github.com/resemble-ai/resemble-enhance), MIT). The cloner and enroller use it **automatically** to clean audio before use.

```bash
pip install -e ".[enhance]"
pip install resemble-enhance --no-deps
```

```python
from openvox.enhance import EnhanceEngine

engine = EnhanceEngine(device="cuda")
engine.enhance_file("poor.wav").save_wav("clean.wav")   # denoise + restore to 44.1 kHz
```

**Flags:** `--in PATH` · `--out PATH` · `--device` · `--denoise-only` · `--nfe`.

---

## 🤖 LLM Voice Agent

Drop any local LLM between OpenVox's ears and mouth and you have a complete, fully offline voice assistant: the agent listens on the mic, transcribes with `openvox.stt`, hands the text to **your** model, and speaks the reply back through streaming `openvox.tts`, **all on‑device**. While it speaks it keeps listening, so the moment the user starts talking it cuts itself off (barge‑in) and picks up the new turn. It keeps conversation history, survives a failing turn, and speaks in any built‑in **or cloned** voice.

Your LLM is just a callable, `respond(user_text, history) -> str | Iterator[str]`, so it plugs into anything. Yield the reply token‑by‑token and OpenVox starts speaking the first sentence while the rest is still generating. Helpers for a local **Ollama** or any **OpenAI‑compatible** server (llama.cpp, vLLM, LM Studio) are built in.

```bash
pip install -e ".[agent]"      # composes the stt + tts engines
```

```python
from openvox.agent import VoiceAgent
from openvox.agent.llm import ollama          # or openai_compatible(base_url=..., model=...)
from openvox.stt import STTEngine
from openvox.tts import TTSEngine

agent = VoiceAgent(
    llm=ollama(model="llama3.2"),             # any callable: respond(text, history) -> str | Iterator[str]
    stt=STTEngine(model="base"),
    tts=TTSEngine(voice="af_heart"),
    voice="af_heart",                          # a built-in voice, or an "alice.ovx" cloned profile
    barge_in=True,                             # interrupt playback the instant the user speaks
)
agent.run()                                    # mic -> STT -> LLM -> streaming TTS, until Ctrl-C
```

Or drive the whole loop from the command line:

```bash
python -m openvox.agent.demo --llm ollama --model llama3.2 --voice af_heart
```

**Flags:** `--llm` (`echo`/`ollama`/`openai`) · `--model` · `--base-url` · `--voice` (built‑in name or `.ovx`) · `--stt-model` · `--no-barge-in`.

---

## 🗺️ Roadmap

The vision in four horizons:

1. **Parity of plumbing.** An importable, offline library: streaming STT ✅, streaming TTS ✅, an LLM voice‑agent loop ✅, a headless daemon, and a ROS 2 node.
2. **Parity of quality.** A full model ladder, GPU / Jetson / ARM backends, punctuation, diarization, wake‑word, and command‑grammar biasing.
3. **Surpass the cloud.** On‑device voice cloning ✅, an LLM plug‑and‑play route ✅, a sub‑100 ms full‑duplex listen‑and‑speak loop, on‑device adaptive fine‑tuning, and mic‑array direction‑of‑arrival.
4. **Platform.** A community voice‑model hub, an eval harness proving OpenVox beats the cloud on real‑world audio, and a hardened cross‑platform SDK.

Next up: **full‑duplex** with echo cancellation (listen and speak at once, sub‑100 ms), and a **unified config route** to tweak every engine's parameters from one place.

---

## 🗂️ Design docs

OpenVox is built spec‑first. Full designs and step‑by‑step implementation plans for each engine live under `docs/superpowers/`, split into [specs](https://github.com/headlessripper/OpenVox/blob/main/docs/superpowers/specs/) and [plans](https://github.com/headlessripper/OpenVox/blob/main/docs/superpowers/plans/).

---

## 🤝 Contributing

Contributions are welcome: bug reports, feature ideas, and pull requests. Please open an issue to discuss any major change before starting work.

## 📜 License

Released under the **OpenVox Proprietary License (Zashiron License v1.2)**, see `LICENSE`. Commercial use requires written authorization.

<img src="https://raw.githubusercontent.com/headlessripper/OpenVox/main/docs/assets/openvox-logo.jpg" alt="OpenVox, Voice AI" width="280" />**OpenVox**  ·  hear and speak, entirely offline.
