<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# SpeechSynthesizer

<!-- Start of the shared content: CICAPIforAudioPlayback -->
SpeechSynthesizer 인터페이스는 클라이언트가 특정 텍스트를 TTS(text-to-speech)로 합성되도록 CLOVAㄴ에 요청하거나, CLOVA가 생성한 TTS를 클라이언트에 전달할 때 사용되는 네임스페이스입니다. SpeechSynthesizer가 제공하는 이벤트 메시지와 지시 메시지는 다음과 같습니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`Request`](#Request)                 | Event     | 특정 텍스트를 TTS로 생성하도록 CLOVA에 요청합니다.                                               |
| [`Speak`](#Speak)                     | Directive | 클라이언트가 합성된 TTS를 스피커로 출력하도록 지시합니다.                                        |
| [`SpeechFinished`](#SpeechFinished)   | Event     | TTS 재생을 완료했음을 CLOVA에 보고합니다.                                 |
| [`SpeechStarted`](#SpeechStarted)     | Event     | TTS 재생을 시작했음을 CLOVA에 보고합니다.                                 |
| [`SpeechStopped`](#SpeechStopped)     | Event     | TTS 재생을 중지했음을 CLOVA에 보고합니다.                                 |

<!-- End of the shared content -->

## Request event {#Request}

특정 텍스트를 TTS로 생성하도록 CLOVA에 요청합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `lang`  | string | 음성 합성에 사용할 언어. <ul><li><code>"en"</code>: 영어</li><li><code>"ja"</code>: 일본어</li><li><code>"ko"</code>: 한국어</li><li><code>"zh"</code>: 중국어</li></ul> | 필수    |
| `text`  | string | TTS 생성을 요청할 대상 텍스트           | 필수    |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Request",
      "messageId": "ab63d4cb-49f0-4a92-94fc-5ee356193551"
    },
    "payload": {
      "text": "음성파일 만들어줘",
      "lang": "ko"
    }
  }
}
```

### See also

* [`SpeechSynthesizer.Speak`](#Speak)

## Speak directive {#Speak}
클라이언트가 합성된 TTS를 스피커로 출력하도록 지시합니다. 클라이언트는 하나의 요청에 대한 응답으로 복수의 `SpeechSynthesizer.Speak` 지시 메시지를 전달받을 수 있습니다. 따라서, 클라이언트는 메시지를 수신한 순서대로 TTS를 재생해야 합니다. TTS는 [multipart 메시지](/Develop/References/CIC_API.md#MultipartMessage)로 전달될 수도 있고 오디오 스트리밍 주소 형태로 전달될 수도 있습니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `contentType`          | string  | 재생할 음성 파일의 MIME 타입. 이 필드는 재생할 음성 파일이 HLS 방식일 때 제공되며, `"application/vnd.apple.mpegurl"` 값을 가집니다. | 조건부  |
| `format`               | string  | 파일 포맷. 현재 `"AUDIO_MPEG"`로 고정되어 있습니다. | 항상    |
| `token`                | string  | TTS를 식별하는 token 값.<div class="note"><p><strong>Note!</strong></p><p>이 필드의 최대 길이는 2048 바이트입니다.</p></div>                    | 항상    |
| `ttsLang`              | string  | TTS 합성에 사용할 언어. <ul><li><code>"en"</code>: 영어</li><li><code>"ja"</code>: 일본어</li><li><code>"ko"</code>: 한국어</li><li><code>"zh"</code>: 중국어</li></ul> | 조건부    |
| `url`                  | string  | 재생할 음성 파일의 URI.<div class="note"><p><strong>Note!</strong></p><p>이 필드의 최대 길이는 2048 바이트입니다.</p></div>                        | 항상    |
| `x-clova-pause-before` | number  | 파일 재생 전 유휴 시간. 단위는 밀리 초이며, `0` 이상의 정수 값을 가집니다.        | 조건부    |

### Remarks

`url`은 아래와 같이 두 가지의 포맷을 가집니다. 클라이언트는 각 포맷에 따라 음성 출력 처리를 다음과 같이 다르게 해야 합니다.

| 포맷 | 설명 |
|---------|-------------------------------|
| `cid:{Content-ID}` 포맷 | 클라이언트의 `url` 값이 `cid:{Content-ID}` 포맷이면 합성된 음성이 multipart 메시지로 전달됩니다. 메시지 헤더 중 `Content-ID` 값이 같은 메시지의 오디오 데이터(바이너리 형식)을 재생해야 합니다. 오디오 데이터가 포함된 메시지는 순서가 보장되지 않기 때문에 전달된 지시 메시지의 `Content-ID` 값을 기준으로 음성 데이터를 출력합니다.|
| URI 포맷 | 전달받은 `url`의 오디오 스트림을 재생해야 합니다.  |

### Message example

```
// cid:{Content-ID} 포맷
// Content-ID가 22f2ca4e-3b08-4d33-b32a-7eb62a8c0369인 오디오 데이터 메시지를 재생해야 합니다.

--Boundary-Text
Content-Disposition: form-data; name="speakDirective1"
Content-Type: application/json; charset=utf-8

{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524",
      "dialogRequestId": "caa7862a-3566-4aef-98de-489be0973e18"
    },
    "payload": {
      "format": "AUDIO_MPEG",
      "token": "cbba5103-8ce4-4e65-869b-f94d5878f579",
      "ttsLang": "ko",
      "url": "cid:22f2ca4e-3b08-4d33-b32a-7eb62a8c0369",
      "x-clova-pause-before": 0
    }
  }
}

