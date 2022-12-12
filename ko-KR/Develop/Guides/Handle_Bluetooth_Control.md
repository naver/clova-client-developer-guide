# 클라이언트 블루투스 제어 처리하기

사용자는 발화 또는 조작을 통하여 클라이언트의 블루투스 연결 동작을 제어할 수 있습니다. CLOVA는 이런 요청을 받으면 사용자가 요청한 동작을 수행하도록 지시 메시지를 클라이언트로 보냅니다. 클라이언트는 CLOVA가 블루투스 제어와 관련하여 전달하는 지시 메시지를 상황에 맞게 처리해야 합니다.

여기서 설명할 내용은 다음과 같습니다.

* [블루투스 기기에 대한 연결 요청 처리하기](#HandleBluetoothConnect)
* [블루투스 기기를 통한 음원 재생 처라히가](#HandleBluetoothPlayAudio)
* [블루투스 기기 연결 해제 처리하기](#HandleBluetoothDisconnect)
* [페어링된 블루투스 기기 삭제 처리하기](#HandleBluetoothDelete)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>클라이언트에서 지시 메시지를 받은 후 처리에 성공하거나 실패할 때마다 응답 결과를 항상 <code>DeviceControl.ActionExecuted</code>나 <code>DeviceControl.ActionFailed</code> 이벤트 메시지를 통해 CLOVA에 보고해야 합니다. 자세한 내용은 <a href="/Develop/Guides/Handle_Device_Control.md">클라이언트 동작 제어 처리하기</a>의 <a href="/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse">처리 결과 보고하기</a> 절을 참고합니다.</p>
</div>

## 블루투스 기기에 대한 연결 요청 처리하기 {#HandleBluetoothConnect}

사용자가 클라이언트와 이전에 연결을 진행한 적이 없는 블루투스 기기를 연결하기 위해서는 클라이언트가 먼저 블루투스 페어링 모드로 진입해야 합니다. 사용자는 CLOVA 앱에서 원격으로 특정 클라이언트의 블루투스 페어링 모드 진입을 시도할 수 있습니다.

블루투스 페어링 모드 진입을 처리하는 동작의 흐름은 다음과 같습니다.

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow1.svg)

사용자가 CLOVA 앱을 통해 블루투스 페어링 모드 진입을 시도한 후 CLOVA는 사용자의 클라이언트에서 블루투스 페어링 모드로 진입하도록 [`DeviceControl.BtStartPairing`](/Develop/References/MessageInterfaces/DeviceControl.md#BtStartPairing) 지시 메시지를 보냅니다. 이 지시 메시지를 받은 클라이언트는 블루투스 페어링 모드로 진입해야 합니다. 다음과 같은 [`DeviceControl.BtStartPairing`](/Develop/References/MessageInterfaces/DeviceControl.md#BtStartPairing) 지시 메시지를 받을 수 있습니다.

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtStartPairing",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {}
  }
}
```

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>페어링 모드 진입은 클라이언트와 특정 블루투스 기기를 최초로 연결할 때만 이루어집니다.</p>
</div>

클라이언트가 페어링 모드에 진입한 상태에서 새로운 블루투스 기기에 대한 연결을 요청하면 CLOVA는 클라이언트가 블루투스 기기와 연결을 하도록 처리해야 합니다.

새로운 블루투스 기기에 대한 연결을 처리하는 흐름은 다음과 같습니다.

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow2.svg)

사용자가 연결할 기기를 지정하면 CLOVA는 클라이언트에 지정된 기기와 연결하도록 [`DeviceControl.BtConnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtConnect) 지시 메시지를 보냅니다. 다음과 같은 [`DeviceControl.BtConnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtConnect) 지시 메시지를 받을 수 있습니다.

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtConnect",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {
      "name": "My Speaker",
      "address": "29:01:11:1f:12:89",
      "connected": false,
      "role": "source"
    }
  }
}
```

새로운 기기와 연결을 진행할 때는 `payload` 내용을 분석해야 합니다.

`payload`의 주요 필드를 간략히 설명하면 다음과 같습니다.
* `address`: 제거할 블루투스 기기의 장치 주소
* `connected`: 제거할 블루투스 기기에 대한 연결 여부
* `name`: 제거할 블루투스 기기의 이름
* `role`: 해당 블루투스 기기와 연결 시 클라이언트의 역할

새로운 블루투스 기기와 연결할 때는 블루투스 기기에서 PIN 코드를 요청합니다. 블루투스 기기에서 PIN 코드를 요청하면 [`DeviceControl.BtRequestForPINCode`](/Develop/References/MessageInterfaces/DeviceControl.md#BtRequestForPINCode) 이벤트 메시지를 이용하여 CLOVA에 요청해야 합니다.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtRequestForPINCode",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
    },
    "payload": {
      "deviceName": "friends device"
    }
  }
}
```

[`DeviceControl.BtRequestForPINCode`](/Develop/References/MessageInterfaces/DeviceControl.md#BtRequestForPINCode) 이벤트 메시지를 전달받은 뒤 사용자가 PIN 코드 입력을 처리하면 CLOVA는 [`DeviceControl.BtConnectByPINCode`](/Develop/References/MessageInterfaces/DeviceControl.md#BtConnectByPINCode) 지시 메시지를 통해 클라이언트가 PIN 코드를 요청한 블루투스 기기와 연결하도록 지시합니다. CLOVA는 연결을 위해 사용된 `pincode` 필드의 값을 클라이언트로 전송합니다.

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtConnectByPINCode",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "pinCode": "1234"
    }
  }
}
```

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>이미 한번 연결을 진행한 기기와 다시 연결할 때는 페어링 모드 진입과 PIN 코드를 확인하는 과정을 생략합니다.</p>
</div>

## 블루투스 기기를 통한 음원 재생 처리하기 {#HandleBluetoothPlayAudio}

사용자가 클라이언트와 연결된 블루투스 기기를 통해 음원을 재생할 것을 요청하면 CLOVA는 클라이언트가 음원을 재생하도록 지시합니다. 블루투스 기기를 통해 음원 재생은 다음과 같이 두 가지 방식으로 요청할 수 있습니다.

* 사용자가 말로 재생 제어를 시도(일반적인 방식)
* 사용자가 CLOVA 앱에서 원격으로 특정 클라이언트에서의 음원 재생을 시도

사용자의 발화를 통한 블루투스 기기 음원 재생 요청을 처리하는 흐름은 다음과 같습니다.

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow3.svg)

사용자가 블루투스 기기를 통한 음원 재생을 요청하면 CLOVA는 클라이언트에 [`DeviceControl.BtPlay`](/Develop/References/MessageInterfaces/DeviceControl.md#BtPlay) 지시 메시지를 보냅니다. 이 때, 음원이 출력되는 곳은 클라이언트의 역할에 따라 결정됩니다. 또한 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 블루투스 기기 정보를 CLOVA에 전달해야 합니다. 다음과 같은 [`DeviceControl.BtPlay`](/Develop/References/MessageInterfaces/DeviceControl.md#BtPlay) 지시 메시지를 받을 수 있습니다.

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtPlay",
      "messageId": "0b75c599-ead8-44a7-ad12-95370b43e7f6",
      "dialogRequestId": "91ee9636-5ede-4658-9df2-ab869e160f52"
    },
    "payload": {}
  }
}
```

클라이언트의 역할이 `"sink"`이면 연결된 블루투스 기기로부터 오디오 스트림을 수신하여 스피커로 음원을 출력합니다.
클라이언트의 역할이 `"source"`이면 클라이언트에서 일시 중지했거나 이전에 재생했던 오디오 스트림을 연결된 블루투스 기기를 통해 재생합니다. 만약, 이전에 재생했던 음원이 없거나 특정 음원을 지정할 수 없는 경우라면 사용자가 "음악 들려줘"라는 발화로 요청한 것과 같은 이벤트 메시지나 제품 UI/UX에 맞는 오디오 콘텐츠 재생 요청을 CLOVA에 전송합니다.

## 블루투스 기기 연결 해제 처리하기 {#HandleBluetoothDisconnect}

사용자가 현재 연결 되어 있는 블루투스 기기에 대한 연결을 해제할 것을 요청하면 CLOVA는 클라이언트가 블루투스 기기에 대한 연결을 해제하도록 지시합니다. 연결 해제를 처리하는 동작의 흐름은 다음과 같습니다.

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow4.svg)

사용자가 블루투스 기기에 대한 연결 해제를 요청하면 CLOVA는 클라이언트에 [`DeviceControl.BtDisconnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDisconnect) 지시 메시지를 보냅니다. 지시 메시지의 내용에 따라 클라이언트는 지정된 블루투스 기기 또는 모든 연결된 블루투스 기기에 대한 연결을 해제해야 합니다. 또한 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 블루투스 기기 정보를 CLOVA에 전달해야 합니다. 모든 연결된 블루투스 기기에 대한 연결을 해제하는 경우 다음과 같은 [`DeviceControl.BtDisconnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDisconnect) 지시 메시지를 받을 수 있습니다.

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtDisconnect",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {}
  }
}
```

## 페어링된 블루투스 기기 삭제 처리하기 {#HandleBluetoothDelete}

사용자가 저장된 블루투스 기기를 삭제할 것을 요청하면 CLOVA는 클라이언트가 지정된 블루투스 기기를 삭제하도록 지시합니다. 저장된 블루투스 기리를 삭제를 처리하는 동작의 흐름은 다음과 같습니다.

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow5.svg)

사용자가 블루투스 기기 삭제를 요청하면 CLOVA는 클라이언트에 [`DeviceControl.BtDelete`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDelete) 지시 메시지를 보냅니다. 클라이언트는 받은 지시 메시지를 분석하여 지정한 기기를 삭제합니다. 다음과 같은 [`DeviceControl.BtDelete`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDelete) 지시 메시지를 받을 수 있습니다.

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtDelete",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {
      "name": "My Speaker",
      "address": "29:01:11:1f:12:89",
      "connected": false,
      "role": "source"
    }
  }
}
```

`payload`의 주요 필드를 간략히 설명하면 다음과 같습니다.
* `address`: 제거할 블루투스 기기의 장치 주소
* `connected`: 제거할 블루투스 기기에 대한 연결 여부
* `name`: 제거할 블루투스 기기의 이름
* `role`: 해당 블루투스 기기와 연결 시 클라이언트의 역할
