# Alerts

Alerts 인터페이스는 클라이언트에서 알람을 등록/수정/제거/시작/중지할 때 사용되는 네임스페이스입니다. 사용자는 알람을 음성이나 CLOVA 앱으로 추가할 수 있고 CLOVA 앱으로만 알람을 수정 및 삭제할 수 있습니다. 알람의 종류는 다음 표와 같습니다.

| 알람 종류       | 설명                                |
|---------------|------------------------------------|
| 액션 타이머(`"ACTIONTIMER"`) | 지정한 시간이 경과한 후 특정 동작을 수행하는 알람                               |
| 알람(`"ALARM"`)            | 지정한 날짜와 시간에 울리는 알람                                            |
| 리마인더(`"REMINDER"`)      | 지정한 날짜와 시간에 사용자가 입력한 내용을 표시하거나 들려주면서 울리는 알람. 예를 들면 사용자가 "내일 7 시에 약 먹을 시간이라고 알려줘"라고 하면 "내일 7 시"는 알람을 울려야 하는 시간되고 "약 먹을 시간"은 알람을 울리 때 알려줘야 하는 내용이됩니다. 클라이언트는 알람이 울릴 때 사용자에게 알려줘야 하는 내용을 화면에 표시하거나 음성으로 들려줘야 합니다.       |
| 타이머(`"TIMER"`)           | 지정한 시간이 경과한 후 울리는 알람                                         |

CLOVA는 알람과 관련된 정보를 클라우드 환경에 보관하며, 사용자가 등록한 알람을 필요할 때 클라이언트에 전달합니다. CLOVA는 반복 알람이면 현재 시간에 가장 근접한 알람만 클라이언트에 전달하며, 해당 알람이 중지될 때 다음 차례의 반복 알람을 클라이언트에 전달합니다.

