---
sidebar_label: "Форматы вывода ответа"
sidebar_position: 2
---

# Форматы вывода ответа

Поддерживаются следующие форматы вывода ответа: `json`, `verbose_json`, `text`, `srt` и `vtt`. По умолчанию, ответ возвращается в `json`.

### Примеры

**text:**

```
Это пример ответа от API
```

**json:**

```json
{
  "text": "Это пример ответа от API"
}
```

**verbose_json:**

```json
{
  "task": "transcribe",
  "text": "Это пример ответа от API",
  "duration": 5.1,
  "language": "ru",
  "segments": [
    {
      "id": 0,
      "avg_logprob": -1.0,
      "compression_ratio": 2.4,
      "end": 5.1,
      "no_speech_prob": 0.5,
      "seek": 0,
      "start": 0.0,
      "temperature": 1.0,
      "text": "Это пример ответа от API",
      "tokens": [509, 600, 658, 281, 2573, 484, 428, 6532, 337, 6492, 807, 264]
    }
  ]
}
```

**vtt:**

```
WEBVTT

00:00.610 --> 00:05.100
Это пример ответа от API


```

**srt:**

```
1
00:00:00,610 --> 00:00:05,100
Это пример ответа от API


```