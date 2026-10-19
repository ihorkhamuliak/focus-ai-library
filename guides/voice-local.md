# Голос → текст для Claude локально, $0

**Українська** | [English](en/voice-local.md)

Дві задачі, один скрипт. Перша: тренажер співбесіди іноземною. Claude ставить питання польською чи англійською, я
відповідаю голосом, Claude сам бачить відповідь і розбирає її. Друга: дамп думок у чат українською замість набору.

Звук нікуди не йде, ключ API не потрібен, модель працює на процесорі ноута.

## Що це дало (заміри на моєму ноуті, жовтень 2026)

- **$0** і жодного ключа. Чужий ключ (клієнта, роботодавця) для особистих задач не береш.
- **~1 с обробки на 1 с мови** (faster-whisper `large-v3-turbo`, CPU, int8). Файл на 17 хв розшифровується за ~8 хв.
- **~940 МБ RAM**, поки вікно відкрите. Тримаєш вікно весь день, і модель вантажиться один раз.
- 30 с мови ≈ 70 слів. Якщо друкуєш ~35 слів за хвилину, це ~2 хв набору: голос економить ~1,5 хв на кожному
  довгому повідомленні (оцінка, не замір).
- Перший вечір тренажера: 9 розібраних відповідей польською на 3 поняття, оцінки від 3 до 7 з 10.

## Як влаштовано

1. Ярлик на робочому столі запускає скрипт з мовою (`pl`, `en`, `uk`).
2. Enter = старт, Enter = стоп. Скрипт пише текст у файл і кладе в буфер обміну.
3. **Дамп:** Ctrl+V у чат. **Тренажер:** Claude у фоні чекає на новий файл у теці записів (команда, що
   повертається, коли з'являється файл) і прокидається сам. Писати «готово» не треба.

```python
# pip install faster-whisper sounddevice numpy ; ffmpeg не потрібен для мікрофона
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

sys.stdout.reconfigure(encoding="utf-8")  # інакше кирилиця падає на консолі Windows (cp1250)
model = WhisperModel("large-v3-turbo", device="cpu", compute_type="int8")
OUT.mkdir(parents=True, exist_ok=True)
while input("\nEnter = запис, q = вихід: ").strip().lower() != "q":
    audio = record()
    if audio.size < RATE or np.abs(audio).max() < 0.003:  # тиша кімнати 0,002-0,008: мікрофон вимкнений
        print("Запис порожній, перевір мікрофон."); continue
    segs, _ = model.transcribe(audio, language=LANG, vad_filter=True, initial_prompt=PROMPT)
    text = " ".join(s.text.strip() for s in segs).strip()
    if not text:
        print("Слів не почуто."); continue
    (OUT / f"{datetime.now():%Y-%m-%d_%H%M%S}.txt").write_text(text + "\n", encoding="utf-8")
    subprocess.run("clip", input=text.encode("utf-16le"))  # Windows; на Mac: pbcopy з utf-8
    print(text)
```

Промпт для Claude, щоб тренажер працював: «Ставиш мені одне питання польською, як на співбесіді. Я відповідаю голосом,
файл з'являється в теці `voice`. Чекай на новий файл у фоні, розбери: оцінка з 10, що добре, чого бракує (1-3 пункти),
одна правка мови. Дивне слово спершу питай, чи я так сказав.»

## Пастки, на які ми наступили

1. **Whisper вигадує слова в тиші.** Наприкінці запису з'явилось «Dz 34 huertos… Надо», хоча я мовчав.
   Лікується `vad_filter=True` (вирізає тишу до моделі) і порогом «порожньо» на гучність.
2. **Мова пливе.** Посеред польської відповіді вилізли чеські слова («všechny»). Мову задаєш явно (`language=`),
   а в `initial_prompt` кладеш словник термінів теми: модель тягнеться до слів, які бачила в промпті.
3. **Модель чує не те, що ти сказав.** Я казав «stuknie», Whisper записав «stupnia», а AI почав виправляти мені
   граматику, якої я не робив. Правило для AI: дивне слово спершу питаєш, потім правиш.
4. **Декодер звуку з файлів падає.** У faster-whisper 1.2.1 вбудований PyAV падав на `metadata_errors`.
   Для файлів (записи дзвінків) звук декодуємо через ffmpeg, а з мікрофона беремо одразу масив.
5. **Буфер Windows і BOM.** `clip` з кодуванням `utf-16` кладе в буфер невидимий символ на початку. Треба `utf-16le`.

## Кому

- Хто готується до співбесіди польською чи англійською і хоче розбір кожної відповіді, а не «молодець».
- Хто довго набирає в чат і думає швидше, ніж друкує.

## Коли НЕ треба

- Якщо говориш з Claude англійською в терміналі: у Claude Code є вбудований `/voice` (затиснути пробіл).
  Українську він, судячи з відкритих запитів, поки не підтримує, а в розширенні VS Code голосу ми не знайшли.
- Слабкий ноут (менше 8 ГБ RAM): бери модель `small`, але українська й польська стануть помітно гіршими.
- Якщо треба розмітка «хто що сказав» на дзвінку: Whisper спікерів не розділяє (див. Whisper + Craig у таблиці
  «що ставили»).
