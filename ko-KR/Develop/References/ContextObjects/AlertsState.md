## Alerts.AlertsState {#AlertsState}

`Alerts.AlertsState`는 클라이언트의 알람 정보를 CIC에 보고할 때 사용하는 메시지 포맷입니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>이 맥락 정보를 작성할 때 <a href="/Develop/References/MessageInterfaces/Alerts.md">Alerts</a>의 지시 메시지가 보내준 알람의 정보를 그대로 입력해야 합니다.</p>
</div>

### Object structure

{% raw %}
```json
{
  "header": {
    "namespace": "Alerts",
    "name": "AlertsState"
  },
  "payload": {
    "allAlerts": [
      {
        "token": {{string}},
        "type": {{string}},
        "scheduledTime": {{string}}
      }
    ],
    "activeAlerts": [
      {
        "token": {{string}},
        "type": {{string}},
        "scheduledTime": {{string}}
      }
    ]
  }
}
```
{% endraw %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `activeAlerts[]` | [AlertInfoObject](#AlertInfoObject) array | 클라이언트에서 현재 울리고 있는 알람 목록을 가지는 객체 배열. 현재 울리고 있는 알람이 없으면 빈 배열을 입력합니다.  | 필수 |
| `allAlerts[]`    | [AlertInfoObject](#AlertInfoObject) array | 클라이언트에 설정된 전체 알람 목록을 가지는 객체 배열. 이 배열에 클라이언트가 설정하고 있는 모든 알람 정보를 입력해야 합니다.    | 필수 |

### Object example

```json
{
  "context": [
    {
      "header": {
        "namespace": "Alerts",
        "name": "AlertsState"
      },
      "payload": {
        "allAlerts": [
          {
            "token": "78434957-c0db-47d9-9104-3d7899df3d4e",
            "type": "ALARM",
            "scheduledTime": "2017-09-23T11:11:11Z"
          },
          {
            "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
            "type": "TIMER",
            "scheduledTime": "2017-09-14T22:22:22Z"
          },
          {
            "token": "5b3eb1a0-3a98-43ca-b28b-040462c2d2c9",
            "type": "ALARM",
            "scheduledTime": "2017-09-16T12:33:44Z"
          }
        ],
        "activeAlerts": [
          {
            "token": "5b3eb1a0-3a98-43ca-b28b-040462c2d2c9",
            "type": "ALARM",
            "scheduledTime": "2017-09-16T12:33:44Z"
          }
        ]
      }
    }
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "AlertStopped",
      "messageId": "b19028a9-e566-4ad9-90c0-4c5fecc9b336",
      "dialogRequestId": "862f996c-69cc-4e53-94df-9aed82b23579"
    },
    "payload": {
      "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
      "type": "TIMER",
      "scheduledTime": "2017-09-14T22:22:22Z"
    }
  }
}
```

### AlertInfoObject {#AlertInfoObject}

개별 알람의 정보를 가지는 객체입니다. 개별 알람에 대한 정보를 이 객체 포맷에 맞게 입력합니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `scheduledTime` | string | 알람이 울릴 날짜와 시간 정보(`"YYYY-MM-DDThh:mm:ssZ"` 포맷)   | 필수 |
| `token`         | string | 알람의 식별자                   | 필수 |
| `type`          | string | 알람의 종류. 다음과 같은 값을 가집니다. <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | 필수 |

#### Object example

```json
예제 1: ALARM 타입
{
  "token": "5b3eb1a0-3a98-43ca-b28b-040462c2d2c9",
  "type": "ALARM",
  "scheduledTime": "2017-09-16T12:33:44Z"
}

예제 2: TIMER 타입
{
  "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
  "type": "TIMER",
  "scheduledTime": "2017-09-14T22:22:22Z"
}

예제 3: REMINDER 타입
{
  "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
  "type": "REMINDER",
  "scheduledTime": "2017-09-14T22:22:22Z"
}

```

### See also

* [`Alerts`](/Develop/References/MessageInterfaces/Alerts.md)
