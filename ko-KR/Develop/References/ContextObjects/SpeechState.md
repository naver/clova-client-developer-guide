## SpeechSynthesizer.SpeechState {#SpeechState}

`SpeechSynthesizer.SpeechState`는 클라이언트가 [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지로 전달된 TTS(text-to-speech)를 재생하고 있는지 여부를 CIC에 보고할 때 사용하는 메시지 포맷입니다.

### Object structure

{% raw %}
```json
{
  "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechState"
  },
  "payload": {
      "token": {{string}},
      "playerActivity": {{string}}
  }
}
```
{% endraw %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `playerActivity` | string | TTS 재생 상태 <ul><li><code>PLAYING</code>: 현재 TTS 재생 상태</li><li><code>STOPPED</code>: 사용자의 음성 입력이나 다른 인터럽트로 TTS 재생이 중간에 중지된 상태</li><li><code>FINISHED</code>: TTS 재생이 완료된 상태</li></ul>     | 필수     |
| `token`          | string | [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지를 통해 전달받은 TTS 식별용 token 값  | 필수     |

### Object example

```json
{
  "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechState"
  },
  "payload": {
      "token": "dc706e02-fe16-4337-9a6c-51f670b5adb2",
      "playerActivity": "FINISHED"
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
