<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# TemplateRuntime

<!-- Start of the shared content: CICAPIforAudioPlayback -->
TemplateRuntime 인터페이스는 클라이언트나 CIC가 미디어 플레이어에 표시할 재생 메타 정보를 요청하거나 전달할 때 사용됩니다. 실제 오디오 스트림 재생에 필요한 정보와 관련된 작업을 수행할 때는 [`AudioPlayer`](/Develop/References/MessageInterfaces/AudioPlayer.md) 인터페이스를 사용하고 재생 목록, 앨범 이미지, 가사와 같은 재생 메타 정보와 관련된 작업을 수행할 때는 `TemplateRuntime` 인터페이스를 사용해야 합니다. 이를 통해 자신 뿐만 아니라 다른 클라이언트 기기의 재생 메타 정보를 조회하고 사용자에게 제공할 수도 있습니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`ExpectRequestPlayerInfo`](#ExpectRequestPlayerInfo)   | Directive | CLOVA가 재생 메타 정보를 요청하도록 클라이언트에 지시합니다. |
| [`LikeCommandIssued`](#LikeCommandIssued)               | Event     | 사용자가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 좋아요 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. |
| [`MoreCommandIssued`](#MoreCommandIssued)               | Event     | 사용자가 클라이언트 기기의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 더보기 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. |
| [`RenderPlayerInfo`](#RenderPlayerInfo)                 | Directive | CLOVA가 미디어 플레이어에 표시할 재생 목록, 앨범 이미지, 가사와 같은 재생 메타 정보를 전달하고 이를 표시하도록 클라이언트에 지시합니다. |
| [`RenderPlaylist`](#RenderPlaylist)                     | Directive | CLOVA가 재생 목록 화면을 표시하도록 클라이언트에 지시합니다. |
| [`RequestPlayerInfo`](#RequestPlayerInfo)               | Event     | 클라이언트가 미디어 플레이어에 표시할 재생 목록, 앨범 이미지, 가사와 같은 재생 메타 정보를 CIC에 요청합니다. |
| [`SelectCommandIssued`](#SelectCommandIssued)           | Event     | 사용자가 재생 목록 화면에서 하나의 아이템을 선택하면 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. |
| [`SubscribeCommandIssued`](#SubscribeCommandIssued)     | Event     | 사용자가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 구독 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다.  |
| [`UnlikeCommandIssued`](#UnlikeCommandIssued)           | Event     | 사용자가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 좋아요 취소 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. |
| [`UnsubscribeCommandIssued`](#UnsubscribeCommandIssued) | Event     | 사용자가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 구독 취소 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. |
| [`UpdateLike`](#UpdateLike)                             | Directive | CLOVA가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대한 사용자의 좋아요(like) 표시 상태를 지정된 값으로 업데이트하도록 클라이언트에 지시합니다.  |
| [`UpdateSubscribe`](#UpdateSubscribe)                   | Directive | CLOVA가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대한 사용자의 구독(subscribe) 표시 상태를 지정된 값으로 업데이트하도록 클라이언트에 지시합니다.  |

<!-- End of the shared content -->

## ExpectRequestPlayerInfo directive {#ExpectRequestPlayerInfo}

CLOVA가 재생 목록, 앨범 이미지, 가사와 같은 재생 메타 정보를 요청하도록 클라이언트에 지시합니다. 클라이언트는 이 지시 메시지를 받으면 [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo) 이벤트 메시지를 CIC로 전송해야 합니다.

### Payload fields

없음

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "ExpectRequestPlayerInfo",
      "dialogRequestId": "9d7dc3ca-17ff-4df9-9800-91736ba2a3b6",
      "messageId": "46ccdf6c-609a-43a5-91c4-7a43b961f0c0"
    },
    "payload": {}
  }
}
```

### See also

* [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)

## LikeCommandIssued event {#LikeCommandIssued}

사용자가 클라이언트 기기의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 좋아요 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. 좋아요 버튼은 [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지 `playableItems[].controls[].name` 필드의 값이 `"LIKE_DISLIKE"`인 것을 구현한 버튼으로 현재 콘텐츠에 사용자가 아직 좋아요를 표시하지 않은 상태에서 좋아요를 표시하는 용도로 사용되는 버튼입니다. 이 이벤트 메시지를 받은 CIC는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | 미디어 콘텐츠의 token. [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지의 `playableItems[].token` 필드로 제공된 token 값이 입력되어야 합니다. | 필수 |

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
      "namespace": "TemplateRuntime",
      "name": "LikeCommandIssued",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```

### See also

* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)
* [`TemplateRuntime.UpdateLike`](#UpdateLike)

## MoreCommandIssued event {#MoreCommandIssued}

사용자가 클라이언트 기기의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 더보기 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. 더보기 버튼은 [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지 `playableItems[].controls[].name` 필드의 값이 `"MORE"`인 것을 구현한 버튼으로 현재 콘텐츠와 연관된 콘텐츠 목록을 추가로 보여주기 위한 용도로 사용되는 버튼입니다. 이 이벤트 메시지를 받은 CIC는 상황에 맞는 지시 메시지를 클라이언트에 보냅니다. 주로 [`TemplateRuntime.RenderPlaylist`](/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlaylist)나 [`Clova.RenderTemplate`](/Develop/References/MessageInterfaces/Clova.md#RenderTemplate) 지시 메시지가 클라이언트에 전송됩니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | 미디어 콘텐츠의 token. [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지의 `playableItems[].token` 필드로 제공된 token 값이 입력되어야 합니다. | 필수 |

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
      "namespace": "TemplateRuntime",
      "name": "MoreCommandIssued",
      "messageId": "641a2806-dc7a-4fcd-89af-d5d0f3f6eb8d"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```

### See also

* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)

<!-- Start of the shared content: TemplateRuntime.RenderPlayerInfo -->

## RenderPlayerInfo directive {#RenderPlayerInfo}

CLOVA가 미디어 플레이어에 표시할 재생 목록, 앨범 이미지, 가사와 같은 재생 메타 정보를 전달하고 이를 표시하도록 클라이언트에 지시합니다. 사용자가 음악 재생을 요청했을 때 클라이언트는 [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지를 받아 미디어를 재생하게 됩니다. 디스플레이 장치가 있는 클라이언트라면 필요에 따라 미디어 플레이어에 재생 관련 정보를 표현해야 할 수 있습니다. 이때, [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo) 이벤트 메시지를 통해 재생 메타 정보를 CIC에 요청할 수 있으며, `TemplateRuntime.RenderPlayerInfo` 지시 메시지를 수신할 수 있습니다. `TemplateRuntime.RenderPlayerInfo` 지시 메시지는 현재 재생해야 하는 미디어 콘텐츠와 추후 재생해야 하는 미디어 콘텐츠의 재생 메타 정보를 담고 있습니다. 클라이언트는 `TemplateRuntime.RenderPlayerInfo` 지시 메시지의 재생 메타 정보를 사용자에게 제공하므로써 현재 재생 미디어의 메타 정보 및 재생 목록을 표시할 수 있습니다. 뿐만 아니라 사용자가 목록에 있는 특정 미디어를 재생하도록 요청하거나 좋아요([`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)), 좋아요 취소([`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued))와 같은 동작을 수행할 때 이를 처리할 수 있는 기반 데이터(`token`)를 제공합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `controls[]`                | object array | 클라이언트가 미디어 플레이어에서 반드시 표시해야 버튼의 정보를 담고 있는 객체 배열입니다.             | 항상 |
| `controls[].enabled`        | boolean      | `controls[].name`에 명시된 버튼이 미디어 플레이어에서 활성화되어야 하는지 나타냅니다.<ul><li><code>true</code>: 활성화</li><li><code>false</code>: 비활성화</li></ul>  | 항상  |
| `controls[].name`           | string       | 버튼 이름. 다음과 같은 값이 포함될 수 있습니다.<ul><li><code>"NEXT"</code>: 다음 버튼</li><li><code>"PLAY_PAUSE"</code>: 재생/일시 정지 버튼</li><li><code>"PREVIOUS"</code>: 이전 버튼</li></ul>  | 항상  |
| `controls[].selected`       | boolean      | 미디어 콘텐츠가 선택된 상태 여부. 이 값은 선호 항목의 개념이 들어간 것을 표현할 때 사용될 수 있습니다. 이 값이 `true`로 선택되었다면 사용자가 선호 항목으로 등록해 둔 콘텐츠이기 때문에 미디어 플레이어에서 관련된 UI에 표현해야 합니다. <ul><li><code>true</code>: 선택됨</li><li><code>false</code>: 선택 안됨</li></ul> | 항상  |
| `controls[].type`           | string       | 버튼의 타입. 현재는 `"BUTTON"` 값만 사용됩니다.  | 항상 |
| `displayType`               | string | 미디어 콘텐츠 표시 형태.<ul><li><code>"list"</code>: 목록 표시 형태</li><li><code>"single"</code>: 단일 항목 표시 형태</li></ul>       | 항상 |
| `playableItems[]`           | object array | 재생할 수 있는 미디어 콘텐츠 목록의 정보를 담고 있는 객체 배열입니다. 이 필드는 빈 배열일 수 있습니다.  | 항상 |
| `playableItems[].artImageUrl`  | string    | 미디어 콘텐츠 관련 이미지의 URI. 앨범 자켓 이미지나 관련 아이콘 이미지가 위치한 URI입니다.      | 조건부 |
| `playableItems[].controls[]`                | object array  | 특정 미디어 콘텐츠를 재생할 때 반드시 표시해야 하는 버튼의 정보를 담고 있는 객체 배열입니다. 이 객체 배열은 생략될 수 있습니다.  | 조건부 |
| `playableItems[].controls[].enabled`        | boolean      | `playableItems[].controls[].name`에 명시된 버튼이 미디어 플레이어에서 활성화되어야 하는지 나타냅니다.<ul><li><code>true</code>: 활성화</li><li><code>false</code>: 비활성화</li></ul>  | 항상  |
| `playableItems[].controls[].name`           | string       | 미디어 콘텐츠 관련 버튼 또는 제어 UI의 이름. 다음과 같은 값이 포함될 수 있습니다.<ul><li><code>"BACKWARD_15S"</code>: 15 초 뒤로 이동 버튼</li><li><code>"BACKWARD_30S"</code>: 30 초 뒤로 이동 버튼</li><li><code>"BACKWARD_60S"</code>: 60 초 뒤로 이동 버튼</li><li><code>"FORWARD_15S"</code>: 15 초 앞으로 이동 버튼</li><li><code>"FORWARD_30S"</code>: 30 초 앞으로 이동 버튼</li><li><code>"FORWARD_60S"</code>: 60 초 앞으로 이동 버튼</li><li><code>"LIKE_DISLIKE"</code>: 좋아요/좋아요 취소 버튼</li><li><code>"MORE"</code>: 더보기 버튼. 연관 콘텐츠 목록 표시용 버튼.</li><li><code>"NEXT"</code>: 다음 버튼</li><li><code>"PLAY_PAUSE"</code>: 재생/일시 정지 버튼</li><li><code>"PREVIOUS"</code>: 이전 버튼</li><li><code>"PROGRESS_BAR"</code>: 진행 표시줄</li><li><code>"REPEAT"</code>: 반복 버튼</li><li><code>"SUBSCRIBE_UNSUBSCRIBE"</code>: 구독/구독 취소 버튼</li></ul> 구현에 대한 추가 설명은 [Remarks 항목](#RenderPlayerInfoRemarks)을 참고합니다. | 항상  |
| `playableItems[].controls[].selected`       | boolean      | 미디어 콘텐츠가 선택된 상태 여부. 이 값은 선호 항목의 개념이 들어간 것을 표현할 때 사용될 수 있습니다. 이 값이 `true`로 선택되었다면 사용자가 선호 항목으로 등록해 둔 콘텐츠이기 때문에 미디어 플레이어에서 관련된 UI에 표현해야 합니다. <ul><li><code>true</code>: 선택됨</li><li><code>false</code>: 선택 안됨</li></ul> | 항상  |
| `playableItems[].controls[].type`           | string       | 버튼의 타입. 현재는 `"BUTTON"` 값만 사용됩니다.  | 항상 |
| `playableItems[].headerText`       | string        | 주로 현재 재생 목록의 제목을 표현하는 텍스트 필드                                                | 조건부  |
| `playableItems[].isLive`           | boolean       | 실시간 콘텐츠 여부.<ul><li><code>true</code>: 실시간 콘텐츠</li><li><code>false</code>: 실시간 콘텐츠 아님</li></ul><div class="note"><p><strong>Note!</strong></p><p>실시간 콘텐츠이면 실시간 콘텐츠임을 의미하는 아이콘(예: live 아이콘)을 표시해야 합니다.</p></div>  | 조건부  |
| `playableItems[].lyrics[]`         | object array  | 가사 정보를 담고 있는 객체 배열.                                                            | 조건부  |
| `playableItems[].lyrics[].data`    | string        | 가사 데이터. 이 필드 또는 `playableItems[].lyrics[].url` 필드 중 하나는 존재합니다.              | 조건부  |
| `playableItems[].lyrics[].format`  | string        | 가사 데이터의 포맷.<ul><li><code>"LRC"</code>: <a href="https://en.wikipedia.org/wiki/LRC_(file_format)" target="_blank">LRC 포맷</a></li><li><code>"PLAIN"</code>: 일반 텍스트 형식</li></ul>  | 항상  |
| `playableItems[].lyrics[].url`     | string        | 가사 데이터의 URI. 이 필드 또는 `playableItems[].lyrics[].data` 필드 중 하나는 존재합니다.        | 조건부  |
| `playableItems[].showAdultIcon`    | boolean       | 성인용 콘텐츠를 나타내는 아이콘의 표시 여부.<ul><li><code>true</code>: 표시해야 함.</li><li><code>false</code>: 표시 안해야 함.</li></ul>   | 항상  |
| `playableItems[].titleSubText1`    | string        | 주로 아티스트 이름을 표현하는 텍스트 필드                                                          | 항상 |
| `playableItems[].titleSubText2`    | string        | 주로 앨범 이름을 표현하는 보조 텍스트 필드                                                      | 조건부 |
| `playableItems[].titleText`        | string        | 현재 음악의 제목을 표현하는 텍스트 필드                                                         | 항상  |
| `playableItems[].token`            | string        | 미디어 콘텐츠의 token                                                                     | 항상 |
| `provider`                         | object        | 미디어 콘텐츠 제공자의 정보가 담긴 객체                                                         | 조건부 |
| `provider.logoUrl`                 | string        | 미디어 콘텐츠 제공자 로고 이미지의 URI. 이 필드 또는 필드의 값이 없거나 로고 이미지를 표시할 수 없으면 `provider.name` 필드에 있는 미디어 콘텐츠 제공자의 이름이라도 표시해야 합니다. | 조건부 |
| `provider.name`                    | string        | 미디어 콘텐츠 제공자의 이름                                                                   | 항상  |
| `provider.smallLogoUrl`            | string        | 크기가 작은 미디어 콘텐츠 제공자 로고 이미지의 URI                                                | 조건부 |

### Remarks {#RenderPlayerInfoRemarks}

`playableItems[].controls[]`에 명시된 버튼이나 제어 UI 항목을 구현할 때 일부 UI는 다음을 따라야 합니다.

| UI 구분                   | 가이드 라인                                                  |
|---------------------------|--------------------------------------------------------------|
| 좋아요/좋아요 취소 버튼(`"LIKE_DISLIKE"`)      | 사용자가 이 UI에 해당하는 버튼을 누르면 클라이언트가 조건에 따라 다음과 같이 동작하도록 구현해야 합니다.<ul><li>아직 좋아요를 표시하지 않았을 때: <a href="#LikeCommandIssued"><code>TemplateRuntime.LikeCommandIssued</code></a> 이벤트 메시지를 CIC에 전송해야 합니다.</li><li>이미 좋아요 표시를 했을 때: <a href="#UnlikeCommandIssued"><code>TemplateRuntime.UnlikeCommandIssued</code></a> 이벤트 메시지를 CIC에 전송해야 합니다.</li></ul> |
| 더보기 버튼(`"MORE"`)                          | 사용자가 이 UI에 해당하는 버튼을 누르면 클라이언트는 [`TemplateRuntime.MoreCommandIssued`](#MoreCommandIssued) 이벤트 메시지를 CIC에 전송해야 합니다. |
| 다음 버튼(`"NEXT"`)                            | 사용자가 이 UI에 해당하는 버튼을 누르면 클라이언트는 [`PlaybackController.NextCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#NextCommandIssued) 이벤트 메시지를 CIC에 전송해야 합니다. |
| 재생/일시 정지 버튼(`"PLAY_PAUSE"`)            | 사용자가 이 UI에 해당하는 버튼을 누르면 클라이언트가 조건에 따라 다음과 같이 동작하도록 구현해야 합니다. <ul><li>플레이어가 재생 중인 상태일 때: <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued"><code>PlaybackController.PauseCommandIssued</code></a> 이벤트 메시지를 CIC에 전송해야 합니다.</li><li>플레이어가 재생 일시 정지 상태일 때: <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ResumeCommandIssued"><code>PlaybackController.ResumeCommandIssued</code></a> 이벤트 메시지를 CIC에 전송해야 합니다.</li></ul> |
| 구독/구독 취소 버튼(`"SUBSCRIBE_UNSUBSCRIBE"`) | 사용자가 이 UI에 해당하는 버튼을 누르면 클라이언트가 조건에 따라 다음과 같이 동작하도록 구현해야 합니다. <ul><li>아직 구독하지 않았을 때: <a href="#SubscribeCommandIssued"><code>TemplateRuntime.SubscribeCommandIssued</code></a> 이벤트 메시지를 CIC에 전송해야 합니다.</li><li>이미 구독하고 있을 때: <a href="#UnsubscribeCommandIssued"><code>TemplateRuntime.UnsubscribeCommandIssued</code></a> 이벤트 메시지를 CIC에 전송해야 합니다.</li></ul> |



### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "RenderPlayerInfo",
      "dialogRequestId": "34abac3-cb46-611c-5111-47eab87b7",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
    },
    "payload": {
      "controls": [
        {
          "enabled": true,
          "name": "PLAY_PAUSE",
          "selected": false,
          "type": "BUTTON"
        },
        {
          "enabled": true,
          "name": "NEXT",
          "selected": false,
          "type": "BUTTON"
        },
        {
          "enabled": true,
          "name": "PREVIOUS",
          "selected": false,
          "type": "BUTTON"
        }
      ],
      "displayType": "list",
      "playableItems": [
        {
          "artImageUrl": "https://musicmeta.musicservice.example.com/example/album/662058.jpg",
          "controls": [
            {
              "enabled": true,
              "name": "LIKE_DISLIKE",
              "selected": false,
              "type": "BUTTON"
            }
          ],
          "headerText": "Classic",
          "lyrics": [
            {
              "data": null,
              "format": "PLAIN",
              "url": null
            }
          ],
          "isLive": false,
          "showAdultIcon": false,
          "titleSubText1": "Alice Sara Ott, Symphonie Orchester Des Bayerischen Rundfunks, Esa-Pekka Salonen",
          "titleSubText2": "Wonderland - Edvard Grieg : Piano Concerto, Lyric Pieces",
          "titleText": "Grieg : Piano Concerto In A Minor, Op.16 - 3. Allegro moderato molto e marcato (Live)",
          "token": "eJyr5lIqSSyITy4tKs4vUrJSUE="
        },
        {
          "artImageUrl": "https://musicmeta.musicservice.example.com/example/album/202646.jpg",
          "controls": [
            {
              "enabled": true,
              "name": "LIKE_DISLIKE",
              "selected": false,
              "type": "BUTTON"
            }
          ],
          "headerText": "Classic",
          "lyrics": [
            {
              "data": null,
              "format": "PLAIN",
              "url": null
            }
          ],
          "isLive": true,
          "showAdultIcon": false,
          "titleSubText1": "Berliner Philharmoniker, Herbert Von Karajan",
          "titleSubText2": "Mendelssohn : Violin Concerto; A Midsummer Night`s Dream",
          "titleText": "Symphony No.4 In A Op.90 'Italian' - III. Con Moto Moderato",
          "token": "eJyr5lIqSSyITy4tKs4vUrJSUEo2"
        },
        ...
      ],
      "provider": {
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png",
        "name": "SampleMusicProvider",
        "smallLogoUrl": "https://img.musicservice.example.net/smallLogo_180125.png"
      }
    }
  }
}
```

### See also

* [`AudioPlayer.Play`](#Play)
* [`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)
* [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)
* [`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)
* [`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)
* [`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)

<!-- End of the shared content -->

## RenderPlaylist directive {#RenderPlaylist}

CLOVA가 재생 목록 화면을 표시하도록 클라이언트에 지시합니다. 사용자가 재생 목록 중 하나의 아이템을 선택하면 사용자의 추가 요청이 [`TemplateRuntime.SelectCommandIssued`](#SelectCommandIssued) 이벤트 메시지를 통해 전달되며, 이에 대해 다음과 같은 응답을 받을 수 있습니다.

* [`TemplateRuntime.RenderPlaylist`](#RenderPlaylist) 지시 메시지: 선택한 아이템에 대해 관련된 재생 목록을 추가로 표시
* [AudioPlayer.Play](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지(또는 재생 관련 지시 메시지): 선택한 아이템 재생

<div class="note">
  <p><strong>Note!</strong></p>
  <p>재생 목록은 최소 3번 이상 중첩하여 표시하고 또 되돌아 갈 수 있도록 구현해야 합니다.</p>
</div>

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `title`       | string  | 표시할 재생 목록 화면의 제목         | 항상 |
| `items[]`     | object array | 재생 목록 화면에 표시할 콘텐츠 목록의 정보를 담고 있는 객체 배열 | 항상 |
| `items[].imageUri`   | string  | 미디어 콘텐츠 관련 이미지의 URI. 앨범 아트 이미지나 관련 아이콘 이미지가 위치한 URI입니다. | 조건부 |
| `items[].showAdultIcon` | boolean | 성인용 콘텐츠임을 나타내는 아이콘의 표시 여부. <ul><li><code>true</code>: 표시함 </li><li><code>false</code>: 표시하지 않음 | 항상 |
| `items[].titleSubText1` | string  | 주로 아티스트 이름을 표현하는 보조 텍스트 필드 | 조건부 |
| `items[].titleSubText2` | string  | 주로 앨범 이름을 표현하는 보조 텍스트 필드 | 조건부 |
| `items[].titleText`     | string  | 주로 제목을 표현하는 텍스트 필드 | 항상 |
| `items[].token`         | string  | 미디어 콘텐츠의 token | 항상 | 
| `items[].type`          | string  | 아이템의 속성. 해당 콘텐츠의 속성을 나타내는 값을 입력합니다. 다음과 같은 값을 사용할 수 있습니다. <ul><li><code>playlist</code>: 재생 목록</li><li><code>track</code>: 곡</li><li><code>live</code>: 라이브 음원</li>  | 항상 |

### Message example

```json
// playlist 목록의 응답 예제
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "RenderPlaylist",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
    },
    "payload": {
      "title": "Music",
      "items": [
        {
          "titleText": "History",
          "showAdultIcon": false,
          "imageUri": "https://music.example.com/album/002/295/2295908.jpg?type=r900Fll&v=20180503114007",
          "token": "1d5b84ef-5a15-4892-adad-7584923e47c2",
          "type": "playlist"
        },
        ...,
        {
          "titleText": "Favorite",
          "showAdultIcon": false,
          "imageUri": "https://music.example.com/album/002/000/2000785.jpg?type=r900Fll&v=20190402194015",
          "token": "1d5b84ef-5a15-4192-asdf-7511923e44b5",
          "type": "playlist"
        }
      ]
    }
  }
}

// track 목록의 응답 예제
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "RenderPlaylist",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
    },
    "payload": {
      "title": "지금 들을만한 플레이리스트",
      "items": [
        {
          "titleText": "월광 소나타",
          "titleSubText1": "베토벤",
          "titleSubText2": "",
          "showAdultIcon": false,
          "imageUri": "https://music.example.com/album/002/295/2295908.jpg?type=r900Fll&v=20180503114007",
          "token": "1d5b84ef-5a15-4892-adad-7584923e47c2",
          "type": "track"
        },
        ...,
        {
          "titleText": "꽃의 왈츠",
          "titleSubText1": "차이콥스키",
          "titleSubText2": "호두까기 인형",
          "showAdultIcon": false,
          "imageUri": "https://music.example.com/album/002/000/2000785.jpg?type=r900Fll&v=20190402194015",
          "token": "1d5b84ef-5a15-4192-asdf-7511923e44b5",
          "type": "track"
        }
      ]
    }
  }
}
```

### See also

* [`TemplateRuntime.SelectCommandIssued`](#SelectCommandIssued)

<!-- Start of the shared content: TemplateRuntime.RequestPlayerInfo -->

## RequestPlayerInfo event {#RequestPlayerInfo}

클라이언트가 미디어 플레이어에 표시할 재생 목록, 앨범 이미지, 가사와 같은 재생 메타 정보를 CIC에 요청합니다. 이 이벤트 메시지를 CIC에 전송하면 CIC는 [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `range`        | object  | 재생 메타 정보의 범위를 지정하는 객체. 이 필드가 사용되지 않으면 클라이언트는 임의의 개수만큼 메타 정보를 수신하게 됩니다.   | 선택  |
| `range.after`  | number  | 기준 미디어 콘텐츠로부터 n개만큼 다음 재생 목록에 포함되는 재생 메타 정보를 요청합니다. 예를 들어, `range.before` 필드의 값을 지정하지 않고 `range.after`의 값을 `5`로 설정하면 기준 미디어 콘텐츠를 포함한 총 6 개의 미디어 콘텐츠에 해당하는 재생 메타 정보를 수신하게 됩니다. `1` 이상의 정수 값을 가집니다. | 선택  |
| `range.before` | number  | 기준 미디어 콘텐츠로부터 n개만큼 이전 재생 목록에 포함되는 재생 메타 정보를 요청합니다. `1` 이상의 정수 값을 가집니다. | 선택  |
| `token`        | string  | 재생 메타 정보를 가져올 때 시작 기준이 되는 미디어 콘텐츠의 token. [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지의 `playableItems[].token` 필드로 제공된 token 값이 입력되어야 합니다. | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "RequestPlayerInfo",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```

### See also

* [`TemplateRuntime.ExpectRequestPlayerInfo`](#ExpectRequestPlayerInfo)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)

<!-- End of the shared content -->

## SelectCommandIssued event {#SelectCommandIssued}

사용자가 재생 목록 화면에서 하나의 아이템을 선택하면 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | 미디어 콘텐츠의 token. [`TemplateRuntime.RenderPlaylist`](#RenderPlaylist) 지시 메시지의 `items[].token` 필드에 제공된 token 값을 입력해야 합니다. | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "SelectCommandIssued",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```

### See also

* [`TemplateRuntime.RenderPlaylist`](#RenderPlaylist)

## SubscribeCommandIssued event {#SubscribeCommandIssued}

사용자가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 구독 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. 구독 버튼은 [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지 `playableItems[].controls[].name` 필드의 값이 `"SUBSCRIBE_UNSUBSCRIBE"`인 것을 구현한 버튼으로 현재 콘텐츠를 사용자가 아직 구독하지 않은 상태에서 구독을 요청하는 용도로 사용되는 버튼입니다. 이 이벤트 메시지를 받은 CIC는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | 미디어 콘텐츠의 token. [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지의 `playableItems[].token` 필드로 제공된 token 값이 입력되어야 합니다. | 필수 |

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
      "namespace": "TemplateRuntime",
      "name": "SubscribeCommandIssued",
      "messageId": "ec3deb51-2ed8-47ae-ac17-d2ce24370f8f"
    },
    "payload": {
      "token": "SSyITy4teJyr5lIqKs4vUrJSUE"
    }
  }
}
```

### See also

* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)
* [`TemplateRuntime.UpdateSubscribe`](#UpdateSubscribe)

## UnlikeCommandIssued event {#UnlikeCommandIssued}

사용자가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 좋아요 취소 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. 좋아요 취소 버튼은 [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지 `playableItems[].controls[].name` 필드의 값이 `"LIKE_DISLIKE"`인 것을 구현한 버튼으로 현재 콘텐츠에 사용자가 이미 좋아요를 표시한 상태에서 좋아요 표시를 해제하는 용도로 사용되는 버튼입니다. 이 이벤트 메시지를 받은 CIC는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | 미디어 콘텐츠의 token. [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지의 `playableItems[].token` 필드로 제공된 token 값이 입력되어야 합니다. | 필수 |

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
      "namespace": "TemplateRuntime",
      "name": "UnlikeCommandIssued",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```

### See also

* [`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UpdateLike`](#UpdateLike)

## UnsubscribeCommandIssued event {#UnsubscribeCommandIssued}

사용자가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대해 구독 취소 버튼을 눌렀을 때 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다. 구독 취소 버튼은 [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지 `playableItems[].controls[].name` 필드의 값이 `"SUBSCRIBE_UNSUBSCRIBE"`인 것을 구현한 버튼으로 현재 콘텐츠를 사용자가 이미 구독하고 있는 상태에서 구독을 해제하는 용도로 사용되는 버튼입니다. 이 이벤트 메시지를 받은 CIC는 상황에 맞는 지시 메시지를 클라이언트에 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | 미디어 콘텐츠의 token. [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지의 `playableItems[].token` 필드로 제공된 token 값이 입력되어야 합니다. | 필수 |

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
      "namespace": "TemplateRuntime",
      "name": "UnsubscribeCommandIssued",
      "messageId": "04deb09e-54cc-4525-9e97-4ff4168872b5"
    },
    "payload": {
      "token": "r5lIqKs4vUSSyITy4teJyrJSUE"
    }
  }
}
```

### See also

* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)
* [`TemplateRuntime.UpdateSubscribe`](#UpdateSubscribe)

## UpdateLike directive {#UpdateLike}

클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대한 사용자의 좋아요(like) 여부에 따라 좋아요 표시 상태를 지정된 값으로 업데이트하도록 지시합니다. 사용자의 발화나 좋아요 버튼([`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)) 또는 좋아요 취소 버튼([`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)) 누름 동작에 대한 응답으로 전달되거나 사용자가 다른 기기에서 좋아요와 관련된 동작을 수행했을 때 이를 동기화하기 위해 전달됩니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `like`        | boolean | 좋아요 표시 여부<ul><li><code>true</code>: 좋아요 표시</li><li><code>false</code>: 좋아요 표시 안함</li></ul>             | 항상 |
| `token`       | string  | 미디어 콘텐츠의 token. [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지의 `playableItems[].token` 필드에 해당하는 값이 token 값이 포함됩니다.      | 항상 |

## Remarks

동기화를 위해 이 지시 메시지가 전달될 때는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "UpdateLike",
      "messageId": "fb65457e-bd2e-4876-b739-2b8cc5b67486",
      "dialogRequestId": "94e045dd-78c7-415e-b73b-f0cb9cbfd75d"
    },
    "payload": {
      "token": "r5lIqKs4vUSSyITy4teJyrJSUE",
      "like": true
    }
  }
}
```

### See also

* [`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)

## UpdateSubscribe directive {#UpdateSubscribe}

클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대한 사용자의 구독 여부에 따라 구독(subscribe) 표시 상태를 지정된 값으로 업데이트하도록 지시합니다. 사용자의 발화나 구독 버튼([`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)) 또는 구독 취소 버튼([`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)) 누름 동작에 대한 응답으로 전달되거나 사용자가 다른 기기에서 구독과 관련된 동작을 수행했을 때 이를 동기화하기 위해 전달됩니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `subscribe`   | boolean | 구독 여부<ul><li><code>true</code>: 구독 중임</li><li><code>false</code>: 구독 중이지 않음</li></ul>             | 항상 |
| `token`       | string  | 미디어 콘텐츠의 token. [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) 지시 메시지의 `playableItems[].token` 필드에 해당하는 값이 token 값이 포함됩니다.      | 항상 |

## Remarks

동기화를 위해 이 지시 메시지가 전달될 때는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "UpdateSubscribe",
      "messageId": "fb65457e-bd2e-4876-b739-2b8cc5b67486",
      "dialogRequestId": "94e045dd-78c7-415e-b73b-f0cb9cbfd75d"
    },
    "payload": {
      "token": "r5lIqKs4vUSSyITy4teJyrJSUE",
      "subscribe": true
    }
  }
}
```

### See also

* [`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)
