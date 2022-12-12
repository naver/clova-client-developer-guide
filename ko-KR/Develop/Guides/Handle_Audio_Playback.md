# 음원 재생 처리하기

CLOVA는 사용자 요청에 따라 음원을 재생하거나 재생과 관련된 지시 메시지를 클라이언트로 보냅니다. 클라이언트는 CLOVA가 음원 재생과 관련하여 전달하는 메시지를 처리하고 클라이언트의 음원 재생 상태를 이벤트 메시지를 이용해 CLOVA로 보내야 합니다. 특히, 이벤트 메시지를 CLOVA에 전송하여 CLOVA가 클라이언트의 재생 상태를 파악하게 하는 것은 중요하며, 상황이나 조건에 맞는 이벤트 메시지 이용하여 재생 상태를 보고해야 합니다. 여기서 설명할 내용은 다음과 같습니다.

* [음원 재생하기](#PlayAudioStream)
* [음원 재생 경과 보고하기](#ReportAudioPlaybackProgress)
* [음원 재생 제어하기](#ControlAudioPlayback)
* [음원 재생 상태 공유하기](#ShareAudioPlaybackState)
* [음원 정보 관리하기](#ManageStreamInfo)

## 음원 재생하기 {#PlayAudioStream}
사용자가 음악 재생을 요청하면 CLOVA는 사용자가 요청한 음원을 재생하도록 클라이언트에 지시합니다. 음원을 재생하는 동작의 흐름은 다음과 같습니다.

![](/Develop/Assets/Images/CIC_Audio_Play_Work_Flow.svg)

사용자가 음원 재생을 요청하면 가장 먼저 CLOVA는 클라이언트에 [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지를 보냅니다. 이 지시 메시지에는 오디오 재생에 필요한 정보가 포함되어 있으며, 이 정보를 활용하여 음원 데이터를 찾거나 오디오 플레이어에 음원 정보를 표시해야 합니다. 다음과 같은 [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지를 받을 수 있습니다.

```json
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
```

`payload`의 주요 필드를 간략히 설명하면 다음과 같습니다.

* `audioItem`: 재생해야하는 음원의 메타 정보를 포함하고 있는 객체
* `audioItem.stream`: 재생에 필요한 오디오 스트림 정보를 담고 있는 객체
* `source`: 오디오 스트리밍 서비스의 출처

클라이언트는 `audioItem.stream` 필드에 포함된 정보를 이용하여 음원을 재생하고 그 외 `audioItem` 필드와 `source` 필드의 내용을 오디오 플레이어 UI에 표시하거나 참고 정보로 활용하면 됩니다.

만약 전달받은 [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지의 `audioItem.stream.urlPlayable` 필드의 값이 `false`로 설정되어 있다면 `audioItem.stream` 필드의 정보로 음원을 바로 재생할 수 없습니다. 이는 서비스 과금이나 보안과 같은 문제로 음원을 실제 사용자에게 들려주기 직전에 한번 더 실제 음원 정보를 조회해야 하는 상황에 발생합니다. 바로 재생할 수 없는 [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지의 예는 다음과 같습니다.

```json
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

따라서 `audioItem.stream.urlPlayable` 필드의 값이 `false`로 설정되어 있다면 [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지로 받았던 `audioItem.stream` 필드의 값을 그대로 사용하여 다음과 같은 [`AudioPlayer.StreamRequested`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamRequested) 이벤트 메시지를 음원 재생 직전에 CLOVA에 한번 더 전송해야 합니다.

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

위와 같이 [`AudioPlayer.StreamRequested`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamRequested) 이벤트 메시지를 보내면 아래와 같이 실제 재생가능한 정보가 포함된 [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver) 지시 메시지를 CLOVA로부터 받게 됩니다. 클라이언트는 새로 받게되는 정보를 이용하여 음원을 재생하면 됩니다.

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

<div class="note">
  <p><strong>Note!</strong></p>
  <p>음원 재생 정보는 항상 최신의 정보를 유지해야 하고 계속 관리되어야 합니다. 자세한 설명은 <a href="#ManageStreamInfo">음원 정보 관리하기</a>를 참조합니다.</p>
</div>


## 음원 재생 경과 보고하기 {#ReportAudioPlaybackProgress}

CLOVA가 사용자가 현재 음원 재생과 관련하여 어떤 상황에 있는지 이해하기 위해 클라이언트는 음원 재생을 시작한 후 다음과 같이 재생 경과 보고를 해야 합니다.

![](/Develop/Assets/Images/CIC_Audio_Play_Progress_Reporting.svg)

위 동작 흐름과 같이 일부 재생 경과 보고는 [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지의 `audioItem.stream.progressReport` 필드나 [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver) 지시 메시지의 `audioStream.progressReport` 필드에 설정된 값에 따라 보고 여부가 달라집니다. 다음 표는 재생 경과 보고를 해야하는 상황과 조건 그리고 사용해야 하는 이벤트 메시지를 나타냅니다.

| 재생 시점                                 | 조건                         | 보고를 위한 이벤트 메시지 |
|-----------------------------------------|----------------------------|----------------------|
| 음원 재생 시작 직후                         | 항상                                                                      | [`AudioPlayer.PlayStarted`](/Develop/References/MessageInterfaces/AudioPlayer.md#PlayStarted) 이벤트 메시지  |
| 음원 재생 시작 후 지정된 시간을 지날 때          | `progressReport.progressReportDelayInMilliseconds` 필드가 `null`이 아닐 때   | [`AudioPlayer.ProgressReportDelayPassed`](/Develop/References/MessageInterfaces/AudioPlayer.md#ProgressReportDelayPassed) 이벤트 메시지  |
| 음원 재생 시작 후 지정된 간격의 시간이 지날 때마다  | `progressReport.progressReportIntervalInMilliseconds` 필드가 `null`이 아닐 때  | [`AudioPlayer.ProgressReportIntervalPassed`](/Develop/References/MessageInterfaces/AudioPlayer.md#ProgressReportIntervalPassed) 이벤트 메시지  |
| 음원 재생 시작 후 음원의 특정 재생 시점을 지날 때  | `progressReport.progressReportPositionInMilliseconds` 필드가 `null`이 아닐 때  | [`AudioPlayer.ProgressReportPositionPassed`](/Develop/References/MessageInterfaces/AudioPlayer.md#ProgressReportPositionPassed) 이벤트 메시지  |
| 음원 재생 완료 직후                         | 항상                                                                      | [`AudioPlayer.PlayFinished`](/Develop/References/MessageInterfaces/AudioPlayer.md#PlayFinished) 이벤트 메시지  |

<div class="note">
  <p><strong>Note!</strong></p>
  <p><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play"><code>AudioPlayer.Play</code></a> 지시 메시지를 받았을 때 <code>audioItem.stream.urlPlayable</code> 필드가 <code>false</code>이면, 추후 <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver"><code>AudioPlayer.StreamDeliver</code></a> 지시 메시지로 음원 정보가 새롭게 전달될 수 있습니다. 만약, <code>AudioPlayer.Play</code> 지시 메시지 이후 받게되는 <code>AudioPlayer.StreamDeliver</code> 지시 메시지에서 <code>progressReport.progressReportDelayInMilliseconds</code>, <code>progressReport.progressReportIntervalInMilliseconds</code>, <code>progressReport.progressReportPositionInMilliseconds</code> 필드의 값이 변경되었다면 변경된 값을 적용하여 음원 재생 경과를 보고해야 합니다. 자세한 설명은 <a href="#ManageStreamInfo">음원 정보 관리하기</a>를 참조합니다.</p>
</div>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>진행 표시줄의 탐택(seek)으로 재생 중인 위치가 변경되면 다음과 같이 <code>ProgressReportPositionPassed</code> 이벤트 메시지를 보내야 합니다.<ul><li>진행 표시줄의 이동으로 지시 받은 보고 시점을 지났을 때: 진행 표시줄의 위치가 변경된 후 즉시 <code>ProgressReportPositionPassed</code> 이벤트 메시지를 보내야 합니다. 이 때, <code>payload.offsetInMilliseconds</code>는 지시 받은 보고 시점의 값을 설정합니다.</li><li>진행 표시줄의 이동으로 보고 시점 이전으로 돌아갔을 때: 보고 시점을 지날 때 <code>ProgressReportPositionPassed</code> 이벤트 메시지를 다시 보내야 합니다.</li></ul></p>
</div>

CLOVA는 위 이벤트 메시지를 통해 사용자가 현재 듣고 있는 음원과 해당 음원의 재생 시점을 파악하게 됩니다. 다음은 [`AudioPlayer.ProgressReportIntervalPassed`](/Develop/References/MessageInterfaces/AudioPlayer.md#ProgressReportIntervalPassed) 이벤트 메시지의 예입니다. 이때, [음원 재생 관련 맥락 정보도 함께 작성](#BuildPlaybackStateContext)해서 전송해야 합니다.

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "offsetInMilliseconds": 10000,
        "totalInMilliseconds": 300000,
        "playerActivity": "PLAYING",
        "repeatMode": "NONE",
        "stream": {
          "beginAtInMilliseconds": 0,
          "format": "audio/mpeg",
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": 120000,
            "progressReportPositionInMilliseconds": 600000
          },
          "token": "TR-NM-4435786",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
        }
      }
    }
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



## 음원 재생 제어하기 {#ControlAudioPlayback}

사용자는 클라이언트가 음원 재생을 일시 정지(pause), 재개(resume), 중지(stop), 다시 재생(replay)하도록 CLOVA에 요청할 수 있습니다. 이런 음원 재생 제어는 다음과 같이 세 가지 방식으로 요청될 수 있습니다.

* 사용자가 말로 재생 제어를 시도 (일반적인 방식)
* 사용자가 클라이언트 기기에서 버튼으로 재생 제어를 시도
* 사용자가 CLOVA 앱에서 원격으로 특정 클라이언트의 재생 제어를 시도

CLOVA는 사용자의 음원 재생 상황을 파악해야 하기 때문에 음원에 대한 재생 제어가 클라이언트에서 바로 수행되지 않습니다. 이런 재생 제어는 모두 CLOVA를 통해 지시 메시지로 수행되며 음원 재생 제어는 주로 [`PlaybackController`](/Develop/References/MessageInterfaces/PlaybackController.md) 인터페이스를 통해 구현되어야 합니다. 또 그 결과는 [`AudioPlayer`](/Develop/References/MessageInterfaces/AudioPlayer.md) 인터페이스의 이벤트 메시지를 통해 보고 되어야 합니다.

다음은 음원 재생이 일시 정지되는 동작의 흐름을 나타냅니다.

![](/Develop/Assets/Images/CIC_Audio_Playback_Control_Flow.svg)

일반적으로 사용자의 발화를 통한 재생 제어는 CLOVA가 분석하여 그에 상응하는 재생 제어 지시 메시지([`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause))가 클라이언트로 전달됩니다. 하지만, 사용자가 클라이언트에서 버튼을 눌러 일시 정지를 요청했다면 다음과 같은 [`PlaybackController.PauseCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued) 이벤트 메시지를 이용하여 일시 정지 버튼이 눌러졌음을 CLOVA에 보고해야 합니다.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PauseCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```

사용자가 CLOVA 앱을 이용해 원격으로 클라이언트의 재생 제어를 요청했다면 CLOVA는 해당 클라이언트에  [`PlaybackController.ExpectPauseCommand`](/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPauseCommand) 형태의 지시 메시지를 전송하게 됩니다. 이는 사용자가 마치 원격으로 버튼을 누른 것처럼 행동하도록 지시하는 메시지며 아래 메시지를 받으면 위와 같은 [`PlaybackController.PauseCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued) 이벤트 메시지를 CLOVA에 보내면 됩니다.

```json
// 사용자가 일시 정지 버튼을 누른 것처럼 동작하도록 요청
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPauseCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

CLOVA는 사용자가 요청한 재생 제어를 다음과 같은 지시 메시지([`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause))로 전달하며, 클라이언트는 지시 메시지에 맞게 음원 재생을 제어하면 됩니다.

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Pause",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

마지막으로 클라이언트는 재생 제어 완료 후 오디오 플레이어의 동작 변화를 CLOVA에 보고하면 됩니다. 다음은 음원 재생을 일시 정지한 후 CLOVA에 보내는 [AudioPlayer.PlayPaused](/Develop/References/MessageInterfaces/AudioPlayer.md#PlayPaused) 이벤트 메시지입니다. 이때, 오디오 플레이어의 상태에 맞게 [맥락 정보(context)가 작성](#BuildPlaybackStateContext)되어야 합니다.

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "offsetInMilliseconds": 83100,
        "totalInMilliseconds": 300000,
        "playerActivity": "PAUSED",
        "repeatMode": "NONE",
        "stream": {
          "beginAtInMilliseconds": 0,
          "format": "audio/mpeg",
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-4435786",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
        }
      }
    }
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

## 음원 정보 관리하기 {#ManageStreamInfo}

클라이언트는 [음원을 재생](#PlayAudioStream)할 때 CLOVA로부터 음원과 관련된 정보를 전달받습니다. 클라이언트는 CLOVA로부터 [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지를 받기 시작한 순간부터 해당 지시 메시지의 `audioItem.stream` 필드에 포함된 음원 정보를 잘 관리해야 합니다. 여기서는 다음에 대해 설명합니다.

* [음원 정보 업데이트하기](#UpdateStreamInfo)
* [음원 맥락 정보 작성하기](#BuildPlaybackStateContext)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>여기서 설명하는 내용을 지키지 않는 경우 음원을 제대로 재생할 수 없거나 또는 사용자가 음악을 다시 들려달라는 요청에 대해 CLOVA가 대응하지 못할 수 있습니다.</p>
</div>

### 음원 정보 업데이트하기 {#UpdateStreamInfo}

`AudioPlayer.Play` 지시 메시지를 받았다면 `audioItem.stream` 필드 이하의 값을 따로 저장합니다. 특히, `audioItem.stream.urlPlayable` 필드가 `false`이면, 실제 재생 가능한 음원 정보를 다시 얻기 위해 클라이언트는 `audioItem.stream` 필드 이하의 정보를 반드시 잘 보관하고 있어야 합니다.

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      ...
    },
    "payload": {
      "audioItem": {
        ...
        "stream": {
          "beginAtInMilliseconds": 0,
          "durationInMilliseconds": 60000,
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": 20000,
            "progressReportPositionInMilliseconds": 30000
          },
          "token": "TR-NM-17716562",
          "url": "clova:TR-NM-17716562",
          "urlPlayable": false
        },
        ...
      },
      ...
    }
  }
}
```

<div class="note">
  <p><strong>Note!</strong></p>
  <p>만약, <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play"><code>AudioPlayer.Play</code></a> 지시 메시지를 받았을 때 <code>audioItem.stream.urlPlayable</code> 필드가 <code>false</code>이면, <strong>추후 <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver"><code>AudioPlayer.StreamDeliver</code></a> 지시 메시지로 전달되는 음원 정보와 반드시 병합해야 합니다.<strong></p>
</div>

이후 [`AudioPlayer.StreamRequested`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamRequested) 이벤트 메시지를 이용해 음원 재생에 필요한 실제 정보를 요청합니다. 이때, 앞서 보관했던 `audioItem.stream` 필드 값을 함께 전송해야 합니다.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "StreamRequested",
      ...
    },
    "payload": {
      ...
      "audioStream": {
        "beginAtInMilliseconds": 0,
        "durationInMilliseconds": 60000,
        "progressReport": {
          "progressReportDelayInMilliseconds": null,
          "progressReportIntervalInMilliseconds": 20000,
          "progressReportPositionInMilliseconds": 30000
        },
        "token": "TR-NM-17716562",
        "url": "clova:TR-NM-17716562",
        "urlPlayable": false
      }
    }
  }
}
```

그러면 아래와 같이 실제 재생 가능한 정보가 포함된 [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver) 지시 메시지를 CLOVA로부터 받게됩니다.

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
            "progressReportDelayInMilliseconds": 3000,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 50000
          },
          "token": "TR-NM-17716562",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
      }
    }
  }
}
```

여기에 포함되어 있는 `audioStream` 필드 이하의 내용을 앞서 `AudioPlayer.Play` 지시 메시지를 통해 보관했던 `audioItem.stream` 필드 이하의 내용과  **반드시 병합해야 합니다.** 다음과 같이 병합합니다.

<table>
  <tr>
    <th colspan="2">메시지별 필드 포함 여부</th><th rowspan="2">병합 방법</th>
  </tr>
  <tr>
    <th><code>AudioPlayer.Play</code></th><th><code>AudioPlayer.StreamDeliver</code></th>
  </tr>
  <tr>
    <th>포함</th><th>미포함 또는 <code>null</code></th><td><code>AudioPlayer.Play</code> 지시 메시지에 포함되었던 필드의 내용을 변경하거나 삭제하지 않고 계속 유지합니다.</td>
  </tr>
  <tr>
    <th>미포함 또는 <code>null</code></th><th>포함</th><td><code>AudioPlayer.StreamDeliver</code> 지시 메시지에 포함된 필드를 새로 추가합니다.</td>
  </tr>
  <tr>
    <th>포함</th><th>포함</th><td>기존 필드의 내용을 <code>AudioPlayer.StreamDeliver</code> 지시 메시지에 포함된 필드의 내용으로 업데이트합니다.</td>
  </tr>
</table>

여기서 사용된 예제를 토대로 정리하면 다음과 같이 필드 정보를 업데이트해야 합니다.

| 필드 이름                                             | `AudioPlayer.Play` 값    | `AudioPlayer.StreamDeliver` 값 | 적용해야 하는 최종 값  |
|-------------------------------------------------------|:------------------------:|:------------------------------:|:----------------------:|
| `beginAtInMilliseconds`                               | `0`                      | 미포함                         | `0`                    |
| `durationInMilliseconds`                              | `60000`                  | 미포함                         | `60000`                |
| `format`                                              | 미포함                   | `"audio/mpeg"`                 | `"audio/mpeg"`         |
| `progressReport.progressReportDelayInMilliseconds`    | `null`                   | `3000`                         | `3000`                 |
| `progressReport.progressReportIntervalInMilliseconds` | `20000`                  | `null`                         | `20000`                |
| `progressReport.progressReportPositionInMilliseconds` | `30000`                  | `50000`                        | `50000`                |
| `token`                                               | `"TR-NM-17716562"`       | `"TR-NM-17716562"`             | `"TR-NM-17716562"`     |
| `url`                                                 | `"clova:TR-NM-17716562"` | `"https://musicservice.example.net/b767313e.mp3"` | `"https://musicservice.example.net/b767313e.mp3"` |
| `urlPlayable`                                         | `false`                  | `true`                         | `true`                 |

병합이 완료되면 음원 정보는 다음과 같이 [맥락 정보로 작성](#BuildPlaybackStateContext) 및 보고되어야 합니다.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      ...
    },
    "payload": {
      ...
      "audioStream": {
        "beginAtInMilliseconds": 0,
        "durationInMilliseconds": 60000,
        "format": "audio/mpeg",
        "progressReport": {
          "progressReportDelayInMilliseconds": 3000,
          "progressReportIntervalInMilliseconds": 20000,
          "progressReportPositionInMilliseconds": 50000
        },
        "token": "TR-NM-17716562",
        "url": "https://musicservice.example.net/b767313e.mp3",
        "urlPlayable": true
      }
    }
  }
}
```

### 음원 맥락 정보 작성하기 {#BuildPlaybackStateContext}

오디오 재생 상태는 클라이언트가 이벤트 메시지를 CLOVA에 전송할 때마다 맥락 정보(context)를 통해 보고되어야 합니다. 클라이언트의 오디오 플레이어가 비활성화된 상태라면 아래와 같이 `playerActivity` 필드 값을 `"IDLE"`로 입력하여 전송하면 됩니다.

```json
{
  "context": [
    ...
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
    ...
  ],
  "event": {
    ...
  }
}
```

만약, 오디오 플레이어가 활성화되어있는 상태라면 오디오 플레이어 상태에 따라 `playerActivity` 필드를 다음 값 중 하나로 선택하면 됩니다.

* `"PLAYING"`
* `"PAUSE"`
* `"STOPPED"`

이때, [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지나 [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver) 지시 메시지를 통해 받은 후 [관리하고 있는 음원 정보](#UpdateStreamInfo)를 맥락 정보에 함께 포함시켜야 합니다.

```json
{
  "context": [
    ...
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
    ...
  ],
  "event": {
    ...
  }
}
```

사용자가 음원 재생을 중지하도록 요청하여 오디오 플레이어의 상태가 `"PAUSE"`로 진입하더라도 보관하고 있던 음원 정보를 임의로 폐기해서는 안됩니다. 만약, 음원 정보를 임의로 폐기했다면 사용자가 음악 재생을 재요청했을 때 CLOVA는 클라이언트가 재생하던 음원에 대한 정보를 맥락 정보에서 파악할 수 없습니다. 이로 인해 CLOVA는 이어듣기와 같은 기능을 사용자에게 제공할 수 없을 수도 있습니다. 따라서, 클라이언트는 재부팅과 같은 특수한 상황을 제외하고는 음원 정보를 계속 유지해야 합니다. 음원 정보를 제거할 필요가 있을 경우 CLOVA가 클라이언트의 음원 정보를 없애도록 [AudioPlayer.ClearQueue](/Develop/References/MessageInterfaces/AudioPlayer.md#ClearQueue) 지시 메시지와 같은 지시 메시지를 보내게 됩니다.


## 음원 재생 상태 공유하기 {#ShareAudioPlaybackState}

한 클라이언트는 사용자 계정에 등록된 다른 모든 클라이언트 또는 특정 클라이언트로부터 음원 재생 상태를 공유받을 수 있습니다. 음원 재생 상태를 공유 받는 동작의 흐름은 다음과 같습니다.

![](/Develop/Assets/Images/CIC_Playback_State_Sync_Work_Flow.svg)

1. CLOVA 앱은 사용자 계정에 등록된 모든 클라이언트 또는 특정 클라이언트의 음원 재생 상태 정보를 CLOVA에 요청합니다.
2. CLOVA는 [`AudioPlayer.ExpectReportPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#ExpectReportPlaybackState) 지시 메시지를 이용하여 사용자 계정에 등록된 모든 클라이언트 또는 특정 클라이언트에 현재 음원 재생 상태를 보고하도록 지시합니다.
3. 음원 재생 상태 보고 요청을 받은 클라이언트는 [`AudioPlayer.ReportPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#ReportPlaybackState) 이벤트 메시지를 사용하여 현재 음원 재생 상태를 CLOVA에 보고합니다.
4. CLOVA는 다른 클라이언트의 음원 재생 상태를 요청했던 클라이언트에 상태 정보를 전달하여 정보를 동기화하도록 지시합니다.

따라서 클라이언트는 [`AudioPlayer.ExpectReportPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#ExpectReportPlaybackState) 지시 메시지를 받으면 [`AudioPlayer.ReportPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#ReportPlaybackState) 이벤트 메시지를 CLOVA에 전송해야 하며, 다음과 같은 정보를 `payload` 필드에 포함시켜야 합니다.

* 오디오 플레이 상태
* 반복 재생 여부
* 현재 재생하고 있는 음원의 재생 시점(음원을 재생 중인 경우)
* [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지를 통해 전달받은 [`AudioStreamInfoObject`](/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject) 객체(음원을 재생 중이거나 일시 정지 중인 경우)
* [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지의 `audioItem.stream.token` 필드 값(음원을 재생 중이거나 일시 정지 중인 경우)
* 재생하고 있는 미디어의 전체 길이(선택)

```json
// 음원을 재생하지 않을 때
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

// 음원을 재생하고 있거나 일시 중지 중일 때
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ReportPlaybackState",
      "messageId": "aa1b20b1-7562-4236-a7a6-65a235571bac"
    },
    "payload": {
      "playerActivity": "PLAYING",
      "repeatMode": "NONE",
      "offsetInMilliseconds": 13000,
      "stream": {
        "beginAtInMilliseconds": 0,
        "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 30000
        },
        "token": "TR-NM-4435786",
        "urlPlayable": false,
        "url": "clova:TR-NM-4435786"
      },
      "token": "TR-NM-4435786"
    }
  }
}
```