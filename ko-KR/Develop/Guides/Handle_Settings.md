# 설정 정보 처리하기

클라이언트는 설정 정보와 관련하여 다음과 같은 작업을 수행해야 합니다.

* [설정 정보 동기화하기](#SynchronizeSettings)
* [CLOVA 제공 설정 적용하기](#ApplySettingsProvidedByCLOVA)

## 설정 정보 동기화하기 {#SynchronizeSettings}

사용자는 클라이언트 기기 자체나 CLOVA 앱에서 클라이언트의 설정을 변경하거나 CLOVA 앱에서 특정 클라이언트의 설정 정보를 조회할 수 있습니다. 이를 위해 CLOVA는 클라이언트가 설정 정보를 보고하도록 만들거나 설정 정보를 변경하도록 지시 메시지를 클라이언트에 보냅니다. 이때, [`Settings`](/Develop/References/MessageInterfaces/Settings.md) 인터페이스를 사용하며, 다음과 같은 처리가 필요합니다.

* 사용자가 CLOVA 앱에서 클라이언트 기기의 설정 정보를 조회할 때 **설정 정보 동기화**가 필요합니다.
* 사용자가 클라이언트 기기의 **설정을 CLOVA 앱에서 원격으로 변경**했을 때 이를 기기의 설정 정보에 반영해야 합니다.
* 사용자가 클라이언트 기기에서 **직접 설정을 변경**했을 때 이를 CLOVA 앱에 반영해야 합니다.

사용자가 CLOVA 앱에서 클라이언트 기기의 설정 정보를 조회할 때 CLOVA 앱은 클라이언트 기기의 최신 설정 정보를 사용자에게 제공해야 합니다. 이런 **설정 정보 동기화**는 다음과 같은 동작 흐름을 가집니다.

![](/Develop/Assets/Images/CIC_Settings_Synchronize_Settings_Info.svg)

설정 정보를 동기화할 때 CLOVA는 다음과 같은 [`Settings.ExpectReport`](/Develop/References/MessageInterfaces/Settings.md#ExpectReport) 지시 메시지를 클라이언트로 보냅니다.

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "ExpectReport",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {}
  }
}
```

위 지시 메시지를 받은 클라이언트는 현재 설정 정보를 담은 [`Settings.Report`](/Develop/References/MessageInterfaces/Settings.md#Report) 이벤트 메시지를 CLOVA에 전송하면 됩니다. CLOVA가 이 이벤트 메시지를 받으면 보고 받은 정보를 CLOVA 앱이 업데이트하도록 만듭니다.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Settings",
      "name": "Report",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "configuration": {
        "key1": "value1",
        "key2": "value2",
        ...
      }
    }
  }
}
```

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>설정 데이터는 클라이언트마다 다르게 정의될 수 있습니다. 클라이언트 설정 정보에 대한 사전 정의는 제휴 담당자에게 문의합니다.</p>
</div>

사용자가 클라이언트 기기의 **설정을 CLOVA 앱에서 원격으로 변경**하면 이를 클라이언트 기기의 설정에 반영해야 합니다. 이런 원격 설정 변경은 다음과 같은 동작 흐름으로 구현됩니다.

![CIC_Settings_Change_Settings_Via_Clova_App](/Develop/Assets/Images/CIC_Settings_Change_Settings_Via_Clova_App.svg)

사용자가 CLOVA 앱에서 클라이언트의 설정을 원격으로 바꿨을 때 CLOVA는 [`Settings.Update`](/Develop/References/MessageInterfaces/Settings.md#Update) 지시 메시지를 클라이언트 기기에 전송합니다. 다음은 음성 설정을 바꾸도록 지시하는 `Settings.Update` 지시 메시지 예입니다.

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "Update",
      "messageId": "332a2c13-ad7c-9d8e-3acd-983a24bab3ca"
    },
    "payload": {
      "configuration": {
        "VoiceTypeChange": "V001"
      }
    }
  }
}
```

위 지시 메시지에 정의된 설정 정보에 따라 클라이언트의 설정을 바꾸고 현재 설정 정보를 담은 [`Settings.Report`](/Develop/References/MessageInterfaces/Settings.md#Report) 이벤트 메시지를 CLOVA에 전송하면 됩니다. 사용자는 CLOVA 앱을 통해 변경된 설정 정보를 확인할 수 있게 됩니다.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Settings",
      "name": "Report",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "configuration": {
        "key1": "value1",
        "key2": "value2",
        ...
      }
    }
  }
}
```

사용자가 직접 클라이언트 기기에서 **직접 설정을 변경**할 수 있으며 동작 흐름은 다음과 같습니다. 이때 클라이언트는 사용자가 변경한 후의 설정 정보를 [`Settings.Report`](/Develop/References/MessageInterfaces/Settings.md#Report) 이벤트 메시지를 이용하여 CLOVA에 전송하면 됩니다.

![CIC_Settings_Change_Settings_On_Device](/Develop/Assets/Images/CIC_Settings_Change_Settings_On_Device.svg)

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>설정 데이터는 클라이언트마다 다르게 정의될 수 있습니다. 클라이언트 설정 정보에 대한 사전 정의는 제휴 담당자에게 문의합니다.</p>
</div>

## CLOVA 제공 설정 적용하기 {#ApplySettingsProvidedByCLOVA}

클라이언트는 기본으로 제공하는 설정 외에 CLOVA가 제공하는 설정을 사용자에게 추가로 제공해야 합니다. 이를 위해 클라이언트는 CLOVA에 연결된 뒤 추가 설정 정보를 CLOVA에 요청하고 이에 대한 응답을 받아 클라이언트 설정 메뉴에 적용해야 합니다. CLOVA가 제공하는 설정을 클라이언트에 적용하는 동작 구조는 다음과 같습니다.

![CIC_Settings_Apply_Version_Spec](/Develop/Assets/Images/CIC_Settings_Apply_Version_Spec.svg)

1. 클라이언트는 [`Settings.RequestVersionSpec`](/Develop/References/MessageInterfaces/Settings.md#RequestVersionSpec) 이벤트 메시지를 CLOVA에 전송하여 클라이언트와 호환되는 추가 설정 정보를 요청합니다.
2. CLOVA는 클라이언트에 호환되는 설정 정보를 [`Settings.UpdateVersionSpec`](/Develop/References/MessageInterfaces/Settings.md#UpdateVersionSpec) 지시 메시지를 사용하여 응답으로 보냅니다.
3. 클라이언트는 `Settings.UpdateVersionSpec` 지시 메시지를 수신하면 해당 지시 메시지에 포함된 내용을 설정 메뉴에 반영합니다.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>현재 CLOVA가 클라이언트에 추가 제공하고 있는 설정 항목은 음성 설정(<code>spec.voiceType</code>)입니다. 추후 제공되는 설정 항목이 변경될 수 있습니다.</p>
</div>