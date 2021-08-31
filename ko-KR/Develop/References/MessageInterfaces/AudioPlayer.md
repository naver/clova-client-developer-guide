<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# AudioPlayer

<!-- Start of the shared content: CICAPIforAudioPlayback -->
AudioPlayer 인터페이스는 클라이언트에서 오디오 스트림 재생을 요청하거나 오디오 스트림을 재생하는 중에 발생하는 이벤트를 CIC로 보고할 때 사용되는 네임스페이스입니다. 

<div class="note">
  <p><strong>Note!</strong></p>
  <p>오디오 서비스를 이용하기 위해서는 클라이언트 기기가 지원하는 오디오 포맷을 전달해야합니다. <a href="/Develop/References/Context_Objects.md#Audio">Device.Audio</a> 맥락 정보의 `acceptContentType[]` 필드를 이용하면 클라이언트 기기가 지원하는 미디어 포맷과 함께 선호하는 포맷의 순서를 전달할 수 있습니다.</p>
</div>

AudioPlayer가 제공하는 이벤트 메시지와 지시 메시지는 다음과 같습니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`ClearQueue`](#ClearQueue)           | Directive | CLOVA가 오디오 스트림 재생 대기열(queue)을 초기화하도록 클라이언트에 지시합니다.                              |
| [`ExpectReportPlaybackState`](#ExpectReportPlaybackState) | Directive | CLOVA가 현재 재생 상태를 보고하도록 클라이언트에 지시합니다. |
| [`Play`](#Play)                       | Directive | CLOVA가 특정 오디오 스트림을 재생하거나 재생 대기열에 추가하도록 클라이언트에 지시합니다.                         |
| [`PlaybackQueueCleared`](#PlaybackQueueCleared) | Event   | 클라이언트가 CIC로부터 [`AudioPlayer.ClearQueue`](#ClearQueue) 지시 메시지를 받았다면 재생 대기열(queue)를 초기화한 후 `PlaybackQueueCleared` 이벤트 메시지를 전송해야 합니다.       |
| [`PlayFinished`](#PlayFinished)       | Event     | 클라이언트가 오디오 스트림 재생을 완료할 때 재생 완료된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다.     |
| [`PlayPaused`](#PlayPaused)           | Event     | 클라이언트가 오디오 스트림 재생을 일시 정지할 때 일시 정지된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다. |
| [`PlayResumed`](#PlayResumed)         | Event     | 클라이언트가 오디오 스트림 재생을 재개할 때 재개된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다.         |
| [`PlayStarted`](#PlayStarted)         | Event     | 클라이언트가 오디오 스트림 재생을 시작할 때 재생이 시작된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다.    |
| [`PlayStopped`](#PlayStopped)         | Event     | 클라이언트가 오디오 스트림 재생을 중지할 때 재생이 중지된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다.    |
| [`ProgressReportDelayPassed`](#ProgressReportPositionPassed) | Event | 오디오 스트림 재생이 시작된 후 지정된 지연 시간만큼 시간이 지났을 때 현재 재생 상태([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState))를 CIC로 보고하기 위해 사용됩니다. 각 오디오 스트림의 지연 시간은 [`AudioPlayer.Play`](#Play) 지시 메시지가 클라이언트로 전달될 때 확인할 수 있습니다. |
| [`ProgressReportIntervalPassed`](#ProgressReportPositionPassed)| Event | 오디오 스트림 재생이 시작된 후 지정된 간격마다 현재 재생 상태([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState))를 CIC로 보고하기 위해 사용됩니다. 각 오디오 스트림의 보고 간격은 [`AudioPlayer.Play`](#Play) 지시 메시지가 클라이언트로 전달될 때 확인할 수 있습니다.|
| [`ProgressReportPositionPassed`](#ProgressReportPositionPassed) | Event | 오디오 스트림 재생이 시작된 후 지정된 보고 시점에 현재 재생 상태([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState))를 CIC로 보고하기 위해 사용됩니다. 각 오디오 스트림의 보고 시점은 [`AudioPlayer.Play`](#Play) 지시 메시지가 클라이언트로 전달될 때 확인할 수 있습니다.|
| [`ReportPlaybackState`](#ReportPlaybackState)           | Event  | 클라이언트의 현재 음원 재생 상태를 CIC로 보고합니다. 클라이언트가 CIC로부터 [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState) 지시 메시지를 받았을 때 `AudioPlayer.ReportPlaybackState` 이벤트 메시지를 CIC로 전송해야 합니다.  |
| [`StreamDeliver`](#StreamDeliver)     | Directive | CLOVA가 오디오 스트림 정보를 교체하여 재생하도록 클라이언트에 지시합니다. |
| [`StreamRequested`](#StreamRequested) | Event     | 오디오 스트림 재생을 위해 CIC로 스트리밍 URL과 같은 추가 정보를 요청하는 이벤트 메시지입니다.               |

<!-- End of the shared content -->

## ClearQueue directive {#ClearQueue}

CLOVA가 오디오 스트림 재생 대기열(queue)을 초기화하도록 클라이언트에 지시합니다. 이 지시 메시지의 `clearBehavior` 필드 값은 초기화 동작을 구분하며, 클라이언트가 재생 대기열을 초기화하면서 현재 재생 중인 오디오 스트림의 재생을 멈춰야 하는지를 결정합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `clearBehavior`           | string | 초기화 동작을 결정하는 구분자<ul><li><code>"CLEAR_ALL"</code>: 재생 대기열을 모두 비우고, 현재 재생 중인 오디오 스트림의 재생을 즉시 멈춥니다.</li><li><code>"CLEAR_ENQUEUED"</code>: 재생 대기열만 비우고, 현재 재생 중인 오디오 스트림은 계속 재생합니다.</li></ul> | 항상 |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ClearQueue",
      "dialogRequestId": "8b81296d-218e-4a08-897a-bee51daad907",
      "messageId": "823a703d-9447-438a-bad5-21fa7a62b623"
    },
    "payload": {
      "clearBehavior": "CLEAR_ALL"
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlaybackQueueCleared`](#PlaybackQueueCleared)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`AudioPlayer.PlayStopped`](#PlayStopped)

## ExpectReportPlaybackState directive {#ExpectReportPlaybackState}

CLOVA가 현재 재생 상태를 보고하도록 클라이언트에 지시합니다. 클라이언트는 이 지시 메시지를 받으면 [`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState) 이벤트 메시지를 CIC로 전송해야 합니다.

### Payload fields

없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ExpectReportPlaybackState",
      "dialogRequestId": "9d7dc3ca-17ff-4df9-9800-91736ba2a3b6",
      "messageId": "46ccdf6c-609a-43a5-91c4-7a43b961f0c0"
    },
    "payload": {}
  }
}
```

### See also

* [`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState)
* [음원 재생 상태 공유하기](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)

<!-- Start of the shared content: AudioPlayer.Play -->

## Play directive {#Play}

CLOVA가 특정 오디오 스트림을 재생하거나 재생 대기열에 추가하도록 클라이언트에 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `audioItem`               | object | 재생할 오디오 스트림의 메타 정보와 재생에 필요한 오디오 스트림 정보를 담고 있는 객체                     | 항상 |
| `audioItem.artImageUrl`   | string | 오디오 콘텐츠 관련 이미지(앨범 이미지)의 URI                                                  | 조건부  |
| `audioItem.audioItemId`   | string | 오디오 스트림 정보를 구분하는 ID. 클라이언트는 이 값을 기준으로 중복된 Play 지시 메시지를 제거할 수 있습니다. | 항상 |
| `audioItem.headerText`    | string | 주로 현재 재생 목록의 제목을 표현하는 텍스트 필드                                                | 조건부  |
| `audioItem.stream`        | [AudioStreamInfoObject](#AudioStreamInfoObject) | 재생에 필요한 오디오 스트림 정보를 담고 있는 객체        | 항상 |
| `audioItem.titleSubText1` | string | 주로 아티스트 이름을 표현하는 텍스트 필드                                                          | 항상 |
| `audioItem.titleSubText2` | string | 주로 앨범 이름을 표현하는 보조 텍스트 필드                                                      | 조건부 |
| `audioItem.titleText`     | string | 현재 음악의 제목을 표현하는 텍스트 필드                                                         | 항상  |
| `audioItem.type`          | string | **(Deprecated)** 음악 서비스 구분자. 음악 스트리밍 서비스를 제공하는 사업자나 서비스 이름입니다. 이 필드 값은 각 서비스마다 달라지는 audioItem 객체의 필드를 파악하고 이를 분석하는 파서(parser)를 선택하는데 이용될 수 있습니다. | 항상 |
| `playBehavior`            | string | 지시 메시지에 포함된 오디오 스트림을 클라이언트에서 언제 재생할지를 결정하는 구분자 <ul><li><code>"REPLACE_ALL"</code>: 재생 대기열을 모두 비우고, 전달받은 오디오 스트림을 즉시 재생합니다.</li><li><code>"ENQUEUE"</code>: 재생 대기열에 전달받은 오디오 스트림을 추가합니다.</li></ul> | 항상 |
| `source`                  | object | 오디오 스트리밍 서비스의 출처 정보                                                    | 항상 |
| `source.logoUrl`          | string | 오디오 스트리밍 서비스의 로고 이미지의 URI. 이 필드 또는 필드의 값이 없거나 로고 이미지를 표시할 수 없으면 `source.name` 필드에 있는 오디오 스트리밍 서비스의 이름이라도 표시해야 합니다.  | 조건부 |
| `source.name`             | string | 오디오 스트리밍 서비스의 이름                                                        | 항상 |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>직관적인 사용자 경험을 위해 가능하면 `source.name`보다 `source.logoUrl`을 사용하는 것을 권장합니다.</p>
</div>

### Remarks

음악 서비스의 과금과 같은 문제로 실제 스트리밍 정보, 즉 스트리밍 URI와 같은 정보는 재생 직전에 획득해야 할 수 있습니다. 이는 `audioItem.stream.urlPlayable` 필드 값에 따라 다음과 같이 구분됩니다.
* `urlPlayable` 필드 값이 `true`이면 `audioItem.stream.url` 필드에 포함된 URI로 오디오 스트림을 바로 재생할 수 있습니다.
* `urlPlayable` 필드 값이 `false`이면 `audioItem.stream.url` 필드에 포함된 URI로 오디오 스트림을 바로 재생할 수 없고 [`AudioPlayer.StreamRequested`](#StreamRequested) 이벤트 메시지를 사용하여 오디오 스트림 정보를 추가로 요청해야 합니다.

### Message example

```json
// 바로 재생 가능한 오디오 스트림 URI 정보가 담긴 예제
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      "dialogRequestId": "34abac3-cb46-611c-5111-47eab87b7",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
    },
    "payload": {
      "audioItem": {
        "audioItemId": "90b77646-93ab-444f-acd9-60f9f278ca38",
        "episodeId": 22346122,
        "stream": {
          "beginAtInMilliseconds": 419704,
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": 60000,
            "progressReportPositionInMilliseconds": null
          },
          "token": "eyJ1cmwiOiJodHRwczovL2FwaS1leC5wb2RiYmFuZy5jb20vY2xvdmEvZmlsZS8xMjU0OC8yMjYxODcwMSIsInBsYXlUeXBlIjoiTk9ORSIsInBvZGNhc3RJZCI6MTI1NDgsImVwaXNvZGVJZCI6MjI2MTg3MDF9",
          "url": "https://streaming.example.com/clova/file/12548/22618701",
          "urlPlayable": true
        },
        "type": "podcast"
      },
      "source": {
        "name": "Potbbang",
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png"
      },
      "playBehavior": "REPLACE_ALL"
    }
  }
}

// 바로 재생 가능하지 않은 오디오 스트림 URI 정보가 담긴 예제
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "audioItem": {
        "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
        "album": {
          "albumId": "2000240",
          "genres": [
            "Classic"
          ],
          "title": "Wonderland - Edvard Grieg : Piano Concerto, Lyric Pieces"
        },
        ...
        "stream": {
          "beginAtInMilliseconds": 0,
          "durationInMilliseconds": 60000,
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-17716562",
          "url": "clova:TR-NM-17716562",
          "urlPlayable": false
        },
        "title": "Symphony No.4 In A Op.90 'Italian' - III. Con Moto Moderato",
        "type": "SampleMusicProvider"
      },
      "source": {
        "name": "Sample Music Provider",
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png"
      },
      "playBehavior": "REPLACE_ALL"
    }
  }
}
```

### See also

* [`AudioPlayer.PlayPaused`](#PlayPaused)
* [`AudioPlayer.PlayResumed`](#PlayResumed)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`AudioPlayer.PlayStopped`](#PlayStopped)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
* [음원 재생하기](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)

<!-- End of the shared content -->

## PlaybackQueueCleared event {#PlaybackQueueCleared}

클라이언트가 CIC로부터 [`AudioPlayer.ClearQueue`](#ClearQueue) 지시 메시지를 받았다면 재생 대기열(queue)를 초기화한 후 `PlaybackQueueCleared` 이벤트 메시지를 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `clearBehavior` | string | [`AudioPlayer.ClearQueue`](#ClearQueue) 지시 메시지의 `clearBehavior` 필드 값. 초기화 동작을 결정하는 구분자로서 해당 내용에 상응하는 작업을 처리한 후 이 필드 값을 채워 보내야 합니다.<ul><li><code>"CLEAR_ALL"</code>: 재생 대기열을 모두 비우고, 현재 재생 중인 오디오 스트림의 재생을 즉시 멈춥니다.</li><li><code>"CLEAR_ENQUEUED"</code>: 재생 대기열만 비우고, 현재 재생 중인 오디오 스트림은 계속 재생합니다.</li></ul> | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlaybackQueueCleared",
      "messageId": "1e1d8d52-9b51-454c-9fac-7597dc5f5246"
    },
    "payload": {
        "clearBehavior": "CLEAR_ALL"
    }
}
}
```

### See also

* [`AudioPlayer.ClearQueue`](#ClearQueue)

<!-- Start of the shared content: AudioPlayer.PlayFinished -->

## PlayFinished event {#PlayFinished}

클라이언트가 오디오 스트림 재생을 완료할 때 재생 완료된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 현재 재생하고 있는 음원의 재생 시점. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다.                       | 필수  |
| `token`                | string | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.stream.token` 필드 값 | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayFinished",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 183000
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [음원 재생 경과 보고하기](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayPaused -->

## PlayPaused event {#PlayPaused}

클라이언트가 오디오 스트림 재생을 일시 정지할 때 일시 정지된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다. 이 이벤트 메시지를 보내기 위해 필요한 사전 시나리오는 다음과 같습니다.

1. 클라이언트는 [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) 이벤트 메시지로 오디오 스트림 재생을 일시 정지하도록 요청하는 사용자의 음성을 CIC로 전송합니다.
2. CIC는 CLOVA 플랫폼에서 인식된 일시 정지 요청을 [`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause) 지시 메시지를 통해 클라이언트에 전달합니다.
3. 클라이언트는 오디오 스트림 재생을 일시 정지하고 PlayPaused 이벤트 메시지를 CIC에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 현재 재생하고 있는 음원의 재생 시점. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다.                         | 필수  |
| `token`                | string | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.stream.token` 필드 값 | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayPaused",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayResumed`](#PlayResumed)
* [`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayResumed -->

## PlayResumed event {#PlayResumed}

클라이언트가 오디오 스트림 재생을 재개할 때 재개된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다. 이 이벤트 메시지를 보내기 위해 필요한 사전 시나리오는 다음과 같습니다.

1. 클라이언트는 [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) 이벤트 메시지로 오디오 스트림 재생을 재개하도록 요청하는 사용자의 음성을 CIC로 전송합니다.
2. CIC는 CLOVA 플랫폼에서 인식된 재생 재개 요청을 [`PlaybackController.Resume`](/Develop/References/MessageInterfaces/PlaybackController.md#Resume) 지시 메시지를 통해 클라이언트에 전달합니다.
3. 클라이언트는 오디오 스트림 재생을 재개하고 PlayResumed 이벤트 메시지를 CIC에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 현재 재생하고 있는 음원의 재생 시점. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다.                         | 필수  |
| `token`                | string | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.stream.token` 필드 값 | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayResumed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayPaused`](#PlayPaused)
* [`PlaybackController.Resume`](/Develop/References/MessageInterfaces/PlaybackController.md#Resume)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayStarted -->

## PlayStarted event {#PlayStarted}

클라이언트가 오디오 스트림 재생을 시작할 때 재생이 시작된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 현재 재생하고 있는 음원의 재생 시점. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다.                         | 필수  |
| `token`                | string | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.stream.token` 필드 값 | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayStarted",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 0
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayStopped`](#PlayStopped)
* [음원 재생하기](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayStopped -->

## PlayStopped event {#PlayStopped}

클라이언트가 오디오 스트림 재생을 중지할 때 재생이 중지된 오디오 스트림 정보를 CIC로 보고하기 위해 사용됩니다. 이 이벤트 메시지를 보내기 위해 필요한 사전 시나리오는 다음과 같습니다.

1. 클라이언트는 [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) 이벤트 메시지로 오디오 스트림 재생을 중지하도록 요청하는 사용자의 음성을 CIC로 전송합니다.
2. CIC는 CLOVA 플랫폼에서 인식된 중지 요청을 [`PlaybackController.Stop`](/Develop/References/MessageInterfaces/PlaybackController.md#Stop) 지시 메시지를 통해 클라이언트에 전달합니다.
3. 클라이언트는 오디오 스트림 재생을 중지하고 PlayStopped 이벤트 메시지를 CIC에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 현재 재생하고 있는 음원의 재생 시점. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다.                         | 필수  |
| `token`                | string | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.stream.token` 필드 값 | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayStopped",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`PlaybackController.Stop`](/Develop/References/MessageInterfaces/PlaybackController.md#Stop)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportDelayPassed -->

## ProgressReportDelayPassed event {#ProgressReportDelayPassed}

오디오 스트림 재생이 시작된 후 지정된 지연 시간만큼 시간이 지났을 때 현재 재생 상태([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState))를 CIC로 보고하기 위해 사용됩니다. 각 오디오 스트림의 지연 시간은 [`AudioPlayer.Play`](#Play) 지시 메시지가 클라이언트로 전달될 때 확인할 수 있습니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 현재 재생하고 있는 음원의 재생 시점. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다.                         | 필수  |
| `token`                | string | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.stream.token` 필드 값 | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ProgressReportDelayPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 60000
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [음원 재생 경과 보고하기](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportIntervalPassed -->

## ProgressReportIntervalPassed event {#ProgressReportIntervalPassed}

오디오 스트림 재생이 시작된 후 지정된 간격마다 현재 재생 상태([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState))를 CIC로 보고하기 위해 사용됩니다. 각 오디오 스트림의 보고 간격은 [`AudioPlayer.Play`](#Play) 지시 메시지가 클라이언트로 전달될 때 확인할 수 있습니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 현재 재생하고 있는 음원의 재생 시점. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다.                         | 필수  |
| `token`                | string | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.stream.token` 필드 값 | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ProgressReportIntervalPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 120000
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [음원 재생 경과 보고하기](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportPositionPassed -->

## ProgressReportPositionPassed event {#ProgressReportPositionPassed}

오디오 스트림 재생이 시작된 후 지정된 보고 시점에 현재 재생 상태([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState))를 CIC로 보고하기 위해 사용됩니다. 각 오디오 스트림의 보고 시점은 [`AudioPlayer.Play`](#Play) 지시 메시지가 클라이언트로 전달될 때 확인할 수 있습니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 현재 재생하고 있는 음원의 재생 시점. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다.                         | 필수  |
| `token`                | string | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.stream.token` 필드 값 | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ProgressReportPositionPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 150000
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [음원 재생 경과 보고하기](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

## ReportPlaybackState event {#ReportPlaybackState}

클라이언트의 현재 음원 재생 상태를 CIC로 보고합니다. 클라이언트가 CIC로부터 [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState) 지시 메시지를 받았을 때 `AudioPlayer.ReportPlaybackState` 이벤트 메시지를 CIC로 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds`   | number  | 현재 재생하고 있는 음원의 재생 시점. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다.  | 선택  |
| `playerActivity`         | string  | 오디오 플레이어의 현재 상태.<ul><li><code>"IDLE"</code>: 미사용 상태</li><li><code>"PLAYING"</code>: 재생 중인 상태</li><li><code>"PAUSED"</code>: 일시 중지된 상태</li><li><code>"STOPPED"</code>: 정지된 상태</li></ul>  | 필수  |
| `repeatMode`             | string  | 반복 재생 모드.<ul><li><code>"NONE"</code>: 반복 재생 안함</li><li><code>"REPEAT_ONE"</code>: 한 곡 반복 재생</li></ul>  | 필수  |
| `stream`                 | [AudioStreamInfoObject](#AudioStreamInfoObject) | Play 지시 메시지의 `audioItem.stream` <div class="note"><p><strong>Note!</strong></p><p>오디오 플레이어의 현재 상태(<code>playerActivity</code> 필드)가 <code>"IDLE"</code>일 때만 이 필드가 생략될 수 있고 그 외 상태이면 이 필드에 값을 반드시 입력해야 합니다.</p></div>   | 선택 |
| `token`                  | string  | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.stream.token` 필드 값 <div class="note"><p><strong>Note!</strong></p><p>오디오 플레이어의 현재 상태(<code>playerActivity</code> 필드)가 <code>"IDLE"</code>일 때만 이 필드가 생략될 수 있고 그 외 상태이면 이 필드에 값을 반드시 입력해야 합니다.</p></div>                   | 선택 |
| `totalInMilliseconds`    | number | 최근 재생 미디어의 전체 길이. [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지를 통해 전달받은 오디오 정보([AudioStreamInfoObject](/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject))에 `durationInMilliseconds` 필드 값이 있으면 이 필드의 값으로 입력하면 됩니다. 단위는 밀리 초이며, `0` 이상의 정수 값을 입력합니다. `playerActivity` 값이 `"IDLE"`이면 이 필드 값은 입력하지 않아도 됩니다. | 선택 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ReportPlaybackState",
      "messageId": "3e57f11a-a02d-4b6f-b183-fdfb6105c650"
    },
    "payload": {
      "playerActivity": "IDLE",
      "repeatMode": "NONE"
    }
  }
}
```

### See also

* [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState)
* [`AudioPlayer.Play`](#Play)
* [음원 재생 상태 공유하기](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)

<!-- Start of the shared content: AudioPlayer.StreamDeliver -->

## StreamDeliver directive {#StreamDeliver}

CLOVA가 오디오 스트림 정보를 교체하여 재생하도록 클라이언트에 지시합니다. [`AudioPlayer.StreamRequested`](#StreamRequested) 이벤트 메시지의 응답이며, 실제 음악 재생이 가능한 오디오 스트림 정보를 수신해야 할 때 사용합니다. 클라이언트가 음악을 재생할 수 있도록 오디오 스트림 정보에 스트리밍할 수 있는 URI 정보가 필수로 포함되어 있습니다.

### Payload fields

| 필드 이름 | 자료형 | 필드 설명 | 포함 여부 |
|---------|------|--------|:---------:|
| `audioItemId` | string | 오디오 스트림 정보를 구분하는 값. 클라이언트는 이 값을 기준으로 중복된 Play 지시 메시지를 제거할 수 있습니다. | 항상 |
| `audioStream` | [AudioStreamInfoObject](#AudioStreamInfoObject) | 재생에 필요한 오디오 스트림 정보를 담고 있는 객체       | 항상 |

### Remarks

`StreamDeliver` 지시 메시지를 통해 전달받는 `AudioStreamInfoObject` 객체는 기존 [`AudioPlayer.Play`](#Play) 지시 메시지를 통해 전달받은 `AudioStreamInfoObject` 객체의 내용과 중복을 피하기 위해 일부 내용이 생략될 수 있습니다. 따라서, 음원을 재생할 때 `StreamDeliver` 지시 메시지와 이미 수신한 [`Play`](#Play) 지시 메시지의 `payload.audioStream` 정보를 조합해서 사용해야 합니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "StreamDeliver",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
      "audioStream": {
          "format": "audio/mpeg",
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-17716562",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
      }
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
* [음원 재생하기](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.StreamRequested -->

## StreamRequested event {#StreamRequested}

오디오 스트림 재생을 위해 CIC로 스트리밍 URI와 같은 추가 정보를 요청하는 이벤트 메시지입니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `audioItemId`   | string  | [`AudioPlayer.Play`](#Play) 지시 메시지의 `audioItem.audioItemId` 필드 값       | 필수 |
| `audioStream`   | [AudioStreamInfoObject](#AudioStreamInfoObject) | Play 지시 메시지의 `audioItem.stream` | 필수 |

### Remarks

음악 서비스의 과금과 같은 것 고려하여 실제 오디오 스트림 정보 발급을 재생 직전으로 지연 해야 할 때가 있습니다. 이 이벤트 메시지는 이처럼 오디오 스트림 정보를 미리 준비하면 안될 때를 위해 설계된 API이며, 따라서 클라이언트는 이 이벤트 메시지를 재생 직전 시점보다 일찍 전달하면 안됩니다.

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "StreamRequested",
      "messageId": "198cf12-4020-b98a-b73b-1234ab312806"
    },
    "payload": {
      "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
      "audioStream": {
        "beginAtInMilliseconds": 0,
        "durationInMilliseconds": 60000,
        "progressReport": {
          "progressReportDelayInMilliseconds": null,
          "progressReportIntervalInMilliseconds": null,
          "progressReportPositionInMilliseconds": 60000
        },
        "token": "TR-NM-17716562",
        "url": "clova:TR-NM-17716562",
        "urlPlayable": false
      }
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.StreamDeliver`](#StreamDeliver)
* [음원 재생하기](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)

<!-- End of the shared content -->

## Shared objects

AudioPlayer API를 이용하여 이벤트 메시지나 지시 메시지를 보낼 때 메시지 본문(`payload`)에 다음과 같은 객체(shared objects)를 공유하여 사용합니다.

| 객체 이름            | 객체 설명                                            |
|--------------------|---------------------------------------------------|
| [AudioStreamInfoObject](#AudioStreamInfoObject) | 재생할 음악의 스트리밍 주소, 인증 정보와 같은 재생 정보가 담긴 객체 |

### AudioStreamInfoObject {#AudioStreamInfoObject}

재생할 음악의 스트리밍 정보를 담고 있는 객체입니다. 클라이언트에 재생할 스트리밍 정보를 전달하거나 클라이언트가 CIC로 현재 재생 중인 음악의 스트리밍 정보를 전달해야 할 때 사용합니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수/포함 여부 |
|---------------|---------|-----------------------------|:-------------:|
| `beginAtInMilliseconds`  | number | 재생을 시작할 지점. 단위는 밀리 초이며, `0` 이상의 정수 값입니다. 이 값이 지정되면 클라이언트는 해당 오디오 스트림을 지정된 위치부터 재생해야 합니다. 이 값이 0이면 해당 스트림을 처음부터 재생해야 합니다.          | 필수/항상 |
| `customData`             | string | 현재 음원과 관련하여 임의의 형식을 가지는 메타 데이터 정보. 특정 범주로 분류되거나 정의될 수 없는 스트리밍 정보는 이 필드에 포함되거나 입력되어야 합니다. 오디오 스트림 재생 문맥에 추가로 필요한 값을 서비스 제공자 임의대로 추가할 수 있습니다.<div class="warning"><p><strong>Warning!</strong></p><p>이 필드의 값을 클라이언트가 임의로 이용해서는 안되며 이는 문제를 발생시킬 수 있습니다. 또한, 이 필드 값은 오디오 재생 상태를 전달할 때 <a href="/Develop/References/Context_Objects.md#PlaybackState">PlaybackState 문맥 정보</a>의 <code>stream</code> 필드에 그대로 첨부되어야 합니다.</p></div> | 선택/조건부  |
| `durationInMilliseconds` | number | 오디오 스트림의 재생 시간. 클라이언트는 `beginAtInMilliseconds` 필드에 지정된 재생 시작 시점부터 이 필드에 지정된 재생 시간만큼 해당 오디오 스트림을 탐색 및 재생할 수 있습니다. 예를 들면, `beginAtInMilliseconds` 필드의 값이 `10000`이고, 이 필드의 값이 `60000`이면 해당 오디오 스트림의 10 초부터 70 초까지의 구간을 재생 및 탐색할 수 있게 됩니다. 단위는 밀리 초이며, `0` 이상의 정수 값입니다.   | 선택/조건부  |
| `format`                 | string  | 미디어 포맷(MIME 타입). 이 필드를 통해 HLS(HTTP Live Streaming) 방식의 콘텐츠인지 구분할 수 있습니다. 다음과 같은 값을 가질 수 있습니다. 기본 값은 `"audio/mpeg"`입니다.<ul><li><code>"audio/mpeg"</code></li><li><code>"audio/mpegurl"</code></li><li><code>"audio/aac"</code></li><li><code>"application/vnd.apple.mpegurl"</code></li></ul> <div class="note"><p><strong>Note!</strong></p><p>HLS 방식으로 콘텐츠를 제공하려는 extension 개발자는 <a href="mailto:{{ book.ServiceEnv.ExtensionAdminEmail }}">{{ book.ServiceEnv.ExtensionAdminEmail }}</a>로 연락합니다.</p></div>   | 선택/조건부  |
| `progressReport`         | object  | 재생 후 재생 상태 정보를 보고 받기 위해 보고 시간을 정해 둔 객체                                                  | 선택/조건부 |
| `progressReport.progressReportDelayInMilliseconds`    | number | 재생 시작 후 지정된 시간이 지났을 때 재생 상태 정보를 보고받기 위해 지정되는 값입니다. 단위는 밀리 초이며, `0` 이상의 정수 값입니다. 이 필드는 null 값을 가질 수 있습니다.  | 선택/조건부 |
| `progressReport.progressReportIntervalInMilliseconds` | number | 재생 중 지정된 시간 간격으로 재생 상태 정보를 보고받기 위해 지정되는 값입니다. 단위는 밀리 초이며, `0` 이상의 정수 값입니다. 이 필드는 null 값을 가질 수 있습니다.        | 선택/조건부 |
| `progressReport.progressReportPositionInMilliseconds` | number | 재생 중 지정된 시점을 지날 때마다 재생 상태 정보를 보고받기 위해 지정되는 값입니다. 단위는 밀리 초이며, `0` 이상의 정수 값입니다. 이 필드는 null 값을 가질 수 있습니다.    | 선택/조건부 |
| `token`                  | string  | 오디오 스트림 token. <div class="note"><p><strong>Note!</strong></p><p>이 필드의 최대 길이는 2048 바이트입니다.</p></div>                          | 필수/항상 |
| `url`                    | string  | 오디오 스트림 URI<div class="note"><p><strong>Note!</strong></p><p>제공하려는 오디오 콘텐츠는 <a href="/Design/UI/Audio.md#SupportedAudioFormat">플랫폼이 지원하는 오디오 압축 포맷</a>이어야 합니다.</p></div><div class="note"><p><strong>Note!</strong></p><p>이 필드의 최대 길이는 2048 바이트입니다.</p></div>  | 필수/항상 |
| `urlPlayable`            | boolean | `url` 필드의 오디오 스트림 URI가 바로 재생 가능한 형태인지 구분하는 값. <ul><li><code>true</code>: 바로 재생이 가능한 형태의 URI</li><li><code>false</code>: 바로 재생이 불가능한 형태의 URI. <a href="#StreamRequested"><code>AudioPlayer.StreamRequested</code></a> 이벤트 메시지를 사용하여 오디오 스트림 정보를 추가로 요청해야 합니다.</li></ul>        | 필수/항상 |

#### Remarks

* 클라이언트가 `beginAtInMilliseconds`와 `durationInMilliseconds` 필드에 지정된 구간에 대해 음악 재생을 완료하면 [`AudioPlayer.PlayFinished`](#PlayFinished) 이벤트 메시지를 CIC로 전송해야 합니다.
* 클라이언트는 `beginAtInMilliseconds`와 `durationInMilliseconds` 필드에 지정된 구간 이외의 시간을 사용자가 탐색(seek)할 수 없도록 UI를 제공해야 합니다.
* 클라이언트가 현재 재생 중인 상태를 CIC에 보고할 때 [`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)의 `totalInMilliseconds` 필드 값은 `beginAtInMilliseconds` 필드와 `durationInMilliseconds` 필드에 지정된 값을 더한 값으로 입력하면 됩니다.

#### Object Example

```json
// 바로 재생 가능한 오디오 스트림 URI 정보가 담긴 객체
{
  "beginAtInMilliseconds": 0,
  "progressReport": {
    "progressReportDelayInMilliseconds": null,
    "progressReportIntervalInMilliseconds": 60000,
    "progressReportPositionInMilliseconds": null
  },
  "url": "https://api-ex.example.com/file/12548/22346122",
  "urlPlayable": true
}

// 바로 재생 가능하지 않은 오디오 스트림 URI 정보가 담긴 예제
{
  "beginAtInMilliseconds": 0,
  "progressReport": {
      "progressReportDelayInMilliseconds": null,
      "progressReportIntervalInMilliseconds": null,
      "progressReportPositionInMilliseconds": 60000
  },
  "token": "TR-NM-4435786",
  "urlPlayable": false,
  "url": "clova:TR-NM-4435786"
}
```

#### See also

* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayFinished`](#PlayFinished)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
