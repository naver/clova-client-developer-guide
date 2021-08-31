## Clova.Location {#Location}

`Clova.Location`은 클라이언트의 현재 위치 정보를 CIC에 보고할 때 사용하는 메시지 포맷입니다. GPS, 기지국, 네트워크 기기와 같은 것을 통해 파악된 위치 정보를 CIC로 전송할 수 있습니다.

### Object structure

{% raw %}
```json
{
  "header": {
    "namespace": "Clova",
    "name": "Location"
  },
  "payload": {
    "latitude": {{string}},
    "longitude": {{string}},
    "refreshedAt": {{string}}
  }
}
```
{% endraw %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `latitude`      | string  | 위도                                                                                     | 필수 |
| `longitude`     | string  | 경도                                                                                     | 필수 |
| `refreshedAt`   | string  | 위치를 마지막으로 확인한 시점(`"YYYY-MM-DDThh:mm:ss±hh:mm"`, <a href="https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations" target="_blank">ISO 8601 Combined date and time representations</a> ) | 필수 |

### Object example

```json
{
  "header": {
    "namespace": "Clova",
    "name": "Location"
  },
  "payload": {
    "latitude": "37.3594915",
    "longitude": "127.1032242",
    "refreshedAt": "2017-04-06T13:34:15.074361+08:28"
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