--Boundary-Text
Content-Disposition: form-data; name="attachment"
Content-ID: 22f2ca4e-3b08-4d33-b32a-7eb62a8c0369
Content-Type: application/octet-stream

{ Audio Attachment }
--Boundary-Text--

// URI 포맷
--Boundary-Text
{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "0313b471-ad7f-4cdd-b4e1-c046ca8b4b58",
      "dialogRequestId": "efa43b14-67f4-4f00-86bc-dfa08a08ad0b"
    },
    "payload": {
      "format": "AUDIO_MPEG",
      "token": "64ffeb07-4b86-4659-9f59-07a77b363a0b",
      "url": "https://example.net/clova/clova_song/1.mp3"
    }
  }
}

// URL 포맷(HLS 음원)
--Boundary-Text
{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "28950dbe-a5a3-4171-9307-0b81d5530a7f",
      "dialogRequestId": "8dce05a4-83ec-42f4-ac6c-a6660c192b96"
    },
    "payload": {
      "contentType": "application/vnd.apple.mpegurl",
      "format": "AUDIO_MPEG",
      "token": "ae97dc8c-859e-4cbb-9884-d32c94029835",
      "url": "https://example.net/clova/clova_song/1.m3u8"
    }
  }
}
--Boundary-Text--
```

### See also

* [`SpeechSynthesizer.Request`](#Request)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- Start of the shared content: SpeechSynthesizer.SpeechFinished -->

## SpeechFinished event {#SpeechFinished}

TTS 재생을 완료했음을 CLOVA에 보고합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | [`SpeechSynthesizer.Speak`](#Speak) 지시 메시지를 통해 전달받은 TTS 식별용 token 값 | 필수    |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechFinished",
      "messageId": "15472673-49a0-4aa1-8cf0-6355669ea473"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

### See also

* [`SpeechSynthesizer.Speak`](#Speak)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: SpeechSynthesizer.SpeechStarted -->

## SpeechStarted event {#SpeechStarted}

TTS 재생을 시작했음을 CLOVA에 보고합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | [`SpeechSynthesizer.Speak`](#Speak) 지시 메시지를 통해 전달받은 TTS 식별용 token 값 | 필수   |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechStarted",
      "messageId": "380c805c-0f19-4ed2-84e2-056f2f4016de"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

### See also

* [`SpeechSynthesizer.Speak`](#Speak)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: SpeechSynthesizer.SpeechStopped -->

## SpeechStopped event {#SpeechStopped}

TTS 재생을 중지했음을 CLOVA에 보고합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | [`SpeechSynthesizer.Speak`](#Speak) 지시 메시지를 통해 전달받은 TTS 식별용 token 값 | 필수    |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechStopped",
      "messageId": "9a511e5c-4f20-413a-94cc-48172fc8710e"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

### See also

* [`SpeechSynthesizer.Speak`](#Speak)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)

<!-- End of the shared content -->
