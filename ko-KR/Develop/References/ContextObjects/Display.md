## Device.Display {#Display}

`Device.Display`는 클라이언트 기기의 디스플레이 장치 정보를 CIC에 보고할 때 사용하는 메시지 포맷입니다. 클라이언트 기기가 가진 디스플레이 장치의 정보를 CLOVA에 전달함으로써 화면 비율과 DPI에 맞는 화질의 미디어 콘텐츠를 응답으로 받을 수 있게 됩니다. 이 맥락 정보(context)를 CIC로 전송하지 않으면 CLOVA는 Full HD 급의 해상도를 가진 디스플레이 장치가 클라이언트에 있다고 가정합니다.

### Object structure

{% raw %}
```json
{
  "header": {
    "namespace": "Device",
    "name": "Display"
  },
  "payload": {
    "contentLayer": {
      "width": {{number}},
      "height": {{number}}
    },
    "dpi": {{number}},
    "orientation": {{string}},
    "size": {{string}}
  }
}
```
{% endraw %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `contentLayer`        | object | 디스플레이에서 콘텐츠가 표시되는 영역의 해상도 정보를 가지는 객체. `size`의 값이 `"custom"`이면 이 필드를 생략할 수 없습니다.  | 선택 |
| `contentLayer.width`  | number | 디스플레이에서 콘텐츠가 표시되는 영역의 너비. 단위는 픽셀(px)이며, 정수 값을 입력해야 합니다.                                            | 필수 |
| `contentLayer.height` | number | 디스플레이에서 콘텐츠가 표시되는 영역의 높이. 단위는 픽셀(px)이며, 단위는 정수 값을 입력해야 합니다.                                           | 필수 |
| `dpi`         | number | 디스플레이 장치의 DPI. `size`의 값이 `"none"`일 때만 이 필드를 생략할 수 있습니다. 정수 값을 입력해야 합니다.            | 선택 |
| `orientation` | string | 디스플레이 장치의 방향. `size`의 값이 `"none"`일 때만 이 필드를 생략할 수 있습니다.<ul><li><code>"landscape"</code>: 가로 방향</li><li><code>"portrait"</code>: 세로 방향</li></ul>  | 선택 |
| `size`        | string | 디스플레이 장치의 해상도 크기를 나타내는 값. 크기가 미리 지정된 값을 입력할 수도 있고 임의의 해상도 크기를 의미하는 값(`"custom"`)을 입력할 수도 있습니다. 또는 디스플레이 장치가 없음을 의미하는 값(`"none"`)을 입력할 수도 있습니다.<ul><li><code>"none"</code>: 클라이언트 기기에 디스플레이 장치가 없음</li><li><code>"s100"</code>: 저해상도(160px X 107px)</li><li><code>"m100"</code>: 중간 해상도(427px X 240px)</li><li><code>"l100"</code>: 고해상도(640px X 360px)</li><li><code>"xl100"</code>: 초고해상도(xlarge type, 899px X 506px)</li><li><code>"custom"</code>: 미리 정의된 규격이 아닌 해상도. 실제 기기의 해상도 값을 `contentLayer` 필드에 입력합니다.</li></ul> | 필수 |


### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "Display"
  },
  "payload": {
    "orientation": "portrait",
    "dpi": 320,
    "size": "custom",
    "contentLayer": {
      "width": 640,
      "height": 280
    }
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
* [Custom extension 메시지]({{ book.DocMeta.CLOVACustomExtensionDeveloperGuideBaseURI }}/Develop/References/Custom_Extension_Message.{{ book.DocMeta.FileExtensionForExternalLink }})
