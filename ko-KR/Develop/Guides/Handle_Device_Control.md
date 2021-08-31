# 클라이언트 동작 제어 처리하기

사용자는 발화 또는 조작을 통하여 클라이언트의 동작을 제어할 수 있습니다. CLOVA는 이런 요청을 받으면 사용자가 요청한 동작을 수행하도록 지시 메시지를 클라이언트로 보냅니다. 클라이언트는 CLOVA가 동작 제어와 관련하여 전달하는 지시 메시지를 상황에 맞게 처리해야 합니다.

여기서 설명할 내용은 다음과 같습니다.

* [클라이언트 기기 설정 활성화하기](#HandleClientFeatureToggle)
* [클라이언트 볼륨 조정하기](#HandleDeviceVolume)
* [처리 결과 보고하기](#HandleActionExecutedResponse)
* [기기 상태 정보 공유하기](#HandleDeviceStateReport)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>클라이언트에서 지시 메시지를 받은 후 처리에 성공하거나 실패할 때마다 응답 결과를 항상 <code>DeviceControl.ActionExecuted</code>나 <code>DeviceControl.ActionFailed</code> 이벤트 메시지를 통해 CIC에 보고해야 합니다. 자세한 내용은 <a href=#HandleActionExecutedResponse>처리 결과 보고하기</a> 절을 참고합니다.</p>
</div>

## 클라이언트 기기 설정 활성화하기 {#HandleClientFeatureToggle}

사용자는 클라이언트 기기의 특정 설정이 필요할 때 활성화하고 필요하지 않다면 비활성화할 수 있습니다. 기기 설정 활성화는 다음과 같이 두 가지 방식으로 요청할 수 있습니다.

* 사용자가 말로 기능 활성화 제어를 시도(일반적인 방식)
* 사용자가 CLOVA 앱에서 원격으로 특정 클라이언트의 기능 활성화 제어를 시도

기기 설정을 활성화하거나 비활성화하는 것을 처리하는 흐름은 다음과 같습니다.

![](/Develop/Assets/Images/CIC_DeviceControl_Work_Flow1.svg)

사용자가 클라이언트의 제어를 발화로 요청([`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize))합니다.
클라이언트는 사용자의 요청을 이벤트 메시지로 전달합니다. 이때, 이벤트 메시지에는 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 맥락 정보가 포함되어 있어야 합니다.
CIC는 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 맥락 정보에 있는 `actions[]` 필드를 분석하여 사용자의 클라이언트 제어 요청을 해당 클라이언트가 수행할 수 있는지 판단합니다.
클라이언트가 해당 요청을 처리할 수 있을 때 CIC는 사용자의 클라이언트에서 특정 기능을 활성화할 수 있도록 [`DeviceControl.TurnOn`](/Develop/References/MessageInterfaces/DeviceControl.md#TurnOn) 지시 메시지를 보냅니다. 이 지시 메시지에는 기능 활성화에 필요한 정보가 포함되어 있으며, 이 정보를 활용하여 활성화할 기능을 찾아야 합니다. 다음과 같은 [`DeviceControl.TurnOn`](/Develop/References/MessageInterfaces/DeviceControl.md#TurnOn) 지시 메시지를 받을 수 있습니다.

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "TurnOn",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "airplane"
    }
  }
}
```

`payload`의 `target` 필드는 제어 대상을 나타내는 필드이며 다음과 같은 내용이 있을 수 있습니다.

* `airplane`: 비행기 모드
* `bluetooth`: 블루투스
* `cellular`: 모바일 통신
* `flashlight`: 플래시 조명
* `gps`: GPS
* `powersave`: 절전 모드
* `soundmode`: 사운드 모드
* `wifi`: 무선랜

클라이언트는 `target` 필드에 포함된 정보를 이용하여 기능을 활성화 해야합니다. 위 지시 메시지를 토대로 클라이언트는 비행기 모드 기능을 활성화해야 합니다.

클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionFailed) 이벤트 메시지를 이용하여 결과를 CIC에 전달해야 합니다.

## 클라이언트 볼륨 조정하기 {#HandleDeviceVolume}

사용자는 음원을 재생 중인 클라이언트 기기의 볼륨을 조정하도록 CLOVA에 요청할 수 있습니다. 이러한 볼륨 조정은 다음과 같이 세 가지 방식으로 요청할 수 있습니다.

* 사용자가 말로 볼륨 조정을 시도(일반적인 방식)
* 사용자가 클라이언트에서 버튼으로 볼륨 조정을 시도
* 사용자가 CLOVA 앱에서 원격으로 특정 클라이언트의 볼륨 조정을 시도

사용자는 발화([`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)) 또는 기기 조작으로 볼륨을 조정할 것을 요청할 수 있습니다. 사용자가 이런 요청을 하면 CLOVA는 사용자의 발화를 분석하고 사용자의 클라이언트에서 특정 기능을 활성화할 수 있도록 [`DeviceControl.Increase`](/Develop/References/MessageInterfaces/DeviceControl.md#Increase) 지시 메시지를 보냅니다. 또한 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 블루투스 기기 정보를 CIC에 전달해야 합니다.

클라이언트는 사용자가 볼륨을 증가시키거나 감소시키거나 지정함에 따라 다음과 같은 지시 메시지를 받게 되며, 해당 지시 메시지의 내용을 확인하여 클라이언트에서 볼륨을 조정해야 합니다.

클라이언트는 다음과 같은 `DeviceControl.Increase` 지시 메시지를 받을 수 있습니다.

{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Increase",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume"
    }
  }
}
```
{% endraw %}

`payload`의 `target` 필드는 제어 대상을 나타내는 필드이며 볼륨 조정을 하기 위해서는 `target`이 `volume`이어야 합니다.

위 지시 메시지를 토대로 클라이언트는 볼륨을 증가시켜야 합니다. 사용자가 특별히 요청하지 않은 경우 기본 볼륨 증가량만큼 볼륨을 조정해야 합니다. 기본 볼륨 증가량은 개발사에서 원하는 수치로 설정할 수 있습니다.

클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionFailed) 이벤트 메시지를 이용하여 결과를 CIC에 전달해야 합니다.

## 기기 상태 정보 공유하기 {#HandleDeviceStateReport}

연동앱이나 GUI를 제공하는 클라이언트에서 CLOVA 앱과 같이 사용자의 계정에 등록된 전체 클라이언트 기기의 상태를 파악할 수 있도록 UI를 제공할 수 있습니다. 이를 위해 CIC가 클라이언트의 상태 정보를 요청할 때도 있으며, 이때 상태 정보를 공유하도록 요청받은 클라이언트는 **반드시 자신의 상태를 공유해야 합니다.** 다음은 사용자 계정에 등록된 클라이언트 기기의 상태 정보를 파악하는 동작 구조입니다.

![](/Develop/Assets/Images/CIC_DeviceControl_Work_Flow2.svg)

1. CLOVA 앱, 연동앱 또는 GUI를 제공하는 클라이언트와 같이 기기 목록과 상태 정보를 표시하려는 클라이언트(이하 CLOVA 앱)는 [`DeviceControl.RequestDeviceList`](/Develop/References/MessageInterfaces/DeviceControl.md#RequestDeviceList) 이벤트 메시지를 CIC에 전송하여 사용자에 등록된 전체 기기 목록 정보를 요청합니다.
2. CIC는 사용자 계정에 등록된 전체 클라이언트 목록과 각 클라이언트를 식별할 수 있는 정보가 포함된 [`DeviceControl.RenderDeviceList`](/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState) 지시 메시지를 CLOVA 앱에게 전송합니다.
3. CLOVA 앱은 [`DeviceControl.RequestStateSynchronization`](/Develop/References/MessageInterfaces/DeviceControl.md#RequestStateSynchronization) 이벤트 메시지를 CIC에 전송합니다.
4. CIC는 사용자 계정에 등록된 모든 클라이언트(CLOVA 앱 제외)에 [`DeviceControl.ExpectReportState`](/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState) 지시 메시지를 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)로 전송합니다.
5. [`DeviceControl.ExpectReportState`](/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState) 지시 메시지를 수신한 클라이언트는 **[`DeviceControl.ReportState`](/Develop/References/MessageInterfaces/DeviceControl.md#ReportState) 이벤트 메시지를 CIC에 전송하여 현재 자신의 상태를 보고해야 합니다.**
6. CIC는 수집된 클라이언트 상태 정보를 [`DeviceControl.SynchronizeState`](/Develop/References/MessageInterfaces/DeviceControl.md#SynchronizeState) 지시 메시지를 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 이용하여 CLOVA 앱에 보냅니다.
7. [`DeviceControl.SynchronizeState`](/Develop/References/MessageInterfaces/DeviceControl.md#SynchronizeState) 지시 메시지를 받게 되면 CLOVA 앱은 수집된 내용을 클라이언트 식별 정보(`deviceId`)로 구분하여 해당 클라이언트의 상태를 업데이트합니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>클라이언트는 사용자 계정에 새로이 추가되거나 CIC에 다시 연결되었을 때 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState"><code>DeviceControl.ExpectReportState</code></a> 지시 메시지를 받게 됩니다. 이때, 클라이언트는 CLOVA 앱에 상태를 공유할 때처럼 동작해야 합니다.</p>
</div>

## 처리 결과 보고하기 {#HandleActionExecutedResponse}

클라이언트 제어에 성공하거나 실패할 때마다 응답 결과를 항상 `DeviceControl.ActionExecuted`나 `DeviceControl.ActionFailed` 이벤트 메시지를 통해 CIC에 보고해야합니다. 제어에 성공했을 때 다음과 같은 [`DeviceControl.ActionExecuted`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionExecuted) 이벤트 메시지를 CIC에 전송합니다.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "ActionExecuted",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {
      "target": "airplane",
      "command": "TurnOn"
    }
  }
}
```

실패했다면 [`DeviceControl.ActionFailed`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionFailed) 이벤트 메시지를 사용합니다.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "ActionFailed",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {
      "target": "airplane",
      "command": "TurnOn"
    }
  }
}
```

`payload`의 `target` 필드는 제어 대상을 나타내는 필드이며 다음과 같은 내용이 있을 수 있습니다.

* `airplane`: 비행기 모드
* `app`: 앱
* `bluetooth`: 블루투스
* `cellular`: 모바일 통신
* `channel`: TV 채널
* `flashlight`: 플래시 조명
* `gps`: GPS
* `powersave`: 절전 모드
* `screenautobrightness`: 화면 자동 밝기
* `screenbrightness`: 화면 밝기
* `soundmode`: 사운드 모드
* `volume`: 스피커 볼륨
* `wifi`: 무선랜

`payload`의 `command` 필드는 `target` 필드의 제어 대상에 내린 명령이 무엇인지 나타내는 필드이며, CIC에서 클라이언트에 명령을 내릴 때 보낸 지시 메시지 이름을 사용합니다.
