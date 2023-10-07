# كلامي · Kalami — Arabic Voice Translator

<div align="center">

بِسْمِ اللَّهِ الرَّحْمَنِ الرَّحِيمِ

**"My Speech" — Arabic → French & English voice translation**

![Version](https://img.shields.io/badge/version-2.1.0-gold)
![License](https://img.shields.io/badge/license-MIT-teal)
![Browser](https://img.shields.io/badge/browser-Chrome%20%7C%20Edge-blue)
![No API Key](https://img.shields.io/badge/API%20key-none%20required-green)
![Tests](https://img.shields.io/badge/tests-50%20passing-brightgreen)

</div>

---

## ✨ What is Kalami?

**Kalami** (كلامي — *"My Speech"*) is a fully browser-native Arabic voice translator.
Speak Arabic → hear it in French and English. No server. No API key. No data stored.
Open the HTML file and speak.

---

## 🚀 Quick Start

```bash
# 1. Clone
git clone https://github.com/yourname/kalami.git
cd kalami

# 2. Open in Chrome or Edge (required for Web Speech API)
open kalami-v2.1.html          # macOS
start kalami-v2.1.html         # Windows
xdg-open kalami-v2.1.html      # Linux

# Or serve locally (recommended for mic access on some systems)
python3 -m http.server 8080
# Then visit: http://localhost:8080/kalami-v2.1.html
```

> ⚠️ **Chrome or Edge only** — Firefox and Safari do not support `ar-SA` Web Speech Recognition.

---

## 📁 Repository Structure

```
kalami/
├── kalami-v2.1.html       # ★ Main application (all-in-one, ~1600 lines)
├── kalami-tests.html      # Automated test suite (50 tests, 7 suites)
├── help.html              # Visual help guide (Alhambra Islamic theme)
├── faq.html               # FAQ & troubleshooting (Medina Islamic theme)
├── README.md              # This file
├── CHANGELOG.md           # Full version history with commit messages
├── .gitignore             # Standard ignores
└── LICENSE                # MIT
```

---

## 🎙️ Features

### Input
| Feature | Detail |
|---|---|
| 🎙️ Arabic Speech | Web Speech API, browser-native |
| 🏷️ 5 Dialects | MSA/Gulf 🇸🇦 · Egyptian 🇪🇬 · Maghrebi 🇲🇦 · Levantine 🇸🇾 · Iraqi 🇮🇶 |
| ✏️ Editable Transcript | Fix recognition errors before translating |
| ⌨️ Arabic Keyboard | 28 letters + numerals + punctuation + translate key |

### Translation Modes
| Mode | How |
|---|---|
| ✋ Manual | Click start → speak → click stop → translate |
| 🔄 Continuous | Mic stays open, translates each pause |
| ⚡ Near-Live | Translates every phrase chunk ~2–3s after spoken |

### Output
| Feature | Detail |
|---|---|
| 🇫🇷 French + 🇬🇧 English | Parallel translation, side-by-side panels |
| 🔊 TTS Playback | Web Speech Synthesis, all browser voices |
| 🎚️ Speech Rate | 0.5× – 2.0× per language |
| 🔊 Speak Selector | FR only / EN only / Both |
| ⚡ Translation Memory | Instant repeat (in-session cache) |
| ↩️ Back-Translation | FR/EN → Arabic verify toggle |

### Session
| Feature | Detail |
|---|---|
| ⭐ Star entries | Bookmark important translations |
| 🔍 Search history | Filter per panel in real time |
| 📋 Copy / 🔁 Replay | Per panel |
| 🖥️ Fullscreen | Expand any panel to full viewport |
| 📄 Export TXT | Full session download |
| 📑 Export PDF | Bilingual, print-ready |
| 🔡 Font slider | Scale all text |
| 🗑️ Clear | Wipe session + audio queue |

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|---|---|
| `Space` | Start / Stop mic |
| `T` | Translate |
| `C` | Clear history |
| `E` | Export TXT |
| `K` | Toggle Arabic keyboard |
| `F` | Fullscreen French panel |
| `G` | Fullscreen English panel |
| `Esc` | Close fullscreen |

---

## 🔧 Architecture

```
Arabic Speech (mic)
      │
      ▼
Web Speech API  (ar-SA / ar-EG / ar-MA / ar-SY / ar-IQ)
      │
      ▼
Arabic Transcript  (editable, contenteditable RTL)
      │
      ├── Translation Memory hit? ──→ instant result (no API)
      │         │ miss
      ▼         ▼
  MyMemory ar→fr        MyMemory ar→en
  [parallel Promise.all]
      │                       │
      ▼                       ▼
🇫🇷 French Panel         🇬🇧 English Panel
  Stack · Star · Search    Stack · Star · Search
  TTS · Copy · PDF         TTS · Copy · PDF
            │
            ▼
    Audio Queue Engine
    FIFO — no TTS overlap
    enqueue → process → chain onend
```

---

## 🧪 Testing

Put `kalami-tests.html` in the same folder as `kalami-v2.1.html`, open in Chrome, click **▶ Run All Tests**.

```
50 tests · 7 suites

🏗️ DOM Structure            15   Elements, keyboard, waveform bars
🧠 State & Logic            15   Initial state, all toggles, modes
⚙️  Core Functions           17   esc(), translate, memory, queue
⌨️  Arabic Virtual Keyboard   8   Letters, numerals, actions
🌐 Translation & API          7   MyMemory, back-trans, errors
📤 Export & Session           6   TXT/PDF, session log, clear
♿ Accessibility               5   XSS, RTL, empty input, ranges
```

---

## 🌐 External Services

| Service | Purpose | Cost | Key |
|---|---|---|---|
| [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API) | Arabic recognition | Free | None |
| [MyMemory](https://mymemory.translated.net/) | Translation (5000 chars/day free) | Free | None |
| [Web Speech Synthesis](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis) | FR/EN voice output | Free | None |
| [Google Fonts](https://fonts.google.com/) | Amiri · Syne · DM Mono · Cormorant | Free | None |

---

## 🌙 Near-Live Mode — How It Works

Chrome fires `isFinal: true` recognition events at natural speech pauses (~2–4 words).
Near-Live hooks into these to translate + speak each chunk via the **Audio Queue Engine**.

```
Speech pause detected     0ms
MyMemory API response  ~400–900ms
TTS utterance starts   ~100ms
────────────────────────────────
Perceived lag          ~2–3 seconds per phrase
```

---

## 📜 Changelog

See [CHANGELOG.md](./CHANGELOG.md) for full version history and all commit messages.

| Version | Date | Highlights |
|---|---|---|
| `v2.1.0` | 2025-03-07 | Near-live mode · audio queue · speak selector · docs |
| `v2.0.0` | 2025-03-06 | Arabic keyboard · memory · back-trans · PDF · shortcuts |
| `v1.0.0` | 2025-03-05 | Initial release · speech → FR/EN · manual/continuous |

---

## 🕌 Design Inspiration

- **Bismillah** — Naskh calligraphy tradition (Amiri font by Khaled Hosny)
- **Alhambra Palace** — 8-point Islamic star tile patterns (`help.html`)
- **Medina Al-Munawwarah** — dome arch silhouettes, circle geometric motifs (`faq.html`)
- **Islamic calligraphy** — Quranic ayat and Arabic proverbs as section dividers

---

## 📄 License

MIT — free to use, modify, and distribute.

---

## 🤲 Credits

- **MyMemory** — Free translation API by [Translated.net](https://translated.net)
- **Amiri Font** — Beautiful Naskh Arabic typeface by [Khaled Hosny](https://aliftype.com)
- **Web Speech API** — Google Chrome speech recognition engine

---

<div align="center">
بارك الله فيكم · <em>May Allah bless you</em>
</div>
