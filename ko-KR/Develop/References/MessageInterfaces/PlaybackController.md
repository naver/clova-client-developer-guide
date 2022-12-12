<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# PlaybackController

<!-- Start of the shared content: CICAPIforAudioPlayback -->
PlaybackController 인터페이스는 클라이언트의 오디오 재생 및 스피커 출력을 제어할 때 사용되는 네임스페이스입니다. PlaybackController가 제공하는 이벤트 메시지와 지시 메시지는 다음과 같습니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`CustomCommandIssued`](#CustomCommandIssued)  | Event     | 사용자가 클라이언트 기기의 단축 버튼 중 하나를 눌렀을 때 이를 CLOVA에 보고합니다.  |
| [`ExpectNextCommand`](#ExpectNextCommand)      | Directive | 클라이언트가 다음 버튼(Next)을 누른 것처럼 동작하도록 지시합니다.  |
| [`ExpectPauseCommand`](#ExpectPauseCommand)    | Directive | 클라이언트가 일시 정지 버튼(Pause)을 누른 것처럼 동작하도록 지시합니다.  |
| [`ExpectPlayCommand`](#ExpectPlayCommand)      | Directive | 클라이언트가 재생 버튼(Play)을 누른 것처럼 동작하도록 지시합니다.  |
| [`ExpectPreviousCommand`](#ExpectPreviousCommand)  | Directive | 클라이언트가 이전 버튼(Previous)을 누른 것처럼 동작하도록 지시합니다.  |
| [`ExpectResumeCommand`](#ExpectResumeCommand)  | Directive | 클라이언트가 재개 버튼(Resume)을 누른 것처럼 동작하도록 지시합니다.  |
| [`ExpectStopCommand`](#ExpectStopCommand)      | Directive | 클라이언트가 중지 버튼(Stop)을 누른 것처럼 동작하도록 지시합니다.  |
| [`Mute`](#Mute)                                | Directive | 클라이언트가 오디오 플레이어의 볼륨을 음소거하도록 지시합니다.            |
| [`Next`](#Next)                                | Directive | 클라이언트가 재생 대기열에 있는 다음 오디오 스트림을 재생하도록 지시합니다.   |
| [`NextCommandIssued`](#NextCommandIssued)      | Event     | 클라이언트에서 다음 버튼(Next)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다. |
| [`Pause`](#Pause)                              | Directive | 클라이언트가 재생 중인 오디오 스트림을 일시 정지하도록 지시합니다.        |
| [`PauseCommandIssued`](#PauseCommandIssued)    | Event     | 클라이언트에서 일시 정지 버튼(Pause)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다.  |
| [`PlayCommandIssued`](#PlayCommandIssued)      | Event     | 클라이언트에서 재생 버튼(Play)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다.  |
| [`Previous`](#Previous)                        | Directive | 클라이언트가 재생 대기열에 있는 이전 오디오 스트림을 재생하도록 지시합니다. |
| [`PreviousCommandIssued`](#PreviousCommandIssued) | Event | 클라이언트에서 이전 버튼(Previous)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다. |
| [`Replay`](#Replay)                            | Directive | 클라이언트가 오디오 스트림을 처음부터 다시 재생하도록 지시합니다.         |
| [`Resume`](#Resume)                            | Directive | 클라이언트가 오디오 스트림 재생을 재개하도록 지시합니다.                |
| [`ResumeCommandIssued`](#ResumeCommandIssued)  | Event | 클라이언트에서 재개 버튼(Resume)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다.  |
| [`SetRepeatMode`](#SetRepeatMode)              | Directive | 클라이언트가 지정된 반복 모드로 재생 상태를 변경하도록  지시합니다.  |
| [`SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued) | Event | 클라이언트에서 반복 재생 버튼(Repeat)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다.  |
| [`Stop`](#Stop)                                | Directive | 클라이언트가 오디오 스트림 재생을 중지하도록 지시합니다.                |
| [`StopCommandIssued`](#StopCommandIssued)      | Event     | 클라이언트에서 중지 버튼(Stop)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다.  |
| [`TurnOffRepeatMode`](#TurnOffRepeatMode)      | Directive | **(Deprecated)** 클라이언트가 한 곡 반복 재생 모드를 해제하도록 지시합니다.                  |
| [`TurnOnRepeatMode`](#TurnOnRepeatMode)        | Directive | **(Deprecated)** 클라이언트가 한 곡 반복 재생 모드를 설정하도록 지시합니다.                  |
| [`Unmute`](#Unmute)                            | Directive | 클라이언트가 오디오 플레이어 볼륨의 음소거를 해제하도록 지시합니다.              |
| [`VolumeDown`](#VolumeDown)                    | Directive | **(Deprecated)** 클라이언트가 오디오 플레이어 볼륨을 낮추도록 지시합니다.                      |
| [`VolumeUp`](#VolumeUp)                        | Directive | **(Deprecated)** 클라이언트가 오디오 플레이어 볼륨을 높이도록 지시합니다.                      |

<!-- End of the shared content -->

## CustomCommandIssued event {#CustomCommandIssued}

사용자가 클라이언트 기기의 단축 버튼 중 하나를 눌렀을 때 이를 CLOVA에 보고합니다. 이 이벤트 메시지를 받은 CLOVA는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `button`      | string  | 클라이언트 기기에 있는 단축 버튼을 구분하기 위한 값입니다. (예: <code>"CUSTOM_BUTTON_2"</code>) | 필수 |

### Remarks

* 클라이언트 기기의 버튼은 물리적인 하드웨어 방식의 버튼일 수도 있고 음악 플레이어 위젯 버튼과 같은 소프트웨어 방식의 버튼일 수도 있습니다.

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "CustomCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "button": "CUSTOM_BUTTON_2"
    }
  }
}
```

### See also

* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## ExpectNextCommand directive {#ExpectNextCommand}

클라이언트가 다음 버튼(Next)을 누른 것처럼 동작하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 사용자가 클라이언트 기기에서 다음 버튼(Next)을 누른 효과가 발생한 것처럼 동작을 수행하고 [`PlaybackController.NextCommandIssued`](#NextCommandIssued) 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 제어 대상 정보를 가지는 객체.<div class="note"><p><strong>Note!</strong></p><p>이 필드가 있다면 클라이언트는 <a href="#NextCommandIssued"><code>PlaybackController.NextCommandIssued</code></a> 이벤트 메시지를 전송할 때 <code>source</code> 필드를 반드시 함께 보내야 합니다.</p></div> | 조건부  |
| target.namespace  | string  | CIC API 네임스페이스. 제어 대상이 무엇인지 파악할 수 있는 정보입니다. [`PlaybackController.NextCommandIssued`](#NextCommandIssued) 이벤트 메시지를 보낼 때 반드시 `source.namespace` 값을 이 필드의 값으로 입력해야 합니다. 다음과 같은 값을 가질 수 있습니다.<ul><li><code>"AudioPlayer"</code>: 오디오 플레이어</li><li><code>"MediaPlayer"</code>: 미디어 플레이어</li></ul>  | 항상  |

### Message example

```json
예제 1: 대상이 지정되지 않은 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectNextCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

예제 2: 대상이 "AudioPlayer"로 지정된 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectNextCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)

## ExpectPauseCommand directive {#ExpectPauseCommand}

클라이언트가 일시 정지 버튼(Pause)을 누른 것처럼 동작하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 사용자가 클라이언트 기기에서 일시 정지 버튼(Pause)을 누른 효과가 발생한 것처럼 동작을 수행하고 [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued) 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 제어 대상 정보를 가지는 객체.<div class="note"><p><strong>Note!</strong></p><p>이 필드가 있다면 클라이언트는 <a href="#PauseCommandIssued"><code>PlaybackController.PauseCommandIssued</code></a> 이벤트 메시지를 전송할 때 <code>source</code> 필드를 반드시 함께 보내야 합니다.</p></div> | 조건부  |
| target.namespace  | string  | CIC API 네임스페이스. 제어 대상이 무엇인지 파악할 수 있는 정보입니다. [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued) 이벤트 메시지를 보낼 때 반드시 `source.namespace` 값을 이 필드의 값으로 입력해야 합니다. 다음과 같은 값을 가질 수 있습니다.<ul><li><code>"AudioPlayer"</code>: 오디오 플레이어</li><li><code>"MediaPlayer"</code>: 미디어 플레이어</li></ul>  | 항상  |

### Message example

```json
예제 1: 대상이 지정되지 않은 예제
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

예제 2: 대상이 "AudioPlayer"로 지정된 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPauseCommand",
      "dialogRequestId": "7182403e-b5eb-4b71-b2af-179a7515edc4",
      "messageId": "bbb3a40d-ad7f-4609-9021-97f3f0aa2a9e"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

## ExpectPlayCommand directive {#ExpectPlayCommand}

클라이언트가 재생 버튼(Play)을 누른 것처럼 동작하도록 지시합니다. 이 지시 메시지는 현재 재생 중인 미디어 스트림을 다른 기기에서 재생하려고 할 때에도 수신될 수 있습니다. 클라이언트는 이 지시 메시지를 받은 후 사용자가 클라이언트 기기에서 재생 버튼(Play)을 누른 효과가 발생한 것처럼 동작을 수행하고 [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `handover`            | object  | 원격으로 미디어 재생을 넘겨 받을 때 필요한 정보가 담긴 객체. 미디어 재생을 넘겨 받아야 하면 `handover` 객체가 메시지에 포함됩니다. `handover` 객체가 포함되어 있으면 클라이언트는 [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) 이벤트 메시지 `payload`의 `handover` 객체를 이 객체의 내용으로 채워야 합니다.     | 조건부 |
| `handover.customData` | string  | 미디어 재생에 필요한 정보.               | 항상 |
| `handover.deviceId`   | string  | 미디어 재생을 넘겨주는 클라이언트 기기의 ID  | 항상 |
| `token`               | string  | 재생해야 하는 미디어 콘텐츠의 token. 미디어 재생을 넘겨 받아야 하면 `token` 필드가 메시지에 포함됩니다. `token` 필드가 포함되어 있으면 클라이언트는 [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) 이벤트 메시지를 전송할 때 `token`에 이 필드의 값을 입력해야 합니다.  | 조건부  |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPlayCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)

## ExpectPreviousCommand directive {#ExpectPreviousCommand}

클라이언트가 이전 버튼(Previous)을 누른 것처럼 동작하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 사용자가 클라이언트 기기에서 이전 버튼(Previous)을 누른 효과가 발생한 것처럼 동작을 수행하고 [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued) 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 제어 대상 정보를 가지는 객체.<div class="note"><p><strong>Note!</strong></p><p>이 필드가 있다면 클라이언트는 <a href="#PreviousCommandIssued"><code>PlaybackController.PreviousCommandIssued</code></a> 이벤트 메시지를 전송할 때 <code>source</code> 필드를 반드시 함께 보내야 합니다.</p></div> | 조건부  |
| target.namespace  | string  | CIC API 네임스페이스. 제어 대상이 무엇인지 파악할 수 있는 정보입니다. [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued) 이벤트 메시지를 보낼 때 반드시 `source.namespace` 값을 이 필드의 값으로 입력해야 합니다. 다음과 같은 값을 가질 수 있습니다.<ul><li><code>"AudioPlayer"</code>: 오디오 플레이어</li><li><code>"MediaPlayer"</code>: 미디어 플레이어</li></ul>  | 항상  |

### Message example

```json
예제 1: 대상이 지정되지 않은 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPreviousCommand",
      "dialogRequestId": "036fbb1e-4739-4453-9439-ccff68eccc63",
      "messageId": "e32f6330-e696-4c21-9e52-529e9b4cbc14"
    },
    "payload": {}
  }
}

예제 2: 대상이 "AudioPlayer"로 지정된 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPreviousCommand",
      "dialogRequestId": "036fbb1e-4739-4453-9439-ccff68eccc63",
      "messageId": "e32f6330-e696-4c21-9e52-529e9b4cbc14"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)

## ExpectResumeCommand directive {#ExpectResumeCommand}

클라이언트가 재개 버튼(Resume)을 누른 것처럼 동작하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 사용자가 클라이언트 기기에서 재개 버튼(Resume)을 누른 효과가 발생한 것처럼 동작을 수행하고 [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 제어 대상 정보를 가지는 객체.<div class="note"><p><strong>Note!</strong></p><p>이 필드가 있다면 클라이언트는 <a href="#ResumeCommandIssued"><code>PlaybackController.ResumeCommandIssued</code></a> 이벤트 메시지를 전송할 때 <code>source</code> 필드를 반드시 함께 보내야 합니다.</p></div> | 조건부  |
| target.namespace  | string  | CIC API 네임스페이스. 제어 대상이 무엇인지 파악할 수 있는 정보입니다. [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) 이벤트 메시지를 보낼 때 반드시 `source.namespace` 값을 이 필드의 값으로 입력해야 합니다. 다음과 같은 값을 가질 수 있습니다.<ul><li><code>"AudioPlayer"</code>: 오디오 플레이어</li><li><code>"MediaPlayer"</code>: 미디어 플레이어</li></ul>  | 항상  |

### Message example

```json
예제 1: 대상이 지정되지 않은 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectResumeCommand",
      "dialogRequestId": "70d75f9a-2202-4a4e-a0cb-9f804d235890",
      "messageId": "3865c844-27dd-4fda-a7d2-03ef96a4cc83"
    },
    "payload": {}
  }
}

예제 2: 대상이 "AudioPlayer"로 지정된 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectResumeCommand",
      "dialogRequestId": "d7b5f918-2bb5-4b94-91c5-e1ddec0aa8c4",
      "messageId": "c75a01b4-8402-444a-9386-aa1819d12d29"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

## ExpectStopCommand directive {#ExpectStopCommand}

클라이언트가 중지 버튼(Stop)을 누른 것처럼 동작하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 사용자가 클라이언트 기기에서 정지 버튼(Stop)을 누른 효과가 발생한 것처럼 동작을 수행하고 [`PlaybackController.StopCommandIssued`](#StopCommandIssued) 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 제어 대상 정보를 가지는 객체.<div class="note"><p><strong>Note!</strong></p><p>이 필드가 있다면 클라이언트는 <a href="#StopCommandIssued"><code>PlaybackController.StopCommandIssued</code></a> 이벤트 메시지를 전송할 때 <code>source</code> 필드를 반드시 함께 보내야 합니다.</p></div> | 조건부  |
| target.namespace  | string  | CIC API 네임스페이스. 제어 대상이 무엇인지 파악할 수 있는 정보입니다. [`PlaybackController.StopCommandIssued`](#StopCommandIssued) 이벤트 메시지를 보낼 때 반드시 `source.namespace` 값을 이 필드의 값으로 입력해야 합니다. 다음과 같은 값을 가질 수 있습니다.<ul><li><code>"AudioPlayer"</code>: 오디오 플레이어</li><li><code>"MediaPlayer"</code>: 미디어 플레이어</li></ul>  | 항상  |

### Message example

```json
예제 1: 대상이 지정되지 않은 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectStopCommand",
      "dialogRequestId": "e3295587-23a5-49d7-bc72-05adf02e4a08",
      "messageId": "8b15ad68-ee0c-44de-959a-0f0c3526b361"
    },
    "payload": {}
  }
}

예제 2: 대상이 "AudioPlayer"로 지정된 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectStopCommand",
      "dialogRequestId": "bbfdad3a-333c-4a97-beb0-33ac9d46ac9d",
      "messageId": "1a565441-e2bc-40a5-bb3e-7846767f37be"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

## Mute directive {#Mute}

클라이언트가 오디오 플레이어의 볼륨을 음소거하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 오디오 스트림 재생과 관련된 스피커 볼륨을 무음이 되도록 변경해야 합니다.

### Payload fields

없음

### Remarks

CLOVA는 스피커 출력과 관계된 제어이면 [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지를 통해 안내 문구를 내려보내지 않습니다. 이는 사용자의 음악 감상과 같은 UX를 고려한 사항이며, 이때는 음성 안내 대신 클라이언트 기기의 조명이나 짧은 효과음 통해 볼륨이 조절되었음을 알리도록 구현해야 합니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Mute",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)

## Next directive {#Next}

클라이언트가 재생 대기열에 있는 다음 오디오 스트림을 재생하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 다음 오디오 스트림을 재생해야 합니다.

### Payload fields

없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Next",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`PlaybackController.Previous`](#Previous)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)

## NextCommandIssued event {#NextCommandIssued}

클라이언트에서 다음 버튼(Next)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다. 사용자가 다음 버튼(Next)을 누르거나 CLOVA로부터 [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand) 지시 메시지를 받았다면 클라이언트는 이 이벤트 메시지를 CLOVA에 전송해야 합니다. 이 이벤트 메시지를 받은 CLOVA는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 클라이언트 기기 ID. 미디어 재생을 원격으로 제어하는 상황이 아니라면 이 필드를 생략합니다. | 선택 |
| `source`      | object  | 이벤트 원천 정보를 가지는 객체. 이 이벤트 메시지를 전송하게 된 상황이나 배경을 알 수 있는 정보를 포함합니다. <div class="tip"><p><strong>Tip!</strong></p><p>클라이언트가 사용자에게 음성 출력(TTS)과 음원 재생을 동시에 제공하고 있는 경우 보통 음성 출력이 제어 대상이 됩니다. 이 필드를 사용하면 이 이벤트 메시지가 어떤 대상을 제어하려고 하는지 지정해 줄 수 있습니다. CLOVA가 제어 대상을 알아서 지정하도록 위임하려면 이 필드를 생략하면 됩니다.</p></div>  | 선택  |
| `source.namespace` | string | CIC API 네임스페이스. 이 이벤트 메시지가 어떤 상황이나 맥락에서 전송된 것인지 파악할 수 있도록 관련 CIC API 네임스페이스를 값을 입력합니다.<ul><li><code>"AudioPlayer"</code>: 오디오 플레이어</li><li><code>"MediaPlayer"</code>: 미디어 플레이어</li></ul>  | 필수  |

### Remarks

* 클라이언트 기기의 버튼은 물리적인 하드웨어 방식의 버튼일 수도 있고 음악 플레이어 위젯 버튼과 같은 소프트웨어 방식의 버튼일 수도 있습니다.

### Message example

```json
예제 1: 원천 정보를 지정하지 않은 예제
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "NextCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

예제 2: 원천 정보를 "AudioPlayer"로 지정한 예제
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "NextCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

<!-- End of the shared content -->

<!-- Start of the shared content: PlaybackController.Pause -->

## Pause directive {#Pause}

클라이언트가 재생 중인 오디오 스트림을 일시 정지하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 오디오 스트림 재생을 일시 정지해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 제어 대상 정보를 가지는 객체. 이 지시 메시지를 통해 제어해야 하는 대상을 알 수 있습니다. | 조건부  |
| target.namespace  | string  | CIC API 네임스페이스. 제어 대상이 무엇인지 파악할 수 있는 정보입니다. 다음과 같은 값을 가질 수 있습니다.<ul><li><code>"AudioPlayer"</code>: 오디오 플레이어</li><li><code>"MediaPlayer"</code>: 미디어 플레이어</li></ul>  | 항상  |

### Message example

```json
예제 1: 대상이 지정되지 않은 예제
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

예제 2: 대상이 "AudioPlayer"로 지정된 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Pause",
      "dialogRequestId": "a85e78b4-44f7-4bea-8a40-66c181b9720f",
      "messageId": "a76c8dfd-c1b6-44f6-a58d-fc8f33c242c1"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`AudioPlayer.PlayPaused`](/Develop/References/MessageInterfaces/AudioPlayer.md#PlayPaused)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

## PauseCommandIssued event {#PauseCommandIssued}

클라이언트에서 일시 정지 버튼(Pause)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다. 사용자가 일시 정지 버튼(Pause)을 누르거나 CLOVA로부터 [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand) 지시 메시지를 받았다면 클라이언트는 이 이벤트 메시지를 CLOVA에 전송해야 합니다. 이 이벤트 메시지를 받은 CLOVA는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 클라이언트 기기 ID. 미디어 재생을 원격으로 제어하는 상황이 아니라면 이 필드를 생략합니다. | 선택 |
| `source`      | object  | 이벤트 원천 정보를 가지는 객체. 이 이벤트 메시지를 전송하게 된 상황이나 배경을 알 수 있는 정보를 포함합니다. <div class="tip"><p><strong>Tip!</strong></p><p>클라이언트가 사용자에게 음성 출력(TTS)과 음원 재생을 동시에 제공하고 있는 경우 보통 음성 출력이 제어 대상이 됩니다. 이 필드를 사용하면 이 이벤트 메시지가 어떤 대상을 제어하려고 하는지 지정해 줄 수 있습니다. CLOVA가 제어 대상을 알아서 지정하도록 위임하려면 이 필드를 생략하면 됩니다.</p></div>  | 선택  |
| `source.namespace` | string | CIC API 네임스페이스. 이 이벤트 메시지가 어떤 상황이나 맥락에서 전송된 것인지 파악할 수 있도록 관련 CIC API 네임스페이스를 값을 입력합니다. 현재 `"AudioPlayer"` 값만 입력할 수 있습니다.  | 필수  |

### Remarks

* 클라이언트 기기의 버튼은 물리적인 하드웨어 방식의 버튼일 수도 있고 음악 플레이어 위젯 버튼과 같은 소프트웨어 방식의 버튼일 수도 있습니다.

### Message example

```json
// 예제 1: 원천 정보를 지정하지 않은 예제
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

// 예제 2: 원천 정보를 "AudioPlayer"로 지정한 예제
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
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPauseCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

## PlayCommandIssued event {#PlayCommandIssued}

클라이언트에서 재생 버튼(Play)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다. 사용자가 클라이언트 기기에서 특정 곡을 재생하도록 UI를 조작했거나 CLOVA로부터 [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) 지시 메시지를 받았다면 클라이언트는 이 이벤트 메시지를 CLOVA에 전송해야 합니다. 이 이벤트 메시지를 받은 CLOVA는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

만약, CLOVA로부터 받은[`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) 지시 메시지의 `payload`에 `handover` 필드가 포함되어 있다면 이를 그대로 사용하여 미디어를 이어 재생해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`            | string  | 클라이언트 기기 ID. 원격으로 미디어 재생을 다른 기기로 넘기는 상황이 아니라면 `deviceId` 필드를 생략합니다. | 선택 |
| `handover`            | object  | 원격으로 미디어 재생을 넘겨 받을 때 필요한 정보를 담는 객체. 미디어 재생을 넘겨 받아야 하면 `handover` 객체의 내용을 [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) 지시 메시지 `payload`의 `handover` 객체로 채워야 합니다.     | 선택 |
| `handover.customData` | string  | 미디어 재생에 필요한 정보.               | 필수 |
| `handover.deviceId`   | string  | 미디어 재생을 넘겨주는 클라이언트 기기의 ID  | 필수 |
| `token`               | string  | 재생해야 하는 미디어 콘텐츠의 token. 사용자가 목록에서 곡을 골라 재생 버튼(play)을 눌렀을 때 [`TemplateRuntime.RenderPlayerInfo`](/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlayerInfo) 지시 메시지의 `playableItems[].token` 필드 값이 적용되어야 합니다. [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) 지시 메시지를 받았다면 해당 메시지의 `token` 필드 값을 입력해야 할 수도 있습니다.  | 선택  |

### Remarks

* 사용자가 클라이언트 기기의 재개 버튼(resume)을 누를 때는 [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PlayCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```

### See also

* [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play)
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPlayCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## Previous directive {#Previous}

클라이언트가 재생 대기열에 있는 이전 오디오 스트림을 재생하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 이전 오디오 스트림을 재생해야 합니다.

### Payload fields

없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Previous",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`PlaybackController.Next`](#Next)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)

## PreviousCommandIssued event {#PreviousCommandIssued}

클라이언트에서 이전 버튼(Previous)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다. 사용자가 이전 버튼(Previous)을 누르거나 CLOVA로부터 [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand) 지시 메시지를 받았다면 클라이언트는 이 이벤트 메시지를 CLOVA에 전송해야 합니다. 이 이벤트 메시지를 받은 CLOVA는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 클라이언트 기기 ID. 미디어 재생을 원격으로 제어하는 상황이 아니라면 이 필드를 생략합니다. | 선택 |
| `source`      | object  | 이벤트 원천 정보를 가지는 객체. 이 이벤트 메시지를 전송하게 된 상황이나 배경을 알 수 있는 정보를 포함합니다. <div class="tip"><p><strong>Tip!</strong></p><p>클라이언트가 사용자에게 음성 출력(TTS)과 음원 재생을 동시에 제공하고 있는 경우 보통 음성 출력이 제어 대상이 됩니다. 이 필드를 사용하면 이 이벤트 메시지가 어떤 대상을 제어하려고 하는지 지정해 줄 수 있습니다. CLOVA가 제어 대상을 알아서 지정하도록 위임하려면 이 필드를 생략하면 됩니다.</p></div>  | 선택  |
| `source.namespace` | string | CIC API 네임스페이스. 이 이벤트 메시지가 어떤 상황이나 맥락에서 전송된 것인지 파악할 수 있도록 관련 CIC API 네임스페이스를 값을 입력합니다. 현재 `"AudioPlayer"` 값만 입력할 수 있습니다.  | 필수  |

### Remarks

* 클라이언트 기기의 버튼은 물리적인 하드웨어 방식의 버튼일 수도 있고 음악 플레이어 위젯 버튼과 같은 소프트웨어 방식의 버튼일 수도 있습니다.

### Message example

```json
// 예제 1: 원천 정보를 지정하지 않은 예제
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PreviousCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

// 예제 2: 원천 정보를 "AudioPlayer"로 지정한 예제
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PreviousCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## Replay directive {#Replay}

클라이언트가 오디오 스트림을 처음부터 다시 재생하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 재생 위치를 오디오 스트림의 처음으로 이동시켜야 하며, 이동시킨 후 바로 오디오 스트림 재생을 시작해야 합니다. 만약 오디오 스트림 재생이 일시 정지(paused)되어 있었다면 오디오 스트림 재생을 재개(resume)해야 합니다.

### Payload fields
없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Replay",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`PlaybackController.Pause`](#Pause)
* [`PlaybackController.Resume`](#Resume)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- Start of the shared content: PlaybackController.Resume -->

## Resume directive {#Resume}

클라이언트가 오디오 스트림 재생을 재개하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 오디오 스트림 재생을 재개해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 제어 대상 정보를 가지는 객체. 이 지시 메시지를 통해 제어해야 하는 대상을 알 수 있습니다. | 조건부  |
| target.namespace  | string  | CIC API 네임스페이스. 제어 대상이 무엇인지 파악할 수 있는 정보입니다. 다음과 같은 값을 가질 수 있습니다.<ul><li><code>"AudioPlayer"</code>: 오디오 플레이어</li><li><code>"MediaPlayer"</code>: 미디어 플레이어</li></ul>  | 항상  |

### Message example

```json
// 예제 1: 대상이 지정되지 않은 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Resume",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

// 예제 2: 대상이 "AudioPlayer"로 지정된 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Resume",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`AudioPlayer.PlayResumed`](/Develop/References/MessageInterfaces/AudioPlayer.md#PlayResumed)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

## ResumeCommandIssued event {#ResumeCommandIssued}

클라이언트에서 재개 버튼(Resume)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다. 사용자가 재개 버튼(Resume)을 누르거나 CLOVA로부터 [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand) 지시 메시지를 받았다면 클라이언트는 이 이벤트 메시지를 CLOVA에 전송해야 합니다. 이 이벤트 메시지를 받은 CLOVA는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 클라이언트 기기 ID. 미디어 재생을 원격으로 제어하는 상황이 아니라면 이 필드를 생략합니다. | 선택 |
| `source`      | object  | 이벤트 원천 정보를 가지는 객체. 이 이벤트 메시지를 전송하게 된 상황이나 배경을 알 수 있는 정보를 포함합니다. <div class="tip"><p><strong>Tip!</strong></p><p>클라이언트가 사용자에게 음성 출력(TTS)과 음원 재생을 동시에 제공하고 있는 경우 보통 음성 출력이 제어 대상이 됩니다. 이 필드를 사용하면 이 이벤트 메시지가 어떤 대상을 제어하려고 하는지 지정해 줄 수 있습니다. CLOVA가 제어 대상을 알아서 지정하도록 위임하려면 이 필드를 생략하면 됩니다.</p></div>  | 선택  |
| `source.namespace` | string | CIC API 네임스페이스. 이 이벤트 메시지가 어떤 상황이나 맥락에서 전송된 것인지 파악할 수 있도록 관련 CIC API 네임스페이스를 값을 입력합니다. 현재 `"AudioPlayer"` 값만 입력할 수 있습니다.  | 필수  |

### Remarks

* 클라이언트 기기의 버튼은 물리적인 하드웨어 방식의 버튼일 수도 있고 음악 플레이어 위젯 버튼과 같은 소프트웨어 방식의 버튼일 수도 있습니다.
* **사용자가 재생 버튼(play)을 누를 때**도 이 이벤트 메시지를 사용해야 합니다.

### Message example

```json
// 예제 1: 원천 정보를 지정하지 않은 예제
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ResumeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

// 예제 2: 원천 정보를 "AudioPlayer"로 지정한 예제
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ResumeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectResumeCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

## SetRepeatMode directive {#SetRepeatMode}

클라이언트가 지정된 반복 모드로 재생 상태를 변경하도록  지시합니다. 클라이언트는 이 지시 메시지를 받은 후 반복 모드에 대한 상태([Context object](/Develop/References/Context_Objects.md))를 지정된 값으로 변경해야 하며, 이벤트 메시지를 전송할 때 [`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)의 `repeatMode` 필드를 지정한 값으로 설정해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `repeatMode`  | string  | 선택된 반복 모드<ul><li><code>NONE</code>: 반복 재생 안함</li><code>REPEAT_ONE</code>: 한 곡 반복 재생</li></ul>  | 항상 |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "SetRepeatMode",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "repeatMode": "NONE"
    }
  }
}
```

### See also

* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)

## SetRepeatModeCommandIssued event {#SetRepeatModeCommandIssued}

클라이언트에서 반복 재생 버튼(Repeat)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다. CLOVA 앱 또는 연동 앱(companion app)에서 원격으로 CLOVA 기기의 재생 모드를 변경할 때 사용합니다. 이 이벤트 메시지를 받은 CLOVA는 [`PlaybackController.SetRepeatMode`](#SetRepeatMode) 지시 메시지를 대상 클라이언트에 전송합니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>연동 앱에서 원격으로 CLOVA 기기의 재생 모드를 변경하는 경우가 아니면 클라이언트가 이 이벤트 메시지를 생성 또는 처리할 필요가 없습니다. 클라이언트 기기의 재생 모드 변화는 <a href="/Develop/References/Context_Objects.md#PlaybackState"><code>AudioPlayer.PlaybackState</code></a> 맥락 정보의 <code>repeatMode</code>필드를 이용하여 CLOVA에 보고하면 됩니다.</p>
</div>

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 클라이언트 기기 ID. 미디어 재생을 원격으로 제어하는 상황이 아니라면 이 필드를 생략합니다.                               | 선택 |
| `repeatMode`  | string  | 선택된 반복 모드<ul><li><code>NONE</code>: 반복 재생 안함</li><code>REPEAT_ONE</code>: 한 곡 반복 재생</li></ul>  | 필수 |

### Remarks

* 연동 앱에서 CLOVA 기기의 재생 모드를 변경하는 기능을 만들지 않는다면 이 이벤트 메시지를 생성하거나 이 메시지와 관련하여 처리할 작업이 없습니다.
* 클라이언트 기기의 재생 모드 변화는 [`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState) 맥락 정보의 `repeatMode` 필드를 이용하여 CLOVA에 보고하면 됩니다.

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "SetRepeatModeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "repeatMode": "NONE"
    }
  }
}
```

### See also

* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.SetRepeatMode`](#SetRepeatMode)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

<!-- Start of the shared content: PlaybackController.Stop -->

## Stop directive {#Stop}

클라이언트가 오디오 스트림 재생을 중지하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 오디오 스트림 재생을 중지해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 제어 대상 정보를 가지는 객체. 이 지시 메시지를 통해 제어해야 하는 대상을 알 수 있습니다. | 조건부  |
| target.namespace  | string  | CIC API 네임스페이스. 제어 대상이 무엇인지 파악할 수 있는 정보입니다. 다음과 같은 값을 가질 수 있습니다.<ul><li><code>"AudioPlayer"</code>: 오디오 플레이어</li><li><code>"MediaPlayer"</code>: 미디어 플레이어</li></ul>  | 항상  |

### Message example

```json
// 예제 1: 대상이 지정되지 않은 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Stop",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

// 예제 2: 대상이 "AudioPlayer"로 지정된 예제
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Stop",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`AudioPlayer.PlayStopped`](/Develop/References/MessageInterfaces/AudioPlayer.md#PlayStopped)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

## StopCommandIssued event {#StopCommandIssued}

클라이언트에서 중지 버튼(Stop)이 눌렸거나 그에 준하는 사용자의 UI 조작이 있었음을 CLOVA에 보고합니다. 사용자가 중지 버튼(Stop)을 누르거나 CLOVA로부터 [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand) 지시 메시지를 받았다면 클라이언트는 이 이벤트 메시지를 CLOVA에 전송해야 합니다. 이 이벤트 메시지를 받은 CLOVA는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 클라이언트 기기 ID. 미디어 재생을 원격으로 제어하는 상황이 아니라면 이 필드를 생략합니다. | 선택 |
| `source`      | object  | 이벤트 원천 정보를 가지는 객체. 이 이벤트 메시지를 전송하게 된 상황이나 배경을 알 수 있는 정보를 포함합니다. <div class="tip"><p><strong>Tip!</strong></p><p>클라이언트가 사용자에게 음성 출력(TTS)과 음원 재생을 동시에 제공하고 있는 경우 보통 음성 출력이 제어 대상이 됩니다. 이 필드를 사용하면 이 이벤트 메시지가 어떤 대상을 제어하려고 하는지 지정해 줄 수 있습니다. CLOVA가 제어 대상을 알아서 지정하도록 위임하려면 이 필드를 생략하면 됩니다.</p></div>  | 선택  |
| `source.namespace` | string | CIC API 네임스페이스. 이 이벤트 메시지가 어떤 상황이나 맥락에서 전송된 것인지 파악할 수 있도록 관련 CIC API 네임스페이스를 값을 입력합니다. 현재 `"AudioPlayer"` 값만 입력할 수 있습니다.  | 필수  |

### Remarks

* 클라이언트 기기의 버튼은 물리적인 하드웨어 방식의 버튼일 수도 있고 음악 플레이어 위젯 버튼과 같은 소프트웨어 방식의 버튼일 수도 있습니다.

### Message example

```json
// 예제 1: 원천 정보를 지정하지 않은 예제
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "StopCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

// 예제 2: 원천 정보를 "AudioPlayer"로 지정한 예제
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "StopCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also

* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectResumeCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [음원 재생 제어하기](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

## TurnOffRepeatMode directive {#TurnOffRepeatMode}

**(Deprecated)** 클라이언트가 한 곡 반복 재생 모드를 해제하도록 지시합니다.

### Payload fields
없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "TurnOffRepeatMode",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)

## TurnOnRepeatMode directive {#TurnOnRepeatMode}

**(Deprecated)** 클라이언트가 한 곡 반복 재생 모드를 설정하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 현재 재생 중인 오디오 스트림을 계속 반복 재생해야 합니다.

### Payload fields

없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "TurnOnRepeatMode",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)

## Unmute directive {#Unmute}

클라이언트가 오디오 플레이어 볼륨의 음소거를 해제하도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 무음으로 설정했던 스피커 볼륨을 원래 크기로 되돌려야 합니다.

### Payload fields

없음

### Remarks

CLOVA는 스피커 출력과 관계된 제어이면 [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지를 통해 안내 문구를 내려보내지 않습니다. 이는 사용자의 음악 감상과 같은 UX를 고려한 사항이며, 이때는 음성 안내 대신 클라이언트 기기의 조명이나 짧은 효과음 통해 볼륨이 조절되었음을 알리도록 구현해야 합니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Unmute",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)

## VolumeDown directive {#VolumeDown}

**(Deprecated)** 클라이언트가 오디오 플레이어 볼륨을 낮추도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 오디오 스트림 재생과 관련된 스피커 볼륨을 낮춰야 합니다. 볼륨을 낮추는 정도는 각 클라이언트의 UX 기준을 따릅니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p><code>PlaybackController.VolumeDown</code> 지시 메시지는 더 이상 지원하지 않을 예정입니다. 이 지시 메시지 대신 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#Decrease"><code>DiviceControl.Decrease</code></a>지시 메시지를 사용해야 합니다.</p>
</div>

### Payload fields

없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "VolumeDown",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)

## VolumeUp directive {#VolumeUp}

**(Deprecated)** 클라이언트가 오디오 플레이어 볼륨을 높이도록 지시합니다. 클라이언트는 이 지시 메시지를 받은 후 오디오 스트림 재생과 관련된 스피커 볼륨을 높여야 합니다. 볼륨을 올리는 정도는 각 클라이언트의 UX 기준을 따릅니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p><code>PlaybackController.VolumeUp</code> 지시 메시지는 더 이상 지원하지 않을 예정입니다. 이 지시 메시지 대신 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#Increase"><code>DiviceControl.Increase</code></a>지시 메시지를 사용해야 합니다.</p>
</div>

### Payload fields

없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "VolumeUp",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also

* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
