<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

<!-- Start of the shared content: AudioPlayer.PlaybackState -->

## AudioPlayer.PlaybackState {#PlaybackState}

`AudioPlayer.PlaybackState`는 현재 재생하고 있거나 마지막으로 재생한 미디어 정보를 CIC에 보고할때 사용하는 메시지 포맷입니다.

### Object structure

{% raw %}
```json
{
  "header": {
    "namespace": "AudioPlayer",
    "name": "PlaybackState"
  },
  "payload": {
    "offsetInMilliseconds": {{number}},
    "playerActivity": {{string}},
    "repeatMode": {{string}},
    "stream": {{AudioStreamInfoObject}},
    "totalInMilliseconds": {{number}}
  }
}
```
{% endraw %}


### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 최근 재생 미디어의 마지막 재생 지점(offset). 단위는 밀리초이며, `0` 이상의 정수 값을 입력해야 합니다. `playerActivity` 값이 `"IDLE"`이면 이 필드 값은 입력하지 않아도 됩니다.                                                  | 선택 |
| `playerActivity`       | string | 플레이어의 상태를 나타내는 값이며 다음과 같은 값을 가집니다.<ul><li><code>"IDLE"</code>: 비활성 상태</li><li><code>"PLAYING"</code>: 재생 중인 상태</li><li><code>"PAUSED"</code>: 일시 정지 상태</li><li><code>"STOPPED"</code>: 중지 상태</li></ul> | 필수 |
| `repeatMode`           | string  | 반복 재생 모드.<ul><li><code>"NONE"</code>: 반복 재생 안함</li><li><code>"REPEAT_ONE"</code>: 한 곡 반복 재생</li></ul>                                                   | 필수  |
| `stream`               | [AudioStreamInfoObject](/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject) | 재생 중인 미디어의 상세 정보를 보관한 객체. `playerActivity` 값이 `"IDLE"`이면 이 필드 값은 입력하지 않아도 됩니다. [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 또는 [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver) 지시 메시지로 전달되었던 미디어 정보(`stream` 객체)의 값을 입력합니다. | 선택 |
| `totalInMilliseconds`  | number | 최근 재생 미디어의 전체 길이. [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지를 통해 전달받은 오디오 정보([AudioStreamInfoObject](/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject))에 `durationInMilliseconds` 필드 값이 있으면 이 필드의 값으로 입력하면 됩니다. 단위는 밀리초이며, `0` 이상의 정수 값을 입력해야 합니다. `playerActivity` 값이 `"IDLE"`이면 이 필드 값은 입력하지 않아도 됩니다.                                                               | 선택 |

### Object example

```json
// Case 1: 재생이 중지된 상태
{
  "header": {
    "namespace": "AudioPlayer",
    "name": "PlaybackState"
  },
  "payload": {
    "offsetInMilliseconds": 10000,
    "totalInMilliseconds": 300000,
    "playerActivity": "STOPPED",
    "repeatMode": "NONE",
    "stream": {
      "beginAtInMilliseconds": 0,
      "progressReport": {
        "progressReportDelayInMilliseconds": null,
        "progressReportIntervalInMilliseconds": null,
        "progressReportPositionInMilliseconds": 60000
      },
      "token": "TR-NM-17740107",
      "url": "clova:TR-NM-17740107",
      "urlPlayable": false
    }
  }
}

// 예제 2: 플레이어가 비활성화된 상태
{
  "header": {
    "namespace": "AudioPlayer",
    "name": "PlaybackState"
  },
  "payload": {
    "playerActivity": "IDLE",
    "repeatMode": "NONE"
  }
}
```

### See also

* [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play)
* [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver)
* [`AudioPlayer.StreamRequested`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamRequested)

<!-- End of the shared content -->
