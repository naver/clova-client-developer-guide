# TextRecognizer

TextRecognizer 인터페이스는 사용자가 입력한 텍스트를 인식할 때 사용되는 네임스페이스입니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`Recognize`](#Recognize) | Directive  | 사용자 텍스트 입력을 전달하여 텍스트 인식을 CLOVA에 요청합니다.  |

## Recognize event {#Recognize}

사용자 텍스트 입력을 전달하여 텍스트 인식을 CLOVA에 요청합니다. CLOVA 내부의 자연어 분석 시스템과 대화 이해 시스템이 텍스트 입력을 해석하여 사용자의 요청을 처리합니다. 사용자의 음성 입력을 받기 어려운 환경에 있거나 마이크 시스템이 정상 동작하지 않는다면 [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) 이벤트 메시지 대신 사용하여 사용자의 텍스트 입력을 받을 수 있습니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `explicit`         | boolean  | [`SpeechRecognizer.ExpectSpeech`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech) 지시 메시지로 인해 사용자 입력을 추가로 받으려면 `SpeechRecognizer.ExpectSpeech` 지시 메시지에 포함된 `explicit` 필드의 값을 그대로 입력합니다.  | 선택  |
| `speechId`   | string   | [`SpeechRecognizer.ExpectSpeech`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech) 지시 메시지로 인해 사용자 입력을 추가로 받으려면 `SpeechRecognizer.ExpectSpeech` 지시 메시지에 포함된 `expectSpeechId` 필드의 값을 그대로 입력합니다.  | 선택  |
| `text`        | string  | 사용자가 입력한 텍스트를 설정합니다. | 필수     |

### Message example

```json
// 일반적인 사용자 텍스트 입력
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "TextRecognizer",
      "name": "Recognize",
      "dialogRequestId": "bc834682-6d22-4bbb-8352-4a49df2ed3d7",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "text": "지금 날씨 어때?"
    }
  }
}

// SpeechRecognizer.ExpectSpeech 지시 메시지에 따른 추가적인 사용자 텍스트 입력
{
  "header": {
      "dialogRequestId": "d3f81fec-4cb9-4ce9-a046-1ea9a71018df",
      "messageId": "8526a048-4141-4c30-98a4-c61e223afece",
      "namespace": "TextRecognizer",
      "name": "Recognize"
  },
  "payload": {
      "text": "내일은?",
      "speechId": "1a4cd9ac-8fd8-4929-9c30-3a592dd2c298",
      "explicit": false
  }
}
```

### See also

* [`SpeechRecognizer.ExpectSpeech`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
