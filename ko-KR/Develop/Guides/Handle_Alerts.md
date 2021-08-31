# 알람 처리하기

사용자는 발화를 통해 CLOVA에 알람을 추가하도록 요청할 수 있습니다. CLOVA는 이런 요청을 받으면 알람을 등록하게 하거나 설정한 시간에 알람이 울릴 수 있도록 지시 메시지를 클라이언트로 보냅니다. 클라이언트는 CLOVA가 알람과 관련하여 전달하는 메시지를 상황에 맞게 처리해야 합니다.

여기서 설명할 내용은 다음과 같습니다.
* [알람 등록하기](#RegisterAlert)
* [알람 시작/중지하기](#StartAndStopAlert)
* [알람 수정 또는 삭제하기](#EditAlert)
* [알람 동기화하기](#SyncAlert)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>알람 처리의 동작 구조로 인해 클라이언트가 네트워크에 연결되지 않으면 알람과 관련된 모든 동작이 올바르게 작동하지 않습니다.</p>
</div>

## 알람 등록하기 {#RegisterAlert}

사용자의 발화를 통해 알람을 등록하는 흐름은 다음과 같습니다.

![](/Develop/Assets/Images/CIC_Alerts_Add_Work_Flow.svg)

사용자가 알람 등록을 발화로 요청([`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize))하면 CLOVA는 사용자의 발화를 분석하고 사용자의 클라이언트 기기가 알람을 추가할 수 있도록 [`Alerts.SetAlert`](/Develop/References/MessageInterfaces/Alerts.md#SetAlert) 지시 메시지를 보냅니다.

클라이언트는 `Alerts.SetAlert` 지시 메시지를 받게 되며, 해당 지시 메시지에서 다음과 같은 내용을 확인하여 클라이언트 기기에 알람을 등록해야 합니다.
* 알람 종류
* 알람 적용 시간
* 알람 제목 또는 내용(선택)
* 알람 시작 시 재생할 음원(선택)

클라이언트는 다음과 같은 `Alerts.SetAlert` 지시 메시지를 받을 수 있습니다.

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

위 지시 메시지는 다음과 같은 정보를 가지고 있습니다. 아래 내용을 토대로 클라이언트는 알람을 등록해야 합니다.
* 알람의 종류: `REMINDER`
* 알람 적용 시간: `2017-09-25T09:00:50+09:00`
* 알람의 제목 또는 내용: `입금하기`
* 재생해야 할 음원과 순서
  * 1 번 재생 항목: `clova://alert/bell/reminder`
  * 2 번 재생 항목: `https://abc.de.fe/tts2`

클라이언트가 알람 등록을 시도한 후 그 결과를 CIC에 전달합니다. 이때, 알람 등록에 성공했다면 [`Alerts.SetAlertSucceeded`](/Develop/References/MessageInterfaces/Alerts.md#SetAlertSucceeded) 이벤트 메시지를 CIC에 전송합니다.

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

알람 등록에 실패했다면 [`Alerts.SetAlertFailed`](/Develop/References/MessageInterfaces/Alerts.md#SetAlertFailed) 이벤트 메시지를 사용합니다.

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

CLOVA는 알람이 등록된 결과를 사용자에게 알려주기 위해 [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지와 [`Clova.RenderTemplate`](/Develop/References/MessageInterfaces/Clova.md#RenderTemplate) 지시 메시지를 클라이언트에 전달합니다. 클라이언트는 이 지시 메시지의 내용을 사용자에게 전달해야 합니다.

## 알람 시작/중지하기 {#StartAndStopAlert}

지정한 시간이 되면 클라이언트는 알람을 울려야 합니다. 알람이 시작된 뒤 동작 구조는 다음과 같습니다.

![](/Develop/Assets/Images/CIC_Alerts_Ring_Work_Flow.svg)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>알람은 시간을 기반으로 동작합니다. 따라서, 알람이 정확한 시간에 동작하려면 수시로 클라이언트가 표준 시간 동기화를 수행하도록 만들어야 합니다.</p>
</div>

알람이 시작되면 우선 [`Alerts.AlertStarted`](/Develop/References/MessageInterfaces/Alerts.md#AlertStarted) 이벤트 메시지로 CIC에 보고해야 합니다.

{% raw %}
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
{% endraw %}

클라이언트는 CIC로 전송하는 모든 이벤트 메시지에 현재 울리고 있는 알람 정보를 채워 보내야 합니다. 이때, [`Alert.AlertsState`](/Develop/References/Context_Objects.md#AlertsState) 맥락 정보의 `activeAlerts` 필드를 사용해야 합니다.

이후, 사용자는 발화([`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize))나 물리적 버튼(하드웨어 방식) 또는 GUI 버튼(소프트웨어 방식)으로 알람을 중지하도록 요청할 것입니다. 이때, 클라이언트는 사용자의 알람 중지 요청을 [`Alerts.RequestAlertStop`](/Develop/References/MessageInterfaces/Alerts.md#RequestAlertStop) 이벤트 메시지로 CIC에 보고해야 합니다.

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

CLOVA는 클라이언트가 알람을 중지하도록 클라이언트에 [`Alerts.StopAlert`](/Develop/References/MessageInterfaces/Alerts.md#StopAlert) 지시 메시지를 보냅니다.

클라이언트는 `Alerts.StopAlert` 지시 메시지를 받게 되며, 해당 지시 메시지에서 다음과 같은 내용을 확인하여 클라이언트 기기에서 울리는 중인 알람을 중지해야 합니다.
* 중지해야 할 알람의 식별자
* 알람의 종류

클라이언트는 다음과 같은 `Alerts.StopAlert` 지시 메시지를 받을 수 있습니다.

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

클라이언트는 지시 메시지에 지정된 `token`과 `type`의 내용과 일치하는 알람을 중지해야 합니다.

클라이언트는 알람을 중지한 후 알람이 중지되었음을 [`Alerts.AlertStopped`](/Develop/References/MessageInterfaces/Alerts.md#AlertStopped) 이벤트 메시지로 보고해야 합니다.

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

만일 등록된 알람이 액션 타이머 알람이라면 CLOVA는 클라이언트에 사용자가 예약한 동작에 해당하는 지시 메시지를 전달합니다.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>반복되는 알람이면 현재 시간에서 가장 가까운 반복 알람 한 건만 클라이언트에 등록 및 실행하며, 클라이언트에 등록된 반복 알람이 실행 후 중지되면 다음 차례의 반복 알람을 CIC로부터 새로 받게 됩니다. 만약 네트워크가 장시간 연결되지 않는다면 반복 알람은 제대로 동작하지 않을 수 있습니다.</p>
</div>

<div class="tip">
<p><strong>Tip!</strong></p>
<p>위의 흐름에서 알람을 수정하거나 삭제할 수 있으며, 오직 CLOVA 앱을 통해서 수정 및 삭제할 수 있습니다. 참고로 사용자의 발화에 대한 응답으로 전송되는 지시 메시지가 아니면 해당 지시 메시지는 <a href="/Develop/Guides/Interact_with_CIC.md#CreateConnection">downchannel</a>을 통해 클라이언트에 전달됩니다.</p>
</div>

## 알람 수정 또는 삭제하기 {#EditAlert}

사용자가 CLOVA 앱에서 알람을 수정하거나 삭제를 시도하면 CLOVA는 사용자 요청을 처리하기 위해 [`Alerts.SetAlert`](/Develop/References/MessageInterfaces/Alerts.md#SetAlert) 지시 메시지 또는 [`Alerts.DeleteAlert`](/Develop/References/MessageInterfaces/Alerts.md#DeleteAlert) 지시 메시지를 클라이언트에 보냅니다.

알람을 수정하는 과정은 알람을 등록하는 과정을 반복하는 것과 같기 때문에, [알람 등록하기](#RegisterAlert)를 참고합니다.

알람을 삭제하는 동작 구조는 다음과 같습니다.

![](/Develop/Assets/Images/CIC_Alerts_Delete_Work_Flow.svg)

사용자는 발화([`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize))나 물리적 버튼(하드웨어 방식) 또는 GUI 버튼(소프트웨어 방식)으로 알람을 삭제하도록 요청할 것입니다. 이때, 클라이언트는 사용자의 알람 삭제 요청을 [`Alerts.RequestDeleteAlert`](/Develop/References/MessageInterfaces/Alerts.md#RequestDeleteAlert) 이벤트 메시지로 CIC에 보고해야 합니다. 

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

이후, 클라이언트는 `Alerts.DeleteAlert` 지시 메시지를 받게 되며, 해당 지시 메시지에서 다음과 같은 내용을 확인하여 클라이언트 기기에서 울리는 중인 알람을 삭제해야 합니다.
* 삭제해야 할 알람의 식별자
* 알람의 종류

클라이언트는 다음과 같은 `Alerts.DeleteAlert` 지시 메시지를 받을 수 있습니다.

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

클라이언트는 지시 메시지에 지정된 `token`과 `type`의 내용과 일치하는 알람을 삭제해야 합니다.

클라이언트가 알람을 수정하거나 삭제합니다. 이때, 알람 삭제에 성공했다면 [`Alerts.DeleteAlertSucceeded`](/Develop/References/MessageInterfaces/Alerts.md#DeleteAlertSucceeded) 이벤트 메시지를 CIC에 전송합니다.

{% raw %}
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
{% endraw %}

알람 삭제에 실패했다면 [`Alerts.DeleteAlertFailed`](/Develop/References/MessageInterfaces/Alerts.md#DeleteAlertFailed) 이벤트 메시지를 사용합니다.

{% raw %}
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
{% endraw %}

## 알람 동기화하기 {#SyncAlert}

사용자의 클라이언트가 추가되거나 일부 또는 특정 클라이언트가 네트워크 연결이 끊긴 후 재접속되는 상황 또는 클라이언트에 등록된 사용자의 계정이 변경되면 클라이언트는 CIC에 연결 또는 재연결되면 [`System.RequestSynchronizeState`](/Develop/References/MessageInterfaces/System.md#RequestSynchronizeState) 이벤트 메시지를 CIC로 전송한 후, 클라이언트는 CIC로부터 [`System.SynchronizeState`](/Develop/References/MessageInterfaces/System.md#SynchronizeState) 지시 메시지를 수신하게 되며, 이때 `allAlerts` 필드에 있는 알람 데이터를 기기 알람 정보와 동기화 해야 합니다.

{% raw %}
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
{% endraw %}
