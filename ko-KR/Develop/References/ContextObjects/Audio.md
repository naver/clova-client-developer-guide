## Device.Audio {#Audio}

`Device.Audio`는 클라이언트 기기의 오디오 장치 정보를 CLOVA에 보고할 때 사용하는 메시지 포맷입니다. [플랫폼 지원 오디오 포맷](/Design/UI/Audio.md#SupportedAudioFormat)뿐만 아니라 클라이언트 기기가 지원하는 미디어 포맷 정보를 CLOVA에 전달함으로써 클라이언트가 재생 가능한 오디오 콘텐츠를 응답으로 받을 수 있게 됩니다. 

응답으로 전달되는 오디오의 포맷은 오디오 콘텐츠에 따라 달라질 수 있으며, 클라이언트 기기로부터 받은 선호 순서를 참고하여 전달됩니다. Device.Audio 정보를 CLOVA에 전달하지 않는 경우, 오디오 콘텐츠를 이용할 수 없습니다.

### Object structure

{% raw %}
```json
{
  "header": {
    "namespace": "Device",
    "name": "Audio"
  },
  "payload": {
    "acceptContentType": [
      {
        "mimeType": {{string}},
        "audioCodec": {{string}},
        "container": {{string}}
      }
    ]
  }
}
```
{% endraw %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `acceptContentType[]`                   | object array | 클라이언트 기기가 재생할 수 있는 미디어 포맷 목록. 클라이언트 기기가 재생할 수 있는 미디어 포맷 중 선호되는 미디어 포맷 순서로 배열을 구성해야 합니다. CLOVA 플랫폼은 이 목록과 목록의 순서를 참고하여 지원 가능한 타입의 오디오 콘텐츠를 제공합니다.  | 필수 |
| `acceptContentType[].audioCodec`        | string | 오디오 압축 포맷(codec). 다음 값을 입력할 수 있습니다. <ul><li><code>AAC</code></li><li><code>MP3</code></li></ul>  | 필수 |
| `acceptContentType[].fileExtension`     | string | 파일 확장자. 다음 값을 입력할 수 있으며, 둘 이상의 파일 확장자를 명시하려면 쉼표(,)로 구분 파일 확장자를 나열합니다. <ul><li><code>3GP</code></li><li><code>AAC</code></li><li><code>M4A</code></li><li><code>MP3</code></li><li><code>MP4</code></li><li><code>TS</code></ul>  | 필수 |
| `acceptContentType[].mimeType`          | string | MIME 타입. 다음 값을 입력할 수 있습니다. <ul><li><code>application/vnd.apple.mpegurl</code></li><li><code>audio/aac</code></li><li><code>audio/mpeg</code></li><li><code>audio/mpegurl</code></li></ul>  | 필수 |


### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "Audio"
  },
  "payload": {
    "acceptContentType": [
      { "mimeType": "application/vnd.apple.mpegurl", "audioCodec": "MP3", "fileExtension": "MP3" },
      { "mimeType": "application/vnd.apple.mpegurl", "audioCodec": "AAC", "fileExtension": "3GP,MP4,M4A,AAC,TS" },
      { "mimeType": "audio/mpeg", "audioCodec": "MP3", "fileExtension":  "MP3" },
      { "mimeType": "audio/aac", "audioCodec": "AAC", "fileExtension": "3GP,MP4,M4A,AAC,TS" }
    ]
  }
}
```

### See also

* [플랫폼 지원 오디오 포맷](/Design/UI/Audio.md#SupportedAudioFormat)
