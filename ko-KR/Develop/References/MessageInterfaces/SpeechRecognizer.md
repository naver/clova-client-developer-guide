# SpeechRecognizer

SpeechRecognizer 인터페이스는 사용자의 음성 인식을 위해 사용되는 네임스페이스입니다. 사용자의 음성 입력은 다음과 같이 수행되어야 합니다.

1. 사용자의 음성이 입력될 때 [`SpeechRecognizer.Recognize`](#Recognize) 이벤트 메시지를 CLOVA에 전송하십시오.
2. 입력되는 사용자의 음성을 200 밀리초(milliseconds) 단위로 계속 CLOVA에 전송하십시오.
3. CLOVA로부터 [`SpeechRecognizer.StopCapture`](#StopCapture) 지시 메시지를 받기 전까지 2 번 단계를 계속 수행하십시오.

SpeechRecognizer가 제공하는 이벤트 메시지와 지시 메시지는 다음과 같습니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`ExpectSpeech`](#ExpectSpeech)                 | Directive | 클라이언트가 마이크를 활성화하여 사용자의 음성 입력을 받도록 클라이언트에 지시합니다.                  |
| [`KeepRecording`](#KeepRecording)               | Directive | 클라이언트가 사용자의 음성 입력을 계속 받도록 클라이언트에 지시합니다.                     |
| [`Recognize`](#Recognize)                       | Event     | 입력되는 사용자의 음성을 전달하여 음성 인식을 CLOVA에 요청합니다.          |
| [`ShowRecognizedText`](#ShowRecognizedText)     | Directive | 클라이언트가 텍스트로 인식된 사용자 음성을 실시간으로 표시하도록 지시합니다.              |
| [`StopCapture`](#StopCapture)                   | Directive | 클라이언트가 사용자의 음성 입력 수신을 중지하도록 지시합니다.           |

## ExpectSpeech directive {#ExpectSpeech}

클라이언트가 마이크를 활성화하여 사용자의 음성 입력을 받도록 클라이언트에 지시합니다. 이 지시 메시지는 사용자의 요청에서 불충분한 정보를 추가로 요구하는 multi-turn 대화를 수행하기 위해 전달됩니다. 입력된 사용자 음성은 [`SpeechRecognizer.Recognize`](#Recognize) 이벤트 메시지를 사용하여 CLOVA에 전송해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `expectContentType`        | string  | 클라이언트가 추가로 사용자의 음성을 입력 받으면 해당 음성 데이터를 어떤 형식으로 보내야 할지 지정한 값입니다. 다음과 같은 값을 가질 수 있습니다. <ul><li><code>"audio/l16"</code>: 음성 인식을 위해 에코/잡음 제거 및 음성 인식에 특징적인 후처리를 수행하지 않은 PCM 포맷의 음성 데이터</li><li><code>"application/x-clova-feat"</code>: 음성 인식을 위해 에코/잡음 제거 및 음성 인식에 특징적인 후처리를 수행한 PCM 포맷의 음성 데이터</li></ul>  | 조건부  |
| `expectSpeechId`        | string  | 사용자의 음성 입력을 추가로 받을 때 CLOVA가 이를 식별하기 위한 ID. 추후 이 값은 사용자의 추가 음성 입력을 [`SpeechRecognizer.Recognize`](#Recognize) 이벤트 메시지로 CLOVA에 전송할 때 `speechId` 필드에 입력되어야 합니다.    | 항상 |
| `explicit`              | boolean | 사용자 음성 입력의 필수 여부.`explicit`는 주로 `true`로 설정되며, 사용자로부터 필수 정보를 추가로 알아내야 할 때 사용됩니다.<ul><li><code>true</code>: 필수</li><li><code>false</code>: 선택</li></ul>예를 들면, 사용자가 "피자 주문해줘."와 같은 요청을 했고 CLOVA는 수량과 같은 필수 정보를 얻기 위해 "몇 판 주문할까요?"라는 음성과 함께 `explicit` 필드가 `true`로 설정된 `SpeechRecognizer.ExpectSpeech` 지시 메시지를 보낼 수 있습니다. 이때 사용자로부터 수량 정보를 입력받지 않는다면 주문을 제대로 수행할 수 없게 됩니다. 따라서, `explicit` 필드가 `true`이면 반드시 사용자 음성 입력을 받아야 합니다. | 항상  |
| `timeoutInMilliseconds` | number  | 사용자의 음성 입력을 받기 위해 대기하는 시간. 단위는 밀리 초이며, `0` 이상의 정수 값을 포함합니다. | 항상    |

### Remarks

* 이 지시 메시지를 받으면 클라이언트는 사용자의 입력을 CLOVA에 전송할 때 이전 요청 메시지와 같은 대화 ID(`dialogRequestId`)를 사용해서 전송해야 합니다.
* `explicit` 필드가 `false`이면 클라이언트가 가지고 있는 정책이나 조건에 따라 사용자의 음성 입력을 받지 않을 수도 있습니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ExpectSpeech",
      "dialogRequestId": "bc834682-6d22-4bbb-8352-4a49df2ed3d7",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "timeoutInMilliseconds": 7000,
      "explicit": false,
      "expectSpeechId": "561aeecf-2096-40fa-ba17-6612e28b339f"
    }
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](#Recognize)

## KeepRecording directive {#KeepRecording}

클라이언트가 사용자의 음성 입력을 계속 받도록 클라이언트에 지시합니다. `SpeechRecognizer.KeepRecording` 지시 메시지는 클라이언트에 입력된 사용자의 음성을 인식하고 있음을 알려주는 중간 응답이며, 이 지시 메시지를 받은 클라이언트는 CLOVA의 응답에 대한 timeout을 구현하거나 UX에 활용할 수 있습니다.

### Payload fields

없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "KeepRecording",
      "dialogRequestId": "bc834682-6d22-4bbb-8352-4a49df2ed3d7",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {}
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](#Recognize)

## Recognize event {#Recognize}

입력되는 사용자의 음성을 전달하여 음성 인식을 CLOVA에 요청합니다. CLOVA 내부의 자연어 분석 시스템과 대화 이해 시스템이 음성 입력을 해석하여 사용자의 요청을 처리합니다. CLOVA로부터 전달되는 대부분의 [지시 메시지](/Develop/References/CIC_API.md#Directive)는 `SpeechRecognizer.Recognize` 이벤트 메시지를 통해 사용자의 요청을 확인한 후 전달됩니다.

다음과 같은 기준의 음성 입력을 처리할 수 있습니다.
* 16-bit Linear PCM
* 16 kHz sample rate
* Mono
* Little endian

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `explicit`                                               | boolean  | [`SpeechRecognizer.ExpectSpeech`](#ExpectSpeech) 지시 메시지로 인해 사용자 입력을 추가로 받으려면 `SpeechRecognizer.ExpectSpeech` 지시 메시지에 포함된 `explicit` 필드의 값을 그대로 입력합니다.  | 선택  |
| `format`                                                 | string   | 음성 데이터 포맷. `AUDIO_L16_RATE_16000_CHANNELS_1`으로 고정 입력합니다.                             | 선택    |
| `initiator`                                              | object   | CLOVA를 호출 시 사용된 방법, 음성 입력 경로, 호출어(wake word)에 대한 정보를 담는 객체<div class="tip"><p><strong>Tip!</strong></p><p>이 필드를 사용하면 음성 인식 성능을 높일 수 있습니다. 따라서, 이 필드를 사용할 것을 권장합니다.</p></div>                      | 선택    |
| `initiator.inputSource`                                  | string   | 사용자의 음성이 유입된 경로 정보(source). 다음과 같은 값을 입력해야 합니다.<ul><li><code>SELF</code>: <code>SpeechRecognizer.Recognize</code> 이벤트 메시지를 전송한 클라이언트 기기가 직접 사용자의 음성을 입력받았다면 이 값을 지정합니다.</li><li><code>CUSTOM_{Source_ID}</code>: <code>SpeechRecognizer.Recognize</code> 이벤트 메시지를 전송한 클라이언트 기기가 아닌 리모컨과 같이 다른 기기가 음성 입력을 받았다면 해당 기기의 ID를 지정합니다.</li></ul><div class="note"><p><strong>Note!</strong></p><p>기기의 ID는 사전에 제휴 담당자와 논의된 값을 사용해야 합니다.</p></div>  | 필수 |
| `initiator.payload`                                      | object   | `initiator` 필드에서 상세한 정보를 담는 객체                                                        | 선택 |
| `initiator.payload.deviceUUID`                           | string   | 기기에서 임의로 생성한 UUID. 한 번 생성된 UUID를 계속 사용해야 하며, CLOVA에서 특정 사용자의 정보를 식별할 수 없는 값이어야 합니다. 즉 이 필드의 값으로 {{ book.ServiceEnv.TargetServiceForClientAuth }} access token 값, CLOVA access token 값이나 클라이언트 ID 또는 이들을 조합하여 만든 값을 사용하면 안됩니다.   | 필수 |
| `initiator.payload.wakeWord`                             | object   | 클라이언트에서 인식된 호출어 정보를 담는 객체. 호출어 인식 성능을 향상시키기 위해 사용됩니다.       | 선택 |
| `initiator.payload.wakeWord.confidence`                  | number   | 기기에서 호출어 인식을 확신하는 정도(confidence)를 나타내는 값. 0 에서 1 사이의 실수(float) 형태의 값으로 입력합니다. 현재 이 필드는 유효하지 않으며 나중을 위해 예약해 둔 필드입니다.                 | 선택 |
| `initiator.payload.wakeWord.indices`                      | object   | 사용자 음성 입력을 담은 오디오 스트림에서 호출어 부분이 포함된 구간 정보를 담는 객체                                           | 필수 |
| `initiator.payload.wakeWord.indices.endIndexInSamples`    | number   | 오디오 스트림에서 호출어가 끝나는 시점의 index 정보. 음성 입력이 16 kHz sample rate를 가지므로 index의 1 단위는 16,000 분의 1 초를 의미합니다. 만약, 오디오 스트림에서 ​호출어 구간이 1 초 위치에서 끝난다면 이 필드의 값으로 1 초를 의미하는 `16000`을 입력해야 합니다. `initiator.payload.wakeWord.indices.startIndexInSamples` 필드 값보다 큰 정수 값을 입력해야 합니다. | 필수  |
| `initiator.payload.wakeWord.indices.startIndexInSamples`  | number   | 오디오 스트림에서 호출어가 시작되는 시점의 index 정보. 음성 입력이 16 kHz sample rate를 가지므로 index의 1 단위는 16,000 분의 1 초를 의미합니다. 호출어는 대체로 사용자 발화의 첫 부분에 위치하기 때문에 index 값을 0으로 입력하게 됩니다. `0` 이상의 정수 값을 입력해야 합니다.   | 필수 |
| `initiator.payload.wakeWord.name`                         | string   | 클라이언트 기기에 설정된 호출어. 다음과 같은 값을 입력할 수 있습니다.<ul><li><code>"hey_clova"</code>: 호출어 "헤이클로바"</li><li><code>"jjangguya"</code>: 호출어 "짱구야"</li></ul>                  | 선택  |
| `initiator.type`                                         | string   | 호출 시 사용된 방법. 다음과 같은 값을 입력할 수 있습니다. <ul><li><code>"PRESS_AND_HOLD"</code>: 음성 입력 수신 버튼(wake up)을 누른 채로 음성 입력</li><li><code>"TAP"</code>: 음성 입력 수신 버튼(wake up)을 눌렀다 뗀 후 음성 입력</li><li><code>"WAKEWORD"</code>: 호출어를 말한 후 음성 입력</li></ul>  | 필수 |
| `lang`                                                   | string   | 사용자 음성 입력이 어떤 언어로 인식되도록 할지 결정합니다. <ul><li><code>"en"</code>: 영어</li><li><code>"ja"</code>: 일본어</li><li><code>"ko"</code>: 한국어</li></ul> | 필수    |
| `profile`                                                | string   | 추후 사용을 위해 예약해 놓은 필드. `CLOSE_TALK`으로 고정 입력합니다.                                     | 선택    |
| `speechId`                                               | string   | [`SpeechRecognizer.ExpectSpeech`](#ExpectSpeech) 지시 메시지로 인해 사용자 입력을 추가로 받으려면 `SpeechRecognizer.ExpectSpeech` 지시 메시지에 포함된 `expectSpeechId` 필드의 값을 그대로 입력합니다.  | 선택  |

### Message example

```json
// 일반적인 사용자 음성 입력
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "Recognize",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "lang": "ko",
      "profile": "CLOSE_TALK",
      "format": "AUDIO_L16_RATE_16000_CHANNELS_1",
      "initiator": {
        "type": "WAKEWORD",
        "inputSource": "SELF",
        "payload": {
          "deviceUUID": "f003af9d-14c5-424b-b1f9-f0134bd0ed86",
          "wakeWord": {
            "name": "hey_clova",
            "confidence": 0.812312,
            "indices": {
              "startIndexInSamples": 0,
              "endIndexInSamples": 16000
            }
          }
        }
      }
    }
  }
}

// SpeechRecognizer.ExpectSpeech 지시 메시지에 따른 추가적인 사용자 음성 입력
{
  "context": [
    ...
  ],
  "header": {
      "dialogRequestId": "4951cbfe-0064-41e2-ac3a-b0e4e1b0a570",
      "messageId": "6ab89102-668b-42eb-89d0-639253db10ba",
      "namespace": "SpeechRecognizer",
      "name": "Recognize"
  },
  "payload": {
      "profile": "CLOSE_TALK",
      "format": "AUDIO_L16_RATE_16000_CHANNELS_1",
      "speechId": "561aeecf-2096-40fa-ba17-6612e28b339f",
      "explicit": false
  }
}
```

### Audio Data

`SpeechRecognizer.Recognize` 이벤트 메시지를 전송한 이후 다음과 같은 음성 데이터를 사용자가 음성 입력을 마치거나 [StopCapture](#StopCapture) 지시 메시지를 받기 전까지 계속 전달합니다. 이때, 음성 데이터는 같은 HTTP 요청 안에서 멀티 파트 메시지로 계속 전딜되어야 합니다.

```
[ Message Header ]
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[ PCM Audio Attachment ]
```

<div class="note">
  <p><strong>Note!</strong></p>
  <p><code>initiator.type</code>의 값이 <code>"WAKEWORD"</code>이면 음성 데이터를 전송할 때 반드시 호출어 부분에 해당하는 음성도 포함시켜야 합니다. 이때, 음성 데이터는 호출어 구간의 시작점보다 300 밀리 초(4,800 sample) 앞선 지점부터 시작되어야 합니다.</p>
</div>

### See also

* [`SpeechRecognizer.ExpectSpeech`](#ExpectSpeech)
* [`SpeechRecognizer.StopCapture`](#StopCapture)

## ShowRecognizedText directive {#ShowRecognizedText}

클라이언트가 텍스트로 인식된 사용자 음성을 실시간으로 표시하도록 지시합니다. CLOVA 음성 인식 시스템은 [`SpeechRecognizer.Recognize`](#Recognize) 이벤트 메시지로 전달받고 있는 사용자의 음성 입력을 분석하여 인식 결과를 제공합니다. CLOVA는 `SpeechRecognizer.ShowRecognizedText` 지시 메시지로 사용자 음성 인식의 중간 처리 결과를 클라이언트로 전달합니다. 클라이언트는 이를 바탕으로 처리 과정을 사용자에게 실시간으로 보여줄 수 있습니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `text`  | string | 입력된 사용자 음성이 어떤 어떻게 인식이 되고 있는지 그 결과를 실시간으로 담고 있습니다. | 항상    |

### Remarks

* 해당 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.
* 기본적으로 CLOVA는 음성 인식 중간 결과를 클라이언트에 전달하지 않으며, 일부 특수한 조건에 `SpeechRecognizer.ShowRecognizedText` 지시 메시지를 전달합니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>이 지시 메시지를 사용하려면 제휴 담당자에게 연락하시기 바랍니다.</p>
</div>

### Message example

```json
// 중간 인식 결과 1
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ShowRecognizedText",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "ceb4c638-d817-4a65-977d-03726d72cb91"
    },
    "payload": {
      "text": "오늘"
    }
  }
}

// 중간 인식 결과 2
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ShowRecognizedText",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "fab4c638-d8df7-4adf-977d-adf72cb91"
    },
    "payload": {
      "text": "오늘 날씨"
    }
  }
}

// 중간 인식 결과 3
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ShowRecognizedText",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "efac233-8df7d-14df-d37d-72f72cb91"
    },
    "payload": {
      "text": "오늘 날씨 알려줘"
    }
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](#Recognize)
* [`SpeechRecognizer.StopCapture`](#StopCapture)

## StopCapture directive {#StopCapture}
클라이언트가 사용자의 음성 입력 수신을 중지하도록 지시합니다. CLOVA는 [`SpeechRecognizer.Recognize`](#Recognize) 이벤트 메시지를 받은 후 더 이상 녹음 데이터(PCM)를 수신할 필요가 없다고 판단했을 때 이 지시 메시지를 클라이언트에 전송합니다. 클라이언트는 이 메시지를 수신한 즉시 사용자 음성 녹음을 중지해야 합니다. CLOVA가 이 메시지를 보낸 후에도 사용자 음성 정보를 수신할 수 있지만 해당 음성 정보는 처리되지 않습니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `recognizedText` | string | 입력된 사용자 음성이 어떻게 인식이 되었는지 그 결과를 담고 있습니다. 기본적으로 이 필드는 `SpeechRecognizer.StopCapture` 지시 메시지에 포함되지 않으며, 일부 특수한 조건에 이 필드가 포함됩니다.<div class="note"><p><strong>Note!</strong></p><p>이 필드를 사용하려면 제휴 담당자에게 연락하시기 바랍니다.</p></div> | 조건부 |

### Remarks

이 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

```json
// 예제 1: recognizedText 필드가 있는 예제
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "StopCapture",
      "dialogRequestId": "eaa19eaa-07bc-447a-9e3f-c3b4a7d994e8",
      "messageId": "cc9f2a05-34c8-4edd-b810-2c040ac3d672"
    },
    "payload": {
      "recognizedText": "오늘 날씨 알려줘"
    }
  }
}
```

// 예제 2: recognizedText 필드가 없는 예제
```json
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "StopCapture",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](#Recognize)
* [`SpeechRecognizer.ShowRecognizedText`](#ShowRecognizedText)
