---
sidebar_label: "Быстрый старт"
sidebar_position: 1
---

# Быстрый старт

В этом разделе вы узнаете, как установить все нужные библиотеки и сможете проверить работу API на простом примере. Также, вы cможете ознакомиться с ограничениями на API.

:::tip[]
API совместим с OpenAI, так что если вы знакомы с пакетом `openai` для Python, Javascript или уже умеете делать запросы с помощью cURL, вы уже знаете, как пользоваться нашим API:) Главное, ознакомьтесь с ограничениями ниже.
:::

## 1. Получение API ключа

Прежде чем начать, вам нужно получить API-ключ. Зарегистируйтесь на [платформе](https://app.nexara.ru) и пополните счет. Если хотите получить бесплатные кредиты на тестирование API, напишите [Данилу](https://t.me/RND_RandoM) в Telegram.

## 2. Установка библиотек

**Python:**

```
pip install openai
```

**JavaScript (Node.js):**

```
npm install openai
```

## 3. Примеры кода

В примерах на API отправляется файл `example.mp3` и после обработки аудио в консоль выводится текст транскрипции.

**Python:**

```py
from openai import OpenAI

client = OpenAI(
    base_url="https://api.nexara.ru/api/v1",
    api_key="ВАШ_API_КЛЮЧ",
)

with open("example.mp3", "rb") as audio_file:
    transcription = client.audio.transcriptions.create(
        model="whisper-1",
        file=audio_file,
    )

print(transcription.text)
```

> **Вывод:** Восемь десятков и семь лет назад наши отцы образовали на этом континенте новую нацию...

**JavaScript:**

```js
const OpenAI = require("openai");

const openai = new OpenAI({
  baseURL: "https://api.nexara.ru/api/v1",
  apiKey: "ВАШ_API_КЛЮЧ",
});

async function transcribeAudio() {
  const fs = require("fs");

  const transcription = await openai.audio.transcriptions.create({
    model: "whisper-1",
    file: fs.createReadStream("example.mp3"),
  });

  console.log(transcription.text);
}

transcribeAudio();
```

> **Вывод:** Восемь десятков и семь лет назад наши отцы образовали на этом континенте новую нацию...

**cURL:**

```bash
curl --request POST \
  --url https://api.nexara.ru/api/v1/audio/transcriptions \
  --header 'Authorization: Bearer ВАШ_API_КЛЮЧ' \
  --header 'Content-Type: multipart/form-data' \
  --form model="whisper-1" \
  --form file="@example.mp3"
```

> **Вывод:** Восемь десятков и семь лет назад наши отцы образовали на этом континенте новую нацию...

## 4. Ограничения

Все файлы должны быть размером **до 1 ГБ**. Наш API поддерживает только следующие форматы аудиофайлов:

- **wav** (audio/wav, audio/x-wav, audio/wave)
- **mp3** (audio/mp3, audio/mpeg, audio/mpg, audio/x-mpeg)
- **m4a** (audio/x-m4a, audio/mp4, audio/mp4a-latm, audio/mpeg4, audio/aac)
- **flac** (audio/flac)
- **ogg** (audio/ogg, audio/oga)
- **opus** (audio/opus)

Также поддерживаются следующие форматы видеофайлов:

- **mp4** (video/mp4)
- **mov** (video/quicktime)
- **avi** (video/x-msvideo)
- **mkv** (video/x-matroska)

:::tip[Совет]
Для экономии трафика рекомендуется переводить видео в аудиоформаты, например через `ffmpeg`:

```
ffmpeg -i input.mp4 -vn -c:a aac -b:a 192k output.m4a
```

В данном примере видео `input.mp4` конвертируется в `output.m4a` с битрейтом 192кбит/с.
:::
