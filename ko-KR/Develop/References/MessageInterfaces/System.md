# System

System 인터페이스는 CLOVA와 클라이언트 사이에서 클라이언트 기기의 펌웨어 정보와 같은 시스템 관련 정보를 동기화해야 할 때 필요한 지시 메시지와 이벤트 메시지를 다음과 같이 제공합니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                 |
|------------------|-----------|-------------------------------------------|
| [`RequestSynchronizeState`](#RequestSynchronizeState)  | Event     | 클라이언트가 시스템 관련 정보를 동기화해야 할 때 이 이벤트 메시지를 CIC로 전송합니다. |
| [`SynchronizeState`](#SynchronizeState)                | Directive | CLOVA가 `payload`에 있는 데이터를 동기화하도록 클라이언트에 지시합니다.            |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>현재 System 네임스페이스는 개편 중에 있으며, 조만간 업데이트될 예정입니다.</p>
</div>

## RequestSynchronizeState event {#RequestSynchronizeState}

클라이언트가 시스템 관련 정보를 동기화해야 할 때 이 이벤트 메시지를 CIC로 전송합니다. CIC는 이 이벤트 메시지를 받으면, 클라이언트에 [`System.SynchronizeState`](#SynchronizeState) 지시 메시지를 전송합니다.

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
      "namespace": "System",
      "name": "RequestSynchronizeState",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {}
  }
}
```

### See also

* [`System.SynchronizeState`](/Develop/References/MessageInterfaces/System.md#SynchronizeState)

## SynchronizeState directive {#SynchronizeState}

CLOVA가 `payload`에 있는 데이터를 동기화하도록 클라이언트에 지시합니다. 클라이언트는 CIC로부터 전달된 데이터에 맞게 클라이언트에 설정된 값을 변경해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `allAlerts[]`   | object array | **(Deprecated)** 클라이언트가 동기화해야 할 알람 목록을 가지는 객체 배열. [`Alerts.SetAlert`](/Develop/References/MessageInterfaces/Alerts.md#SetAlert) 지시 메시지에 사용되는 [`payload`](/Develop/References/MessageInterfaces/Alerts.md#SetAlertPayload) 객체와 같은 포맷을 가집니다. | 항상    |

<div class="note">
  <p><strong>Note!</strong></p>
  <p><code>System.SynchronizeState</code> 지시 메시지를 통해 알람 정보를 동기화하는 것은 더 이상 지원하지 않을 예정이며, 해당 기능은 <a href="/Develop/References/MessageInterfaces/Alerts.md#RequestSynchronizeAlert"><code>Alerts.RequestSynchronizeAlert</code></a> 이벤트 메시지와 <a href="/Develop/References/MessageInterfaces/Alerts.md#SynchronizeAlert"><code>Alerts.SynchronizeAlert</code></a> 지시 메시지를 통해 지원될 예정이며, 이 메시지를 사용해야 합니다. 추후 <code>System.SynchronizeState</code> 지시 메시지에 시스템 관련 정보를 동기화할 수 있는 필드가 추가될 예정입니다.</p>
</div>

### Remarks

추후 동기화 대상이 되는 정보가 더 많아질 수 있습니다.

### Message example

```json
// Deprecated example
{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "SynchronizeState",
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

* [`System.RequestSynchronizeState`](#RequestSynchronizeState)