따라서 클라이언트는 Alerts 인터페이스를 이용해 다음과 같은 동작을 수행해야 합니다.
* CLOVA로부터 지시 메시지를 전달받으면 지시 메시지의 내용에 따라 알람을 등록/수정/삭제해야 합니다.
* 알람이 동작해야 하는 시간에 맞춰 알람을 울려줘야 합니다.
* 알람을 생성/수정/삭제/시작/중지했을 때 반드시 이를 CLOVA에 보고해야 그에 상응하는 결과를 CLOVA가 클라이언트에 보내줄 수 있습니다.
* 이벤트 메시지를 보낼 때 반드시 [`Alerts.AlertsState`](/Develop/References/Context_Objects.md#AlertsState) 맥락 객체를 함께 전송하여 클라이언트에 설정된 알람의 상태를 보고해야 합니다.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>알람이 등록, 수정, 제거, 시작, 중지를 구현하는 방법은 <a href="/Develop/Guides/Handle_Alerts.md">알람 처리하기</a>를 참조합니다.</p>
</div>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>알람은 시간을 기반으로 동작합니다. 따라서, 알람이 정확한 시간에 동작하려면 수시로 클라이언트가 표준 시간 동기화를 수행하도록 만들어야 합니다.</p>
</div>

Alerts가 제공하는 이벤트 메시지와 지시 메시지는 다음과 같습니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`AlertStarted`](#AlertStarted)                 | Event     | 알람이 시작되었음을 CLOVA에 보고합니다. |
| [`AlertStopped`](#AlertStopped)                 | Event     | 알람이 중지되었음을 CLOVA에 보고합니다. |
| [`DeleteAlert`](#DeleteAlert)                   | Directive | 클라이언트가 특정 알람을 삭제하도록 지시합니다. |
| [`DeleteAlertFailed`](#DeleteAlertFailed)       | Event     | 클라이언트에 설정된 특정 알람을 삭제하는데 실패했음을 CLOVA에 보고합니다. |
| [`DeleteAlertSucceeded`](#DeleteAlertSucceeded) | Event     | 클라이언트에 설정된 특정 알람을 삭제하는데 성공했음을 CLOVA에 보고합니다. |
| [`RequestAlertStop`](#RequestAlertStop)         | Event     | 클라이언트가 현재 울리고 있는 알람을 중지하도록 CLOVA에 요청합니다.  |
| [`RequestDeleteAlert`](#RequestDeleteAlert)     | Event     | 클라이언트에 등록되어 있는 알람을 삭제하도록 CLOVA에 요청합니다.  |
| [`RequestSynchronizeAlert`](#RequestSynchronizeAlert) | Event | CLOVA의 클라우드 환경에 저장된 사용자의 알람 정보가 클라이언트에 동기화되도록 CLOVA에 요청합니다. |
| [`SetAlert`](#SetAlert)                         | Directive | 클라이언트가 알람을 새로 추가하거나 특정 알람을 수정하도록 지시합니다. |
| [`SetAlertFailed`](#SetAlertFailed)             | Event     | 클라이언트가 특정 알람을 추가 또는 수정하는데 실패했음을 CLOVA에 보고합니다. |
| [`SetAlertSucceeded`](#SetAlertSucceeded)       | Event     | 클라이언트가 특정 알람을 추가 또는 수정하는데 성공했음을 CLOVA에 보고합니다. |
| [`StopAlert`](#StopAlert)                       | Directive | 클라이언트가 특정 알람을 중지하도록 지시합니다.  |
| [`SynchronizeAlert`](#SynchronizeAlert)         | Directive | 클라이언트가 이 메시지에 포함된 사용자의 알람 데이터를 동기화하도록 지시합니다.  |

위 메시지 중 [`RequestSynchronizeAlert`](#RequestSynchronizeAlert) 이벤트 메시지와 [`SynchronizeAlert`](#SynchronizeAlert) 지시 메시지는 CLOVA와 클라이언트 사이에 알람, 일정과 같이 사용자 계정 관련된 정보를 동기화해야 할 때 사용됩니다. 이런 동기화 작업은 다음과 같은 상황에서 발생할 수 있습니다.

* 사용자 계정을 이용하는 클라이언트 기기가 추가되었을 때
* 클라이언트가 네트워크 접속 장애와 같은 이유로 CLOVA에 다시 연결될 때
* 다른 사용자가 사용을 시작하여 클라이언트 기기에 등록된 사용자 계정이 변경될 때
* 클라이언트 기기가 페어링 앱과 연결이 해제된 후 다시 설정될 때

<div class="note">
  <p><strong>Note!</strong></p>
  <p>네트워크가 끊어지거나 사용자 계정 연결이 해제되었을 때 사용자 계정에 등록된 알람 정보를 기기에서 제거해야 합니다.</p>
</div>

## AlertStarted event {#AlertStarted}

알람이 시작되었음을 CLOVA에 보고합니다. 클라이언트는 알람이 시작되었을 때 반드시 이 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 시작된 알람의 식별자                  | 필수 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "AlertStarted",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.AlertStopped`](#AlertStopped)
* [알람 시작/중지하기](/Develop/Guides/Handle_Alerts.md#StartAndStopAlert)

## AlertStopped event {#AlertStopped}

알람이 중지되었음을 CLOVA에 보고합니다. 클라이언트는 울리던 알람이 중지되었을 때 반드시 이 이벤트 메시지를 CLOVA에 전송해야 합니다.
* **일반 알람이 중지되었을 때** CLOVA로부터 [`Alerts.DeleteAlert`](#DeleteAlert) 지시 메시지를 받게 됩니다.
* **반복 알람이 중지되었을 때** 다음 반복 알람을 설정하기 위해 CLOVA로부터 [`Alerts.SetAlert`](#SetAlert) 지시 메시지를 받게 됩니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 중지된 알람의 식별자                  | 필수 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "AlertStopped",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.AlertStarted`](#AlertStarted)
* [알람 시작/중지하기](/Develop/Guides/Handle_Alerts.md#StartAndStopAlert)

## DeleteAlert directive {#DeleteAlert}

클라이언트가 특정 알람을 삭제하도록 지시합니다. 클라이언트는 특정 알람을 삭제해야 하며, 다음과 같은 이벤트 메시지를 사용하여 그 결과를 CLOVA에 보고해야 합니다.

* [`Alerts.DeleteAlertSucceeded`](#DeleteAlertSucceeded) 이벤트 메시지: 특정 알람을 삭제하는데 성공했을 때
* [`Alerts.DeleteAlertFailed`](#DeleteAlertFailed) 이벤트 메시지: 특정 알람을 삭제하는데 실패했을 때

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 삭제해야 할 알람의 식별자              | 항상 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 항상 |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "DeleteAlert",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
      "dialogRequestId": "6b4061db-fbc1-45a2-9c54-b7c62d366b98"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.DeleteAlertFailed`](#DeleteAlertFailed)
* [`Alerts.DeleteAlertSucceeded`](#DeleteAlertSucceeded)
* [알람 수정 또는 삭제하기](/Develop/Guides/Handle_Alerts.md#EditAlert)

## DeleteAlertFailed event {#DeleteAlertFailed}

클라이언트에 설정된 특정 알람을 삭제하는데 실패했음을 CLOVA에 보고합니다. 클라이언트는 [Alerts.DeleteAlert](#DeleteAlert) 지시 메시지를 수신한 후 특정 알람을 삭제하는데 실패하면 반드시 이 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 삭제하지 못한 알람의 식별자             | 필수 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "DeleteAlertFailed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.DeleteAlert`](#DeleteAlert)
* [`Alerts.DeleteAlertSucceeded`](#DeleteAlertSucceeded)
* [알람 수정 또는 삭제하기](/Develop/Guides/Handle_Alerts.md#EditAlert)

## DeleteAlertSucceeded event {#DeleteAlertSucceeded}

클라이언트에 설정된 특정 알람을 삭제하는데 성공했음을 CLOVA에 보고합니다. 클라이언트는 [Alerts.DeleteAlert](#DeleteAlert) 지시 메시지를 수신한 후 특정 알람을 삭제하는데 성공하면 반드시 이 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 삭제한 알람의 식별자                  | 필수 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "DeleteAlertSucceeded",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.DeleteAlert`](#DeleteAlert)
* [`Alerts.DeleteAlertFailed`](#DeleteAlertFailed)
* [알람 수정 또는 삭제하기](/Develop/Guides/Handle_Alerts.md#EditAlert)

## RequestAlertStop event {#RequestAlertStop}

클라이언트가 현재 울리고 있는 알람을 중지하도록 CLOVA에 요청할 때 사용됩니다. 클라이언트는 사용자가 발화가 아닌 물리 버튼(Hardware)이나 소프트웨어 버튼으로 알람을 중지했을 때 이 이벤트 메시지를 CLOVA에 전송해야 합니다. 클라이언트가 이 이벤트 메시지를 CLOVA에 전송하면 나중에 CLOVA로부터 [`Alerts.StopAlert`](#StopAlert) 지시 메시지를 받게됩니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 중지할 알람의 식별자                  | 필수 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 필수 |

### Remarks

클라이언트의 기기에서 사용자가 버튼을 눌러 직접 알람을 중지하더라도 이 이벤트 메시지를 CLOVA에 전송해서 [`Alerts.StopAlert`](#StopAlert) 지시 메시지를 받은 후 알람을 종료해야 합니다. 이는 알람을 종료하는 프로세스를 일관되게 만들며, 알람의 상태를 동기화하는데 도움이 됩니다. 다만, 알람을 종료하기까지 시간이 걸릴 수 있으므로 사용자가 버튼을 눌러 직접 알람을 중지했을 때 [`Alerts.StopAlert`](#StopAlert) 지시 메시지를 받기 전까지 알람 표시를 임의로 없애거나 알람 소리를 무음으로 바꾸는 방식으로 대응할 수 있습니다.

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "RequestAlertStop",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.StopAlert`](#StopAlert)
* [알람 시작/중지하기](/Develop/Guides/Handle_Alerts.md#StartAndStopAlert)

## RequestDeleteAlert event {#RequestDeleteAlert}

클라이언트가 등록된 알람을 삭제하도록 CLOVA에 요청할 때 사용됩니다. 클라이언트는 사용자가 발화가 아닌 물리 버튼(Hardware)이나 소프트웨어 버튼으로 알람을 삭제했을 때 이 이벤트 메시지를 CLOVA에 전송해야 합니다. 클라이언트가 이 이벤트 메시지를 CLOVA에 전송하면 나중에 [`Alerts.DeleteAlert`](#DeleteAlert) 지시 메시지를 받게 됩니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 중지할 알람의 식별자                  | 필수 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 필수 |

### Remarks

클라이언트의 기기에서 사용자가 버튼을 눌러 직접 알람을 삭제하더라도 이 이벤트 메시지를 CLOVA에 전송해서 [`Alerts.DeleteAlert`](#DeleteAlert) 지시 메시지를 받은 후 알람을 삭제해야 합니다. 이는 알람을 삭제하는 프로세스를 일관되게 만들며, 알람의 상태를 동기화하는 데에 도움이 됩니다. 다만, 알람을 삭제하기까지 시간이 걸릴 수 있으므로 사용자가 버튼을 눌러 직접 알람을 삭제했을 때 [`Alerts.DeleteAlert`](#DeleteAlert) 지시 메시지를 받기 전까지 알람 표시를 임의로 없애는 방식으로 대응하면 됩니다.

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "RequestDeleteAlert",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.DeleteAlert`](#DeleteAlert)
* [알람 수정/삭제하기](/Develop/Guides/Handle_Alerts.md#EditAlert)

## RequestSynchronizeAlert event {#RequestSynchronizeAlert}

CLOVA의 클라우드 환경에 저장된 사용자의 알람 정보가 클라이언트에 동기화되도록 CLOVA에 요청합니다. CLOVA가 이 이벤트 메시지를 받으면, 클라이언트에 [`Alerts.SynchronizeAlert`](#SynchronizeAlert) 지시 메시지를 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

없음

### Message example
```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "RequestSynchronizeAlert",
      "messageId": "dd4f2794-6b14-4cc4-ae1b-5bfa1c469028"
    },
    "payload": {}
  }
}
```

### See also

* [`System.SynchronizeAlert`](/Develop/References/MessageInterfaces/Alerts.md#SynchronizeAlert)
* [알람 동기화하기](/Develop/Guides/Handle_Alerts.md#SyncAlert)

## SetAlert directive {#SetAlert}

클라이언트가 알람을 새로 추가하거나 특정 알람을 수정하도록 지시합니다. 클라이언트는 다음과 같이 알람 추가나 알람 수정을 수행할 수 있습니다.

* **`token` 필드의 값과 같은 식별자를 가진 알람이 현재 클라이언트의 기기에 없으면** 전달받은 알람을 추가합니다.
* **`token` 필드의 값과 같은 식별자를 가진 알람이 현재 클라이언트의 기기에 있으면** 해당 알람을 수정합니다.

또한, 다음과 같은 이벤트 메시지를 사용하여 그 결과를 CLOVA에 보고해야 합니다.

* [`Alerts.SetAlertSucceeded`](#SetAlertSucceeded) 이벤트 메시지: 특정 알람을 추가/수정하는데 성공했을 때
* [`Alerts.SetAlertFailed`](#SetAlertFailed) 이벤트 메시지: 특정 알람을 추가/수정하는데 실패했을 때

### Payload field {#SetAlertPayload}

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `assets[]`         | object array | 리마인더(`"REMINDER"`), 액션 타이머(`"ACTIONTIMER"`), 타이머(`"TIMER"`) 타입의 알람을 울릴 때 들려줄 TTS 오디오 목록을 가지는 객체 배열. 리마인더 타입, 액션 타이머, 타이머 타입일 때만 이 필드가 포함됩니다.   | 조건부 |
| `assets[].assetId` | string | TTS 오디오의 식별자       | 항상 |
| `assets[].url`     | string | TTS 오디오의 URI. 만약, 이 필드의 값이 `"clova://alert/bell/{type}"` 형태이면, 클라이언트는 알람의 종류(`type`)에 따라 클라이언트가 보유하고 있는 벨소리 중 그에 맞는 벨소리를 울려야 합니다. 현재 다음과 같은 값이 포함될 수 있습니다. <ul><li><code>"clova://alert/bell/reminder"</code>: 리마인더용 벨소리 재생</li><li><code>"http(s):&ZeroWidthSpace;//example.com/bell/sound"</code> 형태의 값: TTS 오디오 콘텐츠의 URI</li></ul><div class="note"><p><strong>Note!</strong></p><p>이 필드의 값이 <code>"http(s):&ZeroWidthSpace;//example.com/bell/sound"</code> 형태이면 클라이언트는 해당 URI로부터 오디오 콘텐츠를 다운로드하여 알람을 울려야 할 때 재생해야 합니다. 만약 클라이언트가 해당 오디오 콘텐츠를 다운로드할 수 없거나 재생할 수 없는 상태라면 오디오 재생을 생략해야 합니다.</p></div> | 항상 |
| `assetPlayOrder[]` | string array | `assets` 필드에 있는 TTS 오디오의 재생 순서를 저장하고 있는 배열. 배열의 인덱스 순서에 맞춰 재생할 TTS 오디오의 식별자(`assets[].assetId`)가 입력되어 있습니다. 리마인더(`"REMINDER"`), 액션 타이머(`"ACTIONTIMER"`), 타이머(`"TIMER"`) 타입의 알람일 때만 이 필드가 포함됩니다.<div class="note"><p><strong>Note!</strong></p><p>리마인더(<code>"REMINDER"</code>) 또는 타이머(<code>"TIMER"</code>) 타입이면 알람이 반복적으로 재생되어야 합니다. 리마인더 또는 타이머 타입의 알람을 울려야 할 때 이 배열에 명시된 순서대로 오디오 콘텐츠 재생을 반복합니다.</p></div> | 조건부  |
| `label`          | string | 리마인더나 액션 타이머의 내용                             | 조건부 |
| `scheduledTime`  | string | 알람이 울릴 날짜와 시간 정보(`"YYYY-MM-DDThh:mm:ssZ"` 포맷)   | 항상 |
| `token`          | string | 추가 또는 수정해야 할 알람의 식별자                        | 항상 |
| `type`           | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 항상 |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "SetAlert",
      "messageId": "9a440fa9-983a-48a8-8ad5-faee1250abde",
      "dialogRequestId": "688b051d-6832-4bfd-8cf8-5ff073cd2a82"
    },
    "payload": {
      "type": "REMINDER",
      "token": "77179dbd-b65f-4341-a579-c1b2b97fc5b7",
      "scheduledTime": "2017-09-25T09:00:50+09:00",
      "assets": [
        {
          "assetId": "5141f693-5b39-46b7-80e4-3d71ed5508da",
          "url": "clova://alert/bell/reminder"
        },
        {
          "assetId": "b403ebe5-f911-4c5c-98b3-9f5320510235",
          "url": "https://abc.de.fe/tts2"
        }
      ],
      "label": "입금하기",
      "assetPlayOrder": [
        "5141f693-5b39-46b7-80e4-3d71ed5508da",
        "b403ebe5-f911-4c5c-98b3-9f5320510235"
      ]
    }
  }
}
```

### See also

* [`Alerts.SetAlertFailed`](#SetAlertFailed)
* [`Alerts.SetAlertSucceeded`](#SetAlertSucceeded)
* [알람 등록하기](/Develop/Guides/Handle_Alerts.md#RegisterAlert)
* [알람 수정 또는 삭제하기](/Develop/Guides/Handle_Alerts.md#EditAlert)

## SetAlertFailed event {#SetAlertFailed}

클라이언트가 특정 알람을 추가 또는 수정하는데 실패했음을 CLOVA에 보고합니다. 클라이언트는 [Alerts.SetAlert](#SetAlert) 지시 메시지를 수신한 후 특정 알람을 추가 또는 수정하는데 실패하면 반드시 이 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 추가 또는 수정하지 못한 알람의 식별자     | 필수 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "SetAlertFailed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.SetAlert`](#SetAlert)
* [`Alerts.SetAlertSucceeded`](#SetAlertSucceeded)
* [알람 등록하기](/Develop/Guides/Handle_Alerts.md#RegisterAlert)
* [알람 수정 또는 삭제하기](/Develop/Guides/Handle_Alerts.md#EditAlert)

## SetAlertSucceeded event {#SetAlertSucceeded}

클라이언트가 특정 알람을 추가 또는 수정하는데 성공했음을 CLOVA에 보고합니다. 클라이언트는 [Alerts.SetAlert](#SetAlert) 지시 메시지를 수신한 후 특정 알람을 추가 또는 수정하는데 성공하면 반드시 이 이벤트 메시지를 CLOVA에 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 추가 또는 수정한 알람의 식별자          | 필수 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 필수 |

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "SetAlertSucceeded",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.SetAlert`](#SetAlert)
* [`Alerts.SetAlertFailed`](#SetAlertFailed)
* [알람 등록하기](/Develop/Guides/Handle_Alerts.md#RegisterAlert)
* [알람 수정 또는 삭제하기](/Develop/Guides/Handle_Alerts.md#EditAlert)

## StopAlert directive {#StopAlert}

클라이언트가 특정 알람을 중지하도록 지시합니다. 클라이언트는 지정된 알람을 중지해야 하며, [`AlertStopped`](#AlertStopped) 이벤트 메시지를 사용하여 그 결과를 CLOVA에 보고해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 중지해야 할 알람의 식별자              | 항상 |
| `type`    | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 항상 |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "StopAlert",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
      "dialogRequestId": "6b4061db-fbc1-45a2-9c54-b7c62d366b98"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

### See also

* [`Alerts.AlertStopped`](#AlertStopped)
* [알람 시작/중지하기](/Develop/Guides/Handle_Alerts.md#StartAndStopAlert)

## SynchronizeAlert directive {#SynchronizeAlert}

클라이언트가 이 메시지에 포함된 사용자의 알람 데이터를 동기화하도록 지시합니다. 클라이언트는 CLOVA가 전달한 데이터에 맞게 클라이언트에 설정된 알람 값을 변경해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `allAlerts[]`   | object array | 클라이언트가 동기화해야 할 알람 목록을 가지는 객체 배열. [`Alerts.SetAlert`](#SetAlert) 지시 메시지에 사용되는 [`payload`](#SetAlertPayload) 객체와 같은 포맷을 가집니다. | 항상    |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "SynchronizeAlert",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {
      "allAlerts": [
        {
          "type": "REMINDER",
          "token": "77179dbd-b65f-4341-a579-c1b2b97fc5b7",
          "scheduledTime": "2017-09-25T09:00:50+09:00",
          "assets": [
            {
              "assetId": "5141f693-5b39-46b7-80e4-3d71ed5508da",
              "url": "clova://alert/bell/reminder"
            },
            {
              "assetId": "b403ebe5-f911-4c5c-98b3-9f5320510235",
              "url": "https://abc.de.fe/tts2"
            }
          ],
          "assetPlayOrder": ["5141f693-5b39-46b7-80e4-3d71ed5508da", "b403ebe5-f911-4c5c-98b3-9f5320510235"]
        },
        {
          "type": "ALARM",
          "token": "ee4da70c-8328-4456-ab6f-c28cec626ae6",
          "scheduledTime": "2017-09-26T11:00:50+09:00"
        },
        ...
      ]
    }
  }
}
```

### See also

* [`Alerts.RequestSynchronizeAlert`](#RequestSynchronizeAlert)
* [알람 동기화하기](/Develop/Guides/Handle_Alerts.md#SyncAlert)
