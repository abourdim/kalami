# Changelog · كلامي Kalami

All notable changes to this project are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) and [Conventional Commits](https://www.conventionalcommits.org/).

---

## [2.1.0] — 2025-03-07

### Commit Message

```
feat(kalami): release v2.1 — near-live interpretation + full documentation

بِسْمِ اللَّهِ الرَّحْمَنِ الرَّحِيمِ

Near-live phrase-by-phrase interpretation mode, audio queue engine,
per-session speak language selector, full test suite (50 tests),
and complete documentation package (README, help, FAQ, CHANGELOG).
```

### Added

#### ⚡ Near-Live Interpretation
- Hooks into `SpeechRecognition` `isFinal` chunk events during Manual listening
- Each phrase chunk (~2–3 word pause) triggers immediate translate + TTS
- Expected latency: ~2–3 seconds per phrase from speech to audio output
- `Live` badge pulses in status bar during chunk processing
- `translateChunkLive()` function — silent error handling, never breaks listening flow
- Works independently of Continuous mode

#### 🔊 Speak Language Selector
- Three-button group: 🇫🇷 FR / 🇬🇧 EN / Both
- Controls TTS output for Near-Live, Auto-play, ▶ Play, and 🔁 Replay
- `state.speakLang` — persists for full session
- `speakByLang(frText, enText)` helper respects selector at call time

#### 🎵 Audio Queue Engine
- `enqueueSpeak()` — adds utterance to queue, triggers processing if idle
- `processAudioQueue()` — dequeues and speaks one at a time, chains via `onend`
- `clearAudioQueue()` — empties queue, cancels speechSynthesis, resets flag
- `state.audioQueue []` + `state.audioPlaying` flag prevent overlap
- Queue auto-clears on Stop and Clear button

#### 📚 Documentation
- `README.md` — full feature reference, API table, architecture diagram, shortcuts
- `help.html` — step-by-step visual guide, Alhambra Islamic geometric theme
- `faq.html` — 18 FAQ entries, accordion UI, Medina/calligraphy Islamic theme
- `CHANGELOG.md` — this file

#### 🧪 Test Suite (`kalami-tests.html`)
- 50 automated tests, 7 suites, runs inside hidden iframe
- Suites: DOM Structure · State & Logic · Core Functions · Arabic Keyboard · Translation & API · Export & Session · Accessibility
- API mocked via `w.fetch` override pattern
- Assertion helpers: `assert()`, `assertEqual()`, `assertContains()`, `skip()`

### Changed
- `autoPlayBoth()` now delegates to `speakByLang()` via audio queue
- `speak()` kept for direct one-off calls (replay, play buttons)

### Fixed
- TTS overlap when near-live chunks arrive faster than speech finishes

---

## [2.0.0] — 2025-03-06

### Commit Message

```
feat(kalami): release v2.0 — full featured dashboard with 11 improvements

Arabic virtual keyboard, translation memory, back-translation verify,
speech rate sliders, dialect selector, search history, star/bookmark,
bilingual PDF export, font size slider, keyboard shortcuts, fullscreen mode.
```

### Added

#### ✏️ Editable Transcript
- `transcriptBox` is `contenteditable="true"` with `dir="rtl"`
- Click to correct Arabic recognition errors before translating
- Placeholder hint fades on input

#### ⌨️ Arabic Virtual Keyboard
- Full 28-letter Arabic alphabet + لا ligature
- Eastern Arabic numerals ١–٩٠
- Arabic punctuation: ، ؟ !
- Action keys: ⌫ Del, ⌦ Word (delete last word), مسافة (space), ⚡ Translate
- Inserts at cursor position via `window.getSelection()` / `Range` API
- Toggled by button or `K` shortcut

#### ⚡ Translation Memory
- `state.translationMemory {}` — keyed by `text.toLowerCase()`
- Cache checked before every API call in `translateText()` and `translateChunkLive()`
- `⚡ From memory` badge flashes on cache hit
- Session-only; clears on page close

#### ↩️ Back-Translation Verification
- `fetchBackTranslation(frText, enText)` — parallel fetch fr|ar + en|ar
- Result shown under each entry as `backtrans-text` block
- Enabled via `↩ Verify` toggle (`state.backTrans`)
- Gated: skipped entirely when toggle is off

#### 🏷️ Dialect Selector
- `#dialectSelect` — MSA/Gulf `ar-SA`, Egyptian `ar-EG`, Maghrebi `ar-MA`, Levantine `ar-SY`, Iraqi `ar-IQ`
- Sets `rec.lang` on each recognition build
- Recognition rebuilt on every start to pick up dialect changes

#### 🎚️ Speech Rate Slider
- `#frRateSlider` + `#enRateSlider`, range 0.5–2.0, step 0.1
- Applied to `SpeechSynthesisUtterance.rate` at speak time

#### 🔍 Search History
- `#frSearch` + `#enSearch` text inputs
- `renderHistory()` filters by `item.text.toLowerCase().includes(term)`
- Independent per panel; state tracked in `state.frSearch` / `state.enSearch`
- Cleared on Clear button

#### ⭐ Star / Bookmark
- `item.starred` boolean on each history entry
- Click `.entry-star` → toggles `starred`, re-renders panel
- Starred entries: gold border + `⭐` icon
- Survives search filters (rendered in filtered set)

#### 📑 Export PDF
- Opens `window.open()` with formatted HTML document
- Includes Bismillah header, bilingual table, timestamps
- Triggers `window.print()` after 600ms for Save as PDF
- Amiri + Syne fonts loaded in print window

#### 🔡 Font Size Slider
- `#fontSlider` range 0.8–1.6
- Sets `--font-scale` CSS variable on `:root`
- All `font-size: calc(Xrem * var(--font-scale))` declarations scale uniformly

#### ⌨️ Keyboard Shortcuts
- `Space` — toggleMic()
- `T` — translate current transcript
- `C` — clear
- `E` — export TXT
- `K` — toggle Arabic keyboard
- `F` / `G` — fullscreen FR / EN
- `Esc` — close fullscreen
- Guard: disabled when focus inside input / contenteditable

#### 🖥️ Fullscreen Mode
- `toggleFullscreen(lang)` adds `.fullscreen-mode` to panel
- Fixed positioning, z-index 500, full viewport
- `#fullscreenClose` button (✕) visible when active
- `document.body.style.overflow = 'hidden'` prevents scroll

### Changed
- `renderHistory()` now accepts `searchTerm` and `lang` params
- `addEntries()` assigns unique numeric `id` via `entryCounter`
- Replay buttons enabled after first `addEntries()` call

---

## [1.0.0] — 2025-03-05

### Commit Message

```
feat(kalami): initial release — Arabic voice translator v1.0

Single HTML file. Arabic speech → FR + EN translation + TTS.
Manual + Continuous modes, auto-play, voice selector, copy,
clear history, export TXT, replay last, waveform animation.
بِسْمِ اللَّهِ الرَّحْمَنِ الرَّحِيمِ
```

### Added

#### Core
- Arabic speech recognition via Web Speech API (`ar-SA`)
- MyMemory free API translation: ar→fr + ar→en (parallel)
- Web Speech Synthesis output for FR + EN
- Single `kalami-v1.html` — zero dependencies, zero build

#### Input
- 🎙️ Mic button with pulse animation
- 11-bar animated waveform (CSS keyframes)
- Arabic transcript display (Amiri font, RTL)
- Status bar with color states: listening / processing / ready / error

#### Modes
- ✋ Manual mode: click start → speak → click stop → translate
- 🔄 Continuous mode: mic stays open, translates each pause
- Toggle switch in controls bar with mode badge

#### Output
- 🇫🇷 French panel + 🇬🇧 English panel (side by side)
- Translations append newest-on-top in continuous mode
- ▶ Play button per panel
- 🔁 Replay last button per panel
- 📋 Copy latest to clipboard
- Voice selector dropdown (all installed browser voices)

#### Session
- ⏸️ Auto-play toggle (speaks both languages after each translation)
- 🗑️ Clear history button (wipes all entries)
- 📥 Export TXT (full session as `.txt` download)

#### Design
- Dark navy dashboard (`#05080f`)
- Amber/gold accent palette
- بِسْمِ اللَّهِ الرَّحْمَنِ الرَّحِيمِ — golden, centered, glowing header
- Amiri (Arabic) + Syne (UI) + DM Mono (mono) fonts
- Sticky header with backdrop blur
- Fade/slide animations on load

---

## Browser Support

| Browser | Version | Status |
|---|---|---|
| Google Chrome | 33+ | ✅ Full support |
| Microsoft Edge | 79+ | ✅ Full support |
| Firefox | any | ❌ No Web Speech Recognition |
| Safari | any | ❌ No Web Speech Recognition |

## External Dependencies (all free, no key)

| Service | Used For |
|---|---|
| [MyMemory API](https://mymemory.translated.net/) | Text translation (5000 chars/day free) |
| [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API) | Arabic speech recognition |
| [Web Speech Synthesis](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis) | FR/EN voice output |
| [Google Fonts](https://fonts.google.com/) | Amiri, Syne, DM Mono, Cormorant Garamond |
