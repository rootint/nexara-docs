---
sidebar_label: "Доступные языки"
sidebar_position: 3
---

# Поддерживаемые языки

Nexara поддерживает автоматическое определение и обработку текста на множестве языков. Ниже приведен полный список поддерживаемых языков с их кодами.

## Полный список поддерживаемых языков

| Код языка | Название языка |
|-----------|----------------|
| af | afrikaans |
| am | amharic |
| ar | arabic |
| as | assamese |
| az | azerbaijani |
| ba | bashkir |
| be | belarusian |
| bg | bulgarian |
| bn | bengali |
| bo | tibetan |
| br | breton |
| bs | bosnian |
| ca | catalan |
| cs | czech |
| cy | welsh |
| da | danish |
| de | german |
| el | greek |
| en | english |
| es | spanish |
| et | estonian |
| eu | basque |
| fa | persian |
| fi | finnish |
| fo | faroese |
| fr | french |
| gl | galician |
| gu | gujarati |
| ha | hausa |
| haw | hawaiian |
| he | hebrew |
| hi | hindi |
| hr | croatian |
| ht | haitian creole |
| hu | hungarian |
| hy | armenian |
| id | indonesian |
| is | icelandic |
| it | italian |
| ja | japanese |
| jw | javanese |
| ka | georgian |
| kk | kazakh |
| km | khmer |
| kn | kannada |
| ko | korean |
| la | latin |
| lb | luxembourgish |
| ln | lingala |
| lo | lao |
| lt | lithuanian |
| lv | latvian |
| mg | malagasy |
| mi | maori |
| mk | macedonian |
| ml | malayalam |
| mn | mongolian |
| mr | marathi |
| ms | malay |
| mt | maltese |
| my | myanmar |
| ne | nepali |
| nl | dutch |
| nn | nynorsk |
| no | norwegian |
| oc | occitan |
| pa | punjabi |
| pl | polish |
| ps | pashto |
| pt | portuguese |
| ro | romanian |
| ru | russian |
| sa | sanskrit |
| sd | sindhi |
| si | sinhala |
| sk | slovak |
| sl | slovenian |
| sn | shona |
| so | somali |
| sq | albanian |
| sr | serbian |
| su | sundanese |
| sv | swedish |
| sw | swahili |
| ta | tamil |
| te | telugu |
| tg | tajik |
| th | thai |
| tk | turkmen |
| tl | tagalog |
| tr | turkish |
| tt | tatar |
| uk | ukrainian |
| ur | urdu |
| uz | uzbek |
| vi | vietnamese |
| yi | yiddish |
| yo | yoruba |
| yue | cantonese |
| zh | chinese |

## Автоматическое определение языка

Если параметр языка не указан, передан как пустая строка или установлен как "auto"/"automatic", система автоматически определит язык вашего текста.

## Использование языковых кодов

В API и других интерфейсах вы можете указать язык, используя либо двухбуквенный код (например, "ru" для русского), либо полное название языка (например, "russian"). Система также поддерживает некоторые распространенные альтернативные названия языков (например, "valencian" для "catalan").

## Примеры кода

В примерах на API отправляется файл `example.mp3` и после обработки аудио в консоль выводится текст.

**Python:**

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://api.nexara.ru/api/v1",
    api_key="ВАШ_API_КЛЮЧ",
)

with open("example.mp3", "rb") as audio_file:
    transcription = client.audio.transcriptions.create(
        model="whisper-1",
        file=audio_file,
        language="ru"  # указание языка (опционально)
    )

print(transcription.text)
```

**Вывод:** Восемь десятков и семь лет назад наши отцы образовали на этом континенте новую нацию...

**JavaScript:**

```javascript
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
    language: "ru"  // указание языка (опционально)
  });

  console.log(transcription.text);
}

transcribeAudio();
```

**Вывод:** Восемь десятков и семь лет назад наши отцы образовали на этом континенте новую нацию...

**cURL:**

```bash
curl --request POST \
  --url https://api.nexara.ru/api/v1/audio/transcriptions \
  --header 'Authorization: Bearer ВАШ_API_КЛЮЧ' \
  --header 'Content-Type: multipart/form-data' \
  --form model="whisper-1" \
  --form file="@example.mp3" \
  --form language="ru"
```

**Вывод:** Восемь десятков и семь лет назад наши отцы образовали на этом континенте новую нацию...