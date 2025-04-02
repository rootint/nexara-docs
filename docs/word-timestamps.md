---
sidebar_label: "Временные метки"
sidebar_position: 4
---

# Временные метки для слов (Word Timestamps)

Nexara API позволяет получать не только полную транскрипцию аудиофайла, но и точные временные метки (начало и конец) для каждого распознанного слова. Эта функция полезна для множества задач, таких как создание точных субтитров, анализ речи, синхронизация текста с аудиодорожкой и реализация интерактивных интерфейсов.

## Структура ответа

Когда вы запрашиваете временные метки для слов с `response_format="verbose_json"`, ответ API будет содержать не только поле `text` с полной транскрипцией, но и дополнительные поля, включая `words`.

Поле `words` представляет собой массив объектов, где каждый объект соответствует одному распознанному слову и имеет следующую структуру:

*   `word` (string): Распознанное слово.
*   `start` (float): Время начала слова в секундах от начала аудиофайла.
*   `end` (float): Время окончания слова в секундах от начала аудиофайла.

### Пример ответа (`verbose_json` с метками слов)

```json
{
  "task": "transcribe",
  "language": "russian",
  "duration": 10.5,
  "text": "Восемь десятков и семь лет назад наши отцы образовали на этом континенте новую нацию...",
  "words": [
    {
      "word": "Восемь",
      "start": 0.52,
      "end": 0.98
    },
    {
      "word": "десятков",
      "start": 1.05,
      "end": 1.61
    },
    {
      "word": "и",
      "start": 1.61,
      "end": 1.70
    },
    {
      "word": "семь",
      "start": 1.75,
      "end": 2.05
    },
    {
      "word": "лет",
      "start": 2.10,
      "end": 2.40
    },
    {
      "word": "назад",
      "start": 2.45,
      "end": 2.95
    },
    {
      "word": "наши",
      "start": 3.10,
      "end": 3.48
    },
    {
      "word": "отцы",
      "start": 3.55,
      "end": 4.01
    },
    {
      "word": "образовали",
      "start": 4.15,
      "end": 5.05
    },
    // ... и так далее для всех слов
  ],
  "segments": [ // Поле segments также присутствует в verbose_json
    {
      "id": 0,
      "seek": 0,
      "start": 0.52,
      "end": 5.05,
      "text": "Восемь десятков и семь лет назад наши отцы образовали",
      "tokens": [ /* ... */ ],
      "temperature": 0.0,
      "avg_logprob": -0.25,
      "compression_ratio": 1.5,
      "no_speech_prob": 0.05,
      "words": [ /* массив слов для этого сегмента */ ] // Может дублироваться здесь в зависимости от реализации
    },
    // ... другие сегменты
  ]
}
```

## Примеры кода

В примерах ниже показано, как запросить временные метки для слов при отправке файла `example.mp3`.

**Python:**

```python
from openai import OpenAI
import json # Для красивого вывода JSON

client = OpenAI(
    base_url="https://api.nexara.ru/api/v1",
    api_key="ВАШ_API_КЛЮЧ",
)

with open("example.mp3", "rb") as audio_file:
    transcription = client.audio.transcriptions.create(
        model="whisper-1",
        file=audio_file,
        language="ru",  # указание языка (опционально)
        response_format="verbose_json", # Обязательно для меток слов
        timestamp_granularities=["word"] # Запрашиваем метки слов
    )

# Выводим весь объект ответа, чтобы увидеть структуру с 'words'
# Используем Pydantic model's dict() method if available, or just print the object
try:
    # Если transcription - это Pydantic модель (как в официальном openai client)
    print(json.dumps(transcription.dict(), indent=2, ensure_ascii=False))
except AttributeError:
    # Если это просто dict или другой объект
    print(json.dumps(transcription, indent=2, ensure_ascii=False, default=str))

# Чтобы получить доступ к конкретным словам:
# for word_info in transcription.words:
#     print(f"Слово: {word_info.word}, Начало: {word_info.start}, Конец: {word_info.end}")
```

**Вывод (структура):** Вывод будет соответствовать JSON-структуре, показанной в примере ответа выше.

**JavaScript:**

```javascript
const OpenAI = require("openai");
const fs = require("fs");

const openai = new OpenAI({
  baseURL: "https://api.nexara.ru/api/v1",
  apiKey: "ВАШ_API_КЛЮЧ",
});

async function transcribeAudioWithWordTimestamps() {
  try {
    const transcription = await openai.audio.transcriptions.create({
      model: "whisper-1",
      file: fs.createReadStream("example.mp3"),
      language: "ru", // указание языка (опционально)
      response_format: "verbose_json", // Обязательно для меток слов
      timestamp_granularities: ["word"] // Запрашиваем метки слов
    });

    // Выводим весь объект ответа
    console.log(JSON.stringify(transcription, null, 2));

    // Пример доступа к данным слов:
    // if (transcription.words) {
    //   transcription.words.forEach(wordInfo => {
    //     console.log(`Слово: ${wordInfo.word}, Начало: ${wordInfo.start}, Конец: ${wordInfo.end}`);
    //   });
    // }

  } catch (error) {
    console.error("Ошибка при транскрибации:", error);
  }
}

transcribeAudioWithWordTimestamps();
```

**Вывод (структура):** Вывод будет соответствовать JSON-структуре, показанной в примере ответа выше.

**cURL:**

```bash
curl --request POST \
  --url https://api.nexara.ru/api/v1/audio/transcriptions \
  --header 'Authorization: Bearer ВАШ_API_КЛЮЧ' \
  --header 'Content-Type: multipart/form-data' \
  --form model="whisper-1" \
  --form file="@example.mp3" \
  --form language="ru" \
  --form response_format="verbose_json" \
  --form 'timestamp_granularities[]="word"'
  # Или --form 'timestamp_granularities[]="segment"' --form 'timestamp_granularities[]="word"'
```

**Вывод (структура):** Вывод будет соответствовать JSON-структуре, показанной в примере ответа выше.

## Применение

*   **Точные субтитры:** Создание файлов субтитров (например, `.srt` или `.vtt`), где каждое слово появляется и исчезает в нужный момент.
*   **Интерактивные плееры:** Выделение слов в текстовой транскрипции по мере их произнесения в аудио/видео.
*   **Поиск по аудио:** Быстрый переход к моменту в аудиозаписи, где было произнесено конкретное слово.
*   **Анализ речи:** Изучение длительности слов, пауз между словами для анализа темпа и ритма речи.
