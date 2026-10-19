# Voice → text for Claude, locally, $0

[Українська](../voice-local.md) | **English**

Two jobs, one script. First: an interview trainer in a foreign language. Claude asks a question in Polish or English,
I answer by voice, and Claude picks up the answer on its own and reviews it. Second: dumping thoughts into the chat
in Ukrainian instead of typing.

Audio never leaves the laptop, no API key, the model runs on the laptop CPU.

## What it gave us (measured on my laptop, October 2026)

- **$0** and no key. You don't borrow someone else's key (a client's, an employer's) for personal tasks.
- **~1 s of processing per 1 s of speech** (faster-whisper `large-v3-turbo`, CPU, int8). A 17-minute file takes ~8 min.
- **~940 MB RAM** while the window is open. Keep the window open all day and the model loads once.
- 30 s of speech ≈ 70 words. At ~35 words a minute that's ~2 min of typing: voice saves ~1.5 min on every long
  message (an estimate, not a measurement).
- First evening with the trainer: 9 reviewed answers in Polish on 3 concepts, scores from 3 to 7 out of 10.

## How it works

1. A desktop shortcut starts the script with a language (`pl`, `en`, `uk`).
2. Enter = start, Enter = stop. The script writes the text to a file and puts it on the clipboard.
3. **Dump:** Ctrl+V into the chat. **Trainer:** Claude waits in the background for a new file in the recordings folder
   (a command that returns when a file appears) and wakes up on its own. No need to type "done".

```python
# pip install faster-whisper sounddevice numpy ; ffmpeg isn't needed for the microphone
import sys, time, threading, subprocess
from datetime import datetime
from pathlib import Path
import numpy as np, sounddevice as sd
from faster_whisper import WhisperModel

LANG = sys.argv[1] if len(sys.argv) > 1 else "pl"
OUT = Path(sys.argv[2] if len(sys.argv) > 2 else "voice")
PROMPT = {"pl": "Rozmowa rekrutacyjna: n8n, cron, retry, kolejka, REST API, JSON, webhook, Docker.",
          "en": "Speaking practice: automation, workflow, cron, retry, queue, webhook, JSON, prompt, LLM.",
          "uk": "Нотатка для Claude: n8n, Claude Code, бот, сервер, клієнт."}[LANG]
RATE = 16000

def record():
    chunks, stop = [], threading.Event()
    with sd.InputStream(samplerate=RATE, channels=1, dtype="float32",
                        callback=lambda d, *_: chunks.append(d.copy())):
        threading.Thread(target=lambda: (input(), stop.set()), daemon=True).start()
        while not stop.is_set():
            time.sleep(0.2)
    return np.concatenate(chunks)[:, 0] if chunks else np.zeros(0, dtype=np.float32)

sys.stdout.reconfigure(encoding="utf-8")  # otherwise Cyrillic crashes on a Windows console (cp1250)
model = WhisperModel("large-v3-turbo", device="cpu", compute_type="int8")
OUT.mkdir(parents=True, exist_ok=True)
while input("\nEnter = record, q = quit: ").strip().lower() != "q":
    audio = record()
    if audio.size < RATE or np.abs(audio).max() < 0.003:  # room silence 0.002-0.008: mic is off
        print("Empty recording, check the mic."); continue
    segs, _ = model.transcribe(audio, language=LANG, vad_filter=True, initial_prompt=PROMPT)
    text = " ".join(s.text.strip() for s in segs).strip()
    if not text:
        print("No words heard."); continue
    (OUT / f"{datetime.now():%Y-%m-%d_%H%M%S}.txt").write_text(text + "\n", encoding="utf-8")
    subprocess.run("clip", input=text.encode("utf-16le"))  # Windows; on Mac: pbcopy with utf-8
    print(text)
```

A prompt for Claude to make the trainer work: "Ask me one question in Polish, like in an interview. I answer by voice,
the file appears in the `voice` folder. Wait for a new file in the background, then review it: a score out of 10,
what's good, what's missing (1-3 points), one language fix. If a word looks odd, ask whether I really said it first."

## Traps we hit

1. **Whisper invents words in silence.** "Dz 34 huertos… Надо" showed up at the end of a recording while I was quiet.
   Fixed with `vad_filter=True` (cuts silence before the model) and an "empty" loudness threshold.
2. **The language drifts.** Czech words ("všechny") appeared in the middle of a Polish answer. Set the language
   explicitly (`language=`) and put a glossary of the topic's terms into `initial_prompt`: the model leans toward
   words it saw in the prompt.
3. **The model hears something else.** I said "stuknie", Whisper wrote "stupnia", and the AI started correcting
   grammar I never used. Rule for the AI: ask about an odd word first, correct after.
4. **The audio decoder for files crashes.** In faster-whisper 1.2.1 the bundled PyAV crashed on `metadata_errors`.
   For files (call recordings) decode audio with ffmpeg; from the microphone you get the array directly.
5. **Windows clipboard and BOM.** `clip` with `utf-16` puts an invisible character at the start. Use `utf-16le`.

## Who it's for

- Anyone preparing for an interview in Polish or English who wants every answer reviewed, not just "well done".
- Anyone who types into the chat slowly and thinks faster than they type.

## When NOT to use it

- If you talk to Claude in English in the terminal: Claude Code has a built-in `/voice` (hold the spacebar).
  Judging by open requests it doesn't support Ukrainian yet, and we didn't find voice in the VS Code extension.
- A weak laptop (under 8 GB RAM): take the `small` model, but Ukrainian and Polish get noticeably worse.
- If you need "who said what" on a call: Whisper doesn't separate speakers (see Whisper + Craig in the
  "what we tried" table).
