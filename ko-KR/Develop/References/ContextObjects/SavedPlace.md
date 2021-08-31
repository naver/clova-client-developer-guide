## Clova.SavedPlace {#SavedPlace}

`Clova.SavedPlace`는 클라이언트에 미리 저장되어 있는 위치 정보를 CIC에 보고할 때 사용하는 메시지 포맷입니다.

### Object structure

{% raw %}
```json
{
  "header": {
    "namespace": "Clova",
    "name": "SavedPlace"
  },
  "payload": {
    "places": [
      {
        "latitude": {{string}},
        "longitude": {{string}},
        "refreshedAt": {{string}},
        "name": {{string}}
      }
    ]
  }
}
```
{% endraw %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `places[]`             | object array | 미리 저장된 위치 정보를 가지고 있는 객체 배열                                          | 필수 |
| `places[].latitude`    | string       | 위도                                                                          | 필수 |
| `places[].longitude`   | string       | 경도                                                                          | 필수 |
| `places[].name`        | string       | 저장된 위치의 이름. 다음 값을 입력할 수 있습니다. <ul><li><code>"회사"</code></li><li><code>"집"</code></li></ul>       | 필수 |
| `places[].refreshedAt` | string       | 위치 저장 시간(`"YYYY-MM-DDThh:mm:ss±hh:mm"`, <a href="https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations" target="_blank">ISO 8601 Combined date and time representations</a> )  | 필수 |


### Object example

```json
{
  "header": {
    "namespace": "Clova",
    "name": "SavedPlace"
  },
  "payload": {
    "places": [
      {
        "latitude": "37.3594915",
        "longitude": "127.1032242",
        "refreshedAt": "2017-04-06T13:34:15.074361+08:28",
        "name": "집"
      },
      {
        "latitude": "36.3542315",
        "longitude": "125.1345242",
        "refreshedAt": "2017-03-12T10:21:33.089723+08:28",
        "name": "회사"
      }
    ]
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
