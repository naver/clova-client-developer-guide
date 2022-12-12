# DeviceControl

DeviceControl 인터페이스는 클라이언트 기기를 제어하거나 클라이언트 기기 제어 수행 결과를 CLOVA에 보고할 때 사용되는 네임스페이스입니다.

일부 사용자의 요청은 클라이언트 기기를 제어하는 요청일 수 있습니다. 분석된 사용자의 요청이 클라이언트 기기를 제어하는 요청이면 네임스페이스 `DeviceControl`인 지시 메시지를 받게 되며 클라이언트는 수신한 지시 메시지에 맞게 클라이언트 기기를 제어해야 합니다. 클라이언트 기기 제어를 수행한 후 그 결과를 이벤트 메시지를 사용하여 CLOVA에 전송해야 합니다. 자세한 설명은 [클라이언트 동작 제어 처리하기](/Develop/Guides/Handle_Device_Control.md)를 참조합니다.

클라이언트 기기는 `DeviceControl`의 메시지를 통해 외부 블루투스 기기와 연결할 수 있습니다. CLOVA는 클라이언트에 블루투스 페어링 및 연결을 위한 지시 메시지를 보내 외부 블루투스 기기와 연결하도록 지시하며, 클라이언트는 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 맥락 정보의 [`BluetoothInfoObject`](/Develop/References/Context_Objects.md#BluetoothInfoObject)를 통해 페어링된 기기 정보와 같은 블루투스 관련 정보를 수시로 CLOVA에 보고하게 됩니다. 자세한 연결 방법은 각 지시 메시지 및 이벤트 메시지를 참조합니다.

DeviceControl이 제공하는 이벤트 메시지와 지시 메시지는 다음과 같습니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`ActionExecuted`](#ActionExecuted)       | Event     | 기기 제어 작업이 정상적으로 수행되었음을 CLOVA에 보고합니다.           |
| [`ActionFailed`](#ActionFailed)           | Event     | 기기 제어 작업을 수행할 수 없거나 실패했음을 CLOVA에 보고합니다. |
| [`BtConnect`](#BtConnect)                 | Directive | 클라이언트가 블루투스 기기와 연결을 설정하도록 지시합니다.                               |
| [`BtConnectByPINCode`](#BtConnectByPINCode) | Directive | 클라이언트가 PIN 코드를 요청한 블루투스 기기와 연결하도록 지시합니다.                        |
| [`BtDelete`](#BtDelete)                   | Directive | 클라이언트가 블루투스 페어링 목록에서 특정 기기를 제거하도록 지시합니다.                        |
| [`BtDisconnect`](#BtDisconnect)           | Directive | 클라이언트가 연결된 블루투스 기기와 연결을 끊도록 지시합니다.                               |
| [`BtPlay`](#BtPlay)                       | Directive | 클라이언트가 연결된 블루투스 기기를 통해 오디오 콘텐츠를 재생하도록 지시합니다.                          |
| [`BtRequestForPINCode`](#BtRequestForPINCode) | Event | 외부 블루투스 기기가 PIN 코드 입력 요청을 할 때 해당 PIN 코드를 CLOVA에 전달합니다.     |
| [`BtRequestToCancelPINCodeInput`](#BtRequestToCancelPINCodeInput) | Event | PIN 코드 입력을 취소하도록 CLOVA에 요청합니다. |
| [`BtRescan`](#BtRescan)                   | Directive | 클라이언트가 블루투스 기기를 재탐지(rescan)하도록 지시합니다.                               |
| [`BtStartPairing`](#BtStartPairing)       | Directive | 클라이언트가 블루투스 페어링을 시작하도록 지시합니다.                                       |
| [`BtStopPairing`](#BtStopPairing)         | Directive | 클라이언트가 블루투스 페어링을 중지하도록 지시합니다.                                       |
| [`Decrease`](#Decrease)                   | Directive | 클라이언트가 스피커 볼륨 또는 화면 밝기를 기본 단위만큼 줄이도록 지시하거나, TV 채널을 이전 채널로 변경하도록 지시합니다.                     |
| [`ExpectReportState`](#ExpectReportState) | Directive | 기기의 현재 상태를 CLOVA에 보고합니다.                                 |
| [`Increase`](#Increase)                   | Directive | 클라이언트가 스피커 볼륨 또는 화면 밝기를 기본 단위만큼 높이도록 지시하거나, TV 채널을 다음 채널로 변경하도록 지시합니다.                     |
| [`LaunchApp`](#LaunchApp)                 | Directive | **(Deprecated)** 클라이언트가 특정 앱을 실행하도록 지시합니다.                           |
| [`Open`](#Open)                           | Directive | 클라이언트가 특정 화면을 표시하도록 지시합니다.                                               |
| [`OpenScreen`](#OpenScreen)               | Directive | **(Deprecated)** 클라이언트가 설정 화면을 열도록 지시합니다.                              |
| [`RenderDeviceList`](#RenderDeviceList)   | Directive   | 클라이언트가 사용자 계정에 등록된 클라이언트 기기 목록을 표시하도록 지시합니다.  |
| [`ReportState`](#ReportState)             | Event     | 기기의 현재 상태를 CLOVA에 보고합니다.                     |
| [`RequestDeviceList`](#RequestDeviceList)             | Event     | 사용자 계정에 등록된 클라이언트 기기 목록과 각 클라이언트의 식별 정보를 CLOVA에 요청합니다. |
| [`RequestStateSynchronization`](#RequestStateSynchronization) | Event   | 사용자의 계정에 등록된 다른 클라이언트 기기의 현재 상태를 CLOVA에 요청합니다.  |
| [`SetValue`](#SetValue)                   | Directive | 클라이언트가 스피커 볼륨 또는 화면 밝기를 지정한 값으로 설정하거나 특정 TV 채널로 변경하도록 지시합니다.                    |
| [`SynchronizeState`](#SynchronizeState)   | Directive | 클라이언트가 사용자 계정에 등록된 또 다른 클라이언트 기기의 상태 정보를 업데이트하도록 지시합니다.          |
| [`TurnOff`](#TurnOff)                     | Directive | 클라이언트가 지정한 기능이나 모드를 끄거나 비활성화하도록 지시합니다.                           |
| [`TurnOn`](#TurnOn)                       | Directive | 클라이언트가 지정한 기능이나 모드를 켜거나 활성화하도록 지시합니다.                                   |

## ActionExecuted event {#ActionExecuted}

기기 제어 작업이 정상적으로 수행되었음을 CLOVA에 보고합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `command`     | string  | 정상 수행한 동작.<ul><li> <code>"BtConnect"</code></li><li><code>"BtConnectByPINCode"</code></li><li><code>"BtDisconnect"</code></li><li><code>"BtStartPairing"</code></li><li><code>"BtStopPairing"</code></li><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"Open"</code></li><li><code>"SetValue"</code></li><li><code>"TurnOn"</code></li><li><code>"TurnOff"</code></li></ul> | 필수   |
| `target`      | string  | 제어 대상.<ul><li><code>"airplane"</code>: 비행기 모드</li><li><code>"app"</code>: 앱</li><li><code>"bluetooth"</code>: 블루투스</li><li><code>"cellular"</code>: 모바일 통신</li><li><code>"channel"</code>: TV 채널</li><li><code>"flashlight"</code>: 플래시 조명</li><li><code>"gps"</code>: GPS</li><li><code>"powersave"</code>: 절전 모드</li><li><code>"screenautobrightness"</code>: 화면 자동 밝기</li><li><code>"screenbrightness"</code>: 화면 밝기</li><li><code>"soundmode"</code>: 사운드 모드</li><li><code>"volume"</code>: 스피커 볼륨</li><li><code>"wifi"</code>: 무선랜</li></ul> | 필수     |

### Remarks

CLOVA가 이 이벤트 메시지를 수신하면 사용자 계정에 등록된 모든 클라이언트에 [`SynchronizeState`](#SynchronizeState) 지시 메시지를 전송하여 특정 클라이언트 기기의 변경된 상태 정보를 알립니다.

### Message example

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
      "target": "gps",
      "command": "TurnOn"
    }
  }
}
```

### See also

* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtConnectByPINCode`](#BtConnectByPINCode)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.Decrease`](#Decrease)
* [`DeviceControl.Increase`](#Increase)
* [`DeviceControl.Open`](#Open)
* [`DeviceControl.SetValue`](#SetValue)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [클라이언트 동작 제어 처리하기](/Develop/Guides/Handle_Device_Control.md)
* [클라이언트 블루투스 제어 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md)

## ActionFailed event {#ActionFailed}

기기 제어 작업을 수행할 수 없거나 실패했음을 CLOVA에 보고합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `command`     | string  | 실패한 동작. <ul><li><code>"BtConnect"</code></li><li><code>"BtConnectByPINCode"</code></li><li><code>"BtDisconnect"</code></li><li><code>"BtStartPairing"</code></li><li><code>"BtStopPairing"</code></li><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"Open"</code></li><li><code>"SetValue"</code></li><li><code>"TurnOn"</code></li><li><code>"TurnOff"</code></li></ul> | 필수   |
| `target`      | string  | 제어 대상.<ul><li><code>"airplane"</code>: 비행기 모드</li><li><code>"app"</code>: 앱</li><li><code>"bluetooth"</code>: 블루투스</li><li><code>"cellular"</code>: 모바일 통신</li><li><code>"channel"</code>: TV 채널</li><li><code>"flashlight"</code>: 플래시 조명</li><li><code>"gps"</code>: GPS</li><li><code>"powersave"</code>: 절전 모드</li><li><code>"screenautobrightness"</code>: 화면 자동 밝기</li><li><code>"screenbrightness"</code>: 화면 밝기</li><li><code>"soundmode"</code>: 사운드 모드</li><li><code>"volume"</code>: 스피커 볼륨</li><li><code>"wifi"</code>: 무선랜</li></ul> | 필수     |

### Remarks

* CLOVA가 이 이벤트 메시지를 수신하면 사용자 계정에 등록된 모든 클라이언트에 [`SynchronizeState`](#SynchronizeState) 지시 메시지를 전송하여 특정 클라이언트 기기의 변경된 상태 정보를 알립니다.
* [`LaunchApp`](#LaunchApp) 지시 메시지를 수신한 후 앱 실행에 실패하면 `target` 필드는 `"app"`으로 설정합니다.

### Message example

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
      "target": "gps",
      "command": "TurnOn"
    }
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtConnectByPINCode`](#BtConnectByPINCode)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.Decrease`](#Decrease)
* [`DeviceControl.Increase`](#Increase)
* [`DeviceControl.Open`](#Open)
* [`DeviceControl.SetValue`](#SetValue)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [클라이언트 동작 제어 처리하기](/Develop/Guides/Handle_Device_Control.md)
* [클라이언트 블루투스 제어 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md)

## BtConnect directive {#BtConnect}

클라이언트가 블루투스 기기와 연결을 설정하도록 지시합니다. 다음과 같이 상황을 구분합니다.

| 상황               | 설명                                     |
|--------------------|------------------------------------------|
| 연결할 기기를 지정하지 않았을 때 | 클라이언트는 각자의 기준에 따라 페어링된 기기 중 어떤 것과 연결할지 결정해야 합니다. 예를 들면, 가장 최근에 연결했던 순서대로 기기 연결을 시도할 수 있습니다. |
| 클라이언트 역할만 지정했을 때    | 클라이언트가 <code>"sink"</code> 역할을 할 때와 <code>"source"</code> 역할을 할 때, 각각에 따라 페어링된 기기 중 어떤 것과 연결할지 결정해야 합니다. |
| 연결할 기기를 지정했을 때        | 클라이언트는 지정된 기기와 연결을 시도해야 합니다. CLOVA는 클라이언트 기기와 페어링된 블루투스 기기 중 하나를 지정합니다. |

### Payload fields

* 연결할 기기를 지정하지 않을 때
없음

* 클라이언트의 역할만 지정했을 때

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `role`        | string  | 블루투스 기기와 연결 시 클라이언트의 역할.<ul><li><code>"sink"</code>: 오디오 스트림을 수신하는 역할(주로 스피커)</li><li><code>"source"</code>: 오디오 스트림을 송신하는 역할(음원 데이터 전달자)</li></ul> | 항상     |

* 연결할 기기를 지정할 때

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `address`     | string  | 연결할 블루투스 기기의 장치 주소     | 항상     |
| `connected`   | boolean | 연결할 블루투스 기기에 대한 연결 여부. <ul><li><code>true</code>: 연결된 상태</li><li><code>false</code>: 연결되어 있지 않은 상태</li></ul>      | 항상     |
| `name`        | string  | 연결할 블루투스 기기의 이름         | 항상     |
| `role`        | string  | 해당 블루투스 기기와 연결 시 클라이언트의 역할.<ul><li><code>"sink"</code>: 오디오 스트림을 수신하는 역할(주로 스피커)</li><li><code>"source"</code>: 오디오 스트림을 송신하는 역할(음원 데이터 전달자)</li></ul> | 항상     |

### Remarks

* `payload` 없이 이 메시지를 받으면 클라이언트는 페어링된 블루투스 기기 중 하나와 연결을 시도해야 합니다.
* `payload`에 `role` 필드만 있는 메시지를 받으면 클라이언트 역할에 맞춰 페어링된 블루투스 기기 중 하나와 연결을 시도해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.
* 클라이언트는 [`DeviceControl.ReportState`](#ReportState) 이벤트 메시지의 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 맥락 정보에 실제 연결 결과를 포함시켜 CLOVA에 전송해야 합니다. 합니다.

### Message example

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
      "role": "sink"
    }
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [블루투스 기기 연결 해제 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDisconnect)
* [블루투스 기기에 대한 연결 요청 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [클라이언트 기기 설정 활성화하기](/Develop/Guides/Handle_Device_Control.md#HandleClientFeatureToggle)

## BtConnectByPINCode directive {#BtConnectByPINCode}

클라이언트가 PIN 코드를 요청한 블루투스 기기와 연결하도록 지시합니다. CLOVA는 이 메시지에 사용자가 입력한 PIN 코드를 전달합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `pinCode`     | string  | 블루투스 PIN 코드. 이 필드가 빈 문자열(`""`)이면 클라이언트는 페어링을 중단해야 합니다. | 항상     |

### Remarks

* PIN 코드를 사용하지 않는 블루투스 기기와 연결을 요청할 때는 이 지시 메시지를 전달하지 않습니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.
* 클라이언트는 [`DeviceControl.ReportState`](#ReportState) 이벤트 메시지의 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 맥락 정보에 실제 연결 결과를 포함시켜 CLOVA에 전송해야 합니다. 합니다.

### Message example

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

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtRequestForPINCode`](#BtRequestForPINCode)
* [`DeviceControl.ReportState`](#ReportState)
* [블루투스 기기에 대한 연결 요청 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)

## BtDelete directive {#BtDelete}

클라이언트가 블루투스 페어링 목록에서 특정 기기를 제거하도록 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `address`     | string  | 제거할 블루투스 기기의 장치 주소     | 항상     |
| `connected`   | boolean | 제거할 블루투스 기기에 대한 연결 여부. <ul><li><code>true</code>: 연결된 상태</li><li><code>false</code>: 연결되어 있지 않은 상태</li></ul>      | 항상     |
| `name`        | string  | 제거할 블루투스 기기의 이름         | 항상     |
| `role`        | string  | 해당 블루투스 기기와 연결 시 클라이언트의 역할.<ul><li><code>"sink"</code>: 오디오 스트림을 수신하는 역할(주로 스피커)</li><li><code>"source"</code>: 오디오 스트림을 송신하는 역할(음원 데이터 전달자)</li></ul> | 항상     |

### Remarks

* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 블루투스 기기 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

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

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [블루투스 기기에 대한 연결 요청 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [페어링된 블루투스 기기 삭제 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDelete)

## BtDisconnect directive {#BtDisconnect}

클라이언트가 연결된 블루투스 기기와 연결을 끊도록 지시합니다.

### Payload fields

* 연결된 모든 기기와 연결을 끊을 때

없음

* 연결할 기기를 지정할 때

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `address`     | string  | 연결 해제하려는 블루투스 기기의 장치 주소     | 항상     |
| `connected`   | boolean | 연결 해제하려는 블루투스 기기에 대한 연결 여부. <ul><li><code>true</code>: 연결된 상태</li><li><code>false</code>: 연결되어 있지 않은 상태</li></ul>      | 항상     |
| `name`        | string  | 연결 해제하려는 블루투스 기기의 이름         | 항상     |
| `role`        | string  | 해당 블루투스 기기와 연결 시 클라이언트의 역할.<ul><li><code>"sink"</code>: 오디오 스트림을 수신하는 역할(주로 스피커)</li><li><code>"source"</code>: 오디오 스트림을 송신하는 역할(음원 데이터 전달자)</li></ul> | 항상     |

### Remarks

* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 블루투스 기기 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

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

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [블루투스 기기 연결 해제 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDisconnect)
* [블루투스 기기에 대한 연결 요청 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [페어링된 블루투스 기기 삭제 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDelete)

## BtPlay directive {#BtPlay}

클라이언트가 연결된 블루투스 기기를 통해 오디오 콘텐츠를 재생하도록 지시합니다. 사용자가 "블루투스로 음악 재생해줘"와 같은 명령을 했을 때 이 지시 메시지가 전달됩니다. 클라이언트는 다른 기기와 블루투스로 연결될 때 오디오 스트림을 수신해서 출력하는 역할(`"sink"`)이나 오디오 스트림을 송신하는 역할(`"source"`)을 맡게되며, 클라이언트는 각 역할(`role`)에 따라 이 지시 메시지를 처리해야 합니다.

* 클라이언트의 역할이 `"sink"`이면: 연결된 블루투스 기기로부터 오디오 스트림을 수신하여 스피커로 음원을 출력합니다.
* 클라이언트의 역할이 `"source"`이면: 클라이언트에서 일시 중지했거나 이전에 재생했던 오디오 스트림을 연결된 블루투스 기기를 통해 재생합니다. 만약, 이전에 재생했던 음원이 없거나 특정 음원을 지정할 수 없는 경우라면 사용자가 "음악 들려줘"라는 발화로 요청한 것과 같은 이벤트 메시지나 제품 UI/UX에 맞는 오디오 콘텐츠 재생 요청을 CLOVA에 전송합니다.

### Payload fields

없음

### Remarks

* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 블루투스 기기 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

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

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)

## BtRequestForPINCode event {#BtRequestForPINCode}

외부 블루투스 기기가 PIN 코드 입력 요청을 할 때 해당 PIN 코드를 CLOVA에 전달합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceName`  | string  | 연결할 블루투스 기기의 이름. PIN 코드 입력 요청 화면에 출력됩니다. | 필수     |

### Remarks

* 연결하고자 하는 블루투스 기기가 PIN 코드를 요청할 때만 이 메시지를 보내야 합니다. 일반적으로 최초 연결 시에만 PIN 코드를 요청합니다.
* CLOVA가 이 이벤트 메시지를 받으면 [`DeviceControl.BtConnectByPINCode`](#BtConnectByPINCode) 지시 메시지를 통해 클라이언트에 PIN 코드를 전달합니다.

### Message example

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

### See also

* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtConnectByPINCode`](#BtConnectByPINCode)
* [`DeviceControl.BtRequestToCancelPINCodeInput`](#BtRequestToCancelPINCodeInput)
* [블루투스 기기에 대한 연결 요청 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)

## BtRequestToCancelPINCodeInput event {#BtRequestToCancelPINCodeInput}

PIN 코드 입력을 취소하도록 CLOVA에 요청합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

없음

### Remarks

특정 시간 동안 PIN 코드 입력이 없거나 블루투스 연결이 끊어지는 것과 같은 특수 상황에서 PIN 코드 입력 요청을 취소할 수 있습니다.
### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtRequestToCancelPINCodeInput",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
    },
    "payload": {}
  }
}
```

### See also

* [`DeviceControl.BtRequestForPINCode`](#BtRequestForPINCode)
* [블루투스 기기에 대한 연결 요청 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)

## BtRescan directive {#BtRescan}

클라이언트가 블루투스 기기를 재탐지(rescan)하도록 지시합니다. 주변 연결 가능한 블루투스 기기 목록을 보여주는 페어링 화면을 표시하거나 사용자가 목록의 갱신을 요청하면 CLOVA가 이 지시 메시지를 클라이언트에 전송합니다.

### Payload fields

없음

### Remarks

* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 블루투스 기기 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtRescan",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {}
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [블루투스 기기에 대한 연결 요청 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [페어링된 블루투스 기기 삭제 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDelete)

## BtStartPairing directive {#BtStartPairing}

클라이언트가 블루투스 페어링을 시작하도록 지시합니다.

### Payload fields

없음

### Remarks

* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 블루투스 기기 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

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

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [블루투스 기기 연결 해제 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDisconnect)
* [블루투스 기기에 대한 연결 요청 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)

## BtStopPairing directive {#BtStopPairing}

클라이언트가 블루투스 페어링을 중지하도록 지시합니다.

### Payload fields

없음

### Remarks

* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 블루투스 기기 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtStopPairing",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {}
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStartPairing`](#BtStopPairing)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [블루투스 기기 연결 해제 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDisconnect)
* [블루투스 기기에 대한 연결 요청 처리하기](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [클라이언트 기기 설정 활성화하기](/Develop/Guides/Handle_Device_Control.md#HandleClientFeatureToggle)

## Decrease directive {#Decrease}

클라이언트가 스피커 볼륨 또는 화면 밝기를 기본 단위만큼 줄이도록 지시하거나, TV 채널을 이전 채널로 변경하도록 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | 제어 대상.<ul><li><code>"channel"</code>: TV 채널</li><li><code>"screenbrightness"</code>: 화면 밝기</li><li><code>"volume"</code>: 스피커 볼륨</li></ul> | 항상     |
| `value`       | string  | 변경할 밝기나 볼륨의 크기 정보       | 조건부    |

### Remarks

* `value` 필드의 값이 없을 경우 크기 변경의 기본 단위는 클라이언트측에서 직접 결정하면 됩니다.
* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 스피커 볼륨 정보와 화면 밝기 정보를 CLOVA에 전달해야 합니다.
* 사용자가 기기가 표현할 수 있는 화면의 밝기나 볼륨의 범위를 벗어나는 값 변경 요청을 하더라도 CLOVA는 기기에 맞게 크기 정보를 조절하여 이 지시 메시지를 보냅니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.
* CLOVA는 보통 기기 제어에 대한 지시 메시지를 클라이언트에 전달할 때 음성 안내([`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지)를 함께 제공합니다. 다만, `target` 필드가 `"volume"`으로 설정된 것처럼 스피커 출력과 관계된 제어이면 [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지를 통해 안내 문구를 내려보내지 않습니다. 이는 사용자의 음악 감상과 같은 UX를 고려한 사항이며, 이때는 음성 안내 대신 클라이언트 기기의 조명이나 짧은 효과음 통해 볼륨이 조절되었음을 알리도록 구현해야 합니다.

### Message example

```json
// 크기 정보 없이 볼륨을 내려달라고 한 경우
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Decrease",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume"
    }
  }
}

// 특정 크기 만큼 볼륨을 내려달라고 한 경우
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Decrease",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume"
    }
  }
}

// 특정 크기 만큼 볼륨을 내려달라고 한 경우
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Decrease",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume",
      "value": "3"
    }
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.Increase`](#Increase)
* [`DeviceControl.SetValue`](#SetValue)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [클라이언트 볼륨 조정하기](/Develop/Guides/Handle_Device_Control.md#HandleDeviceVolume)

## ExpectReportState directive {#ExpectReportState}

기기의 현재 상태를 CLOVA에 보고합니다. 이 메시지를 받은 클라이언트는 즉시 [`DeviceControl.ReportState`](#ReportState) 이벤트 메시지를 사용하여 기기의 현재 상태를 보고해야 하며, 이후에는 `durationInSeconds` 필드에 지정된 시간 동안 `intervalInSeconds` 필드에 지정된 주기로 기기의 현재 상태를 보고해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `durationInSeconds` | number  | 보고 기간. 지정된 시간동안 기기의 현재 상태를 보고합니다. 단위는 초 단위이며, `0` 이상의 정수 값을 포함합니다. 이 필드가 없으면 최초 1 회만 보고를 수행합니다. | 조건부     |
| `intervalInSeconds` | number  | 보고 주기. 지정된 주기로 기기의 형태 상태를 보고합니다. 단위는 초 단위이며, `0` 이상의 정수 값을 포함합니다. 이 필드는 `durationInSeconds` 필드가 있을 때만 유효하며 없으면 함께 생략됩니다. | 조건부     |

### Remarks

* `DeviceControl.ExpectReportState` 지시 메시지는 다른 클라이언트에서 동기화를 위해 [`DeviceControl.RequestStateSynchronization`](#RequestStateSynchronization) 이벤트 메시지를 CLOVA에 전송했을 때 수신됩니다.
* 이 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "ExpectReportState",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "durationInSeconds": 600,
      "intervalInSeconds": 60
    }
  }
}
```

### See also

* [`DeviceControl.ReportState`](#ReportState)
* [`DeviceControl.RequestStateSynchronization`](#RequestStateSynchronization)

## Increase directive {#Increase}

클라이언트가 스피커 볼륨 또는 화면 밝기를 기본 단위만큼 높이도록 지시하거나, TV 채널을 다음 채널로 변경하도록 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | 제어 대상.<ul><li><code>"channel"</code>: TV 채널</li><li><code>"screenbrightness"</code>: 화면 밝기</li><li><code>"volume"</code>: 스피커 볼륨</li></ul> | 항상     |
| `value`       | string  | 변경할 밝기나 볼륨의 크기 정보       | 조건부    |

### Remarks

* `value` 필드의 값이 없을 경우 크기 변경의 기본 단위는 클라이언트측에서 직접 결정하면 됩니다.
* 사용자가 기기가 표현할 수 있는 화면의 밝기나 볼륨의 범위를 벗어나는 값 변경 요청을 하더라도 CLOVA는 기기에 맞게 크기 정보를 조절하여 이 지시 메시지를 보냅니다.
* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 스피커 볼륨 정보와 화면 밝기 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.
* CLOVA는 보통 기기 제어에 대한 지시 메시지를 클라이언트에 전달할 때 음성 안내([`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지)를 함께 제공합니다. 다만, `target` 필드가 `"volume"`으로 설정된 것처럼 스피커 출력과 관계된 제어이면 [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지를 통해 안내 문구를 내려보내지 않습니다. 이는 사용자의 음악 감상과 같은 UX를 고려한 사항이며, 이때는 음성 안내 대신 클라이언트 기기의 조명이나 짧은 효과음 통해 볼륨이 조절되었음을 알리도록 구현해야 합니다.

### Message example

```json
// 크기 정보 없이 볼륨을 올려달라고 한 경우
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

// 특정 크기 만큼 볼륨을 올려달라고 한 경우
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

// 특정 크기 만큼 볼륨을 올려달라고 한 경우
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Increase",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume",
      "value": "3"
    }
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.Decrease`](#Decrease)
* [`DeviceControl.SetValue`](#SetValue)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [클라이언트 볼륨 조정하기](/Develop/Guides/Handle_Device_Control.md#HandleDeviceVolume)

## LaunchApp directive {#LaunchApp}

**(Deprecated)** 클라이언트가 특정 앱을 실행하도록 지시합니다. 앱을 지정하는 값으로 앱의 custom URI scheme나 중계 페이지 URI 또는 앱 이름이 전달됩니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | 대상 앱에 대한 정보. 다음과 같은 타입의 앱 정보를 가질 수 있습니다.<ul><li>custom URI scheme: 대상 앱의 custom URI scheme(예: <code>"{{ book.ServiceEnv.OrientedServiceWithLowerCase }}searchapp://..."</code>)</li><li>중계 페이지 URI: 설치된 대상 앱이 있으면 해당 앱을 실행하는 중계 페이지 URI(예: <code>"http://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}app.{{ book.ServiceEnv.OrientedServiceWithLowerCase }}.com/..."</code>)</li><li>앱 이름: 사용자의 발화를 인식한 앱의 이름(예: <code>"{{ book.ServiceEnv.OrientedService }}앱"</code>)</li></ul> | 항상     |

### Remarks

* 앱을 실행할 수 없거나 앱 실행에 실패했다면 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "LaunchApp",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "{{ book.ServiceEnv.OrientedServiceWithLowerCase }}searchapp://..."
    }
  }
}
```

### See also

* [`DeviceControl.ActionFailed`](#ActionFailed)

## Open directive {#Open}

클라이언트가 특정 화면을 표시하도록 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | 표시할 화면. 현재 다음과 같은 값을 입력할 수 있습니다.<ul><li><code>"home"</code>: 홈 화면</li><li><code>"settings"</code>: 설정 화면</li></ul> | 항상   |

### Remarks

클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Open",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "15ecb9a6-e727-4d58-8ba1-42cc8b63d5c0"
    },
    "payload": {
      "target": "settings"
    }
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)

## OpenScreen directive {#OpenScreen}

**(Deprecated)** 클라이언트가 설정 화면을 열도록 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | 표시할 화면. 현재 설정 화면을 여는 값인 `"settings"`만 입력됩니다. | 항상     |

### Remarks

클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "OpenScreen",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "settings"
    }
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)

## RenderDeviceList directive {#RenderDeviceList}

클라이언트가 사용자 계정에 등록된 클라이언트 기기 목록을 표시하도록 지시합니다.

### Payload fields

| 필드 이름                       | 자료형    | 필드 설명                      | 포함 여부 |
|-------------------------------|---------|------------------------------|:---------:|
| `deviceList[]`                  | object array | 사용자 계정에 등록된 클라이언트 기기 목록 정보를 가지는 객체 배열 | 항상     |
| `deviceList[].availabilities[]` | object array | 기기의 특정 기능 지원 정보    | 항상 |
| `deviceList[].availabilities[].available` | bool | 기기의 특정 기능 지원 여부.<ul><li><code>true</code>: 지원</li><li><code>false</code>: 미지원</li></ul> | 항상 |
| `deviceList[].availabilities[].name` | string | 특정 기능의 이름<ul><li><code>"DELEGATION"</code>: 명령어 도우미. 뮤직탭 클릭하여 앱에서 재생하도록 하는 것과 같이 명령을 위임 처리할 수 있도록 지원하는 기능입니다.</li><li><code>"AUDIOPLAYER_CONTROL"</code>: 앱과 기기간 오디오 재생 정보 조회와 재생 제어 지원</li><li><code>"AUDIOPLAYER_EXPOSE": 앱과 기기간 오디오 재생 시 미니 플레이어 지원</code></li><li><code>"SPEAKER_REGISTRATION": 화자 등록 지원</code></li> | 항상 |
| `deviceList[].bindTime`       | string  | 기기 등록 시간                     | 항상 |
| `deviceList[].clientId`       | string  | 기기의 클라이언트 ID               | 항상 |
| `deviceList[].clientName`     | string  | 기기의 이름                        | 항상 |
| `deviceList[].companionAppUrl`| string  | 연동앱 URI                         | 조건부 |
| `deviceList[].configuration`  | string  | 사용자 설정 정보를 가지는 객체              | 조건부 |
| `deviceList[].configuration.DeviceInfoDefinedName` | string| 사용자가 설정한 기기 이름 | 조건부 |
| `deviceList[].deviceId`       | string  | 기기 ID                       | 항상 |
| `deviceList[].deviceImageUrl` | string  | 기기 이미지 URI               | 조건부 |
| `deviceList[].deviceName`     | string  | 기기의 이름                   | 항상 |
{% if book.DocMeta.TargetReaderType == "Internal" -%}
| `deviceList[].is3rdParty`     | boolean | 써드 파티 여부                 | 항상 |
{% endif -%}
| `deviceList[].manufacturerName`| string | 제조사 이름                   | 조건부 |
| `deviceList[].modelId`        | string  | 모델 ID                       | 항상 |
| `deviceList[].userGuideUrl`   | string  | 사용자 설명서 URI             | 조건부 |
| `deviceList[].websiteUrl`     | string  | 제품 공식 페이지 URI          | 조건부 |

### Remarks

클라이언트 기기의 상태 정보를 [`DeviceControl.RequestStateSynchronization`](#RequestStateSynchronization) 이벤트 메시지를 이용하여 각 기기별로 요청하려면 이 지시 메시지의 `deviceList[].deviceId` 필드를 이용해야 합니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "RenderDeviceList",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "deviceList": [
        {
          "availabilities": [
            {
              "name": "DELEGATION",
              "available": true
            },
            {
              "name": "AUDIOPLAYER_CONTROL",
              "available": true
            }
          ],
          "clientName": "FRIENDS_BROWN",
          "clientId": "dade0b2bc39a4d7bb73138600558b3b8",
          "deviceId": "c789bcaf84903692cafe73995c9a719d2f713fd16f7fdc3b51088a0723b7f6e9",
          "deviceName": "CLOVA-BROWN-9D2",
          "deviceImageUrl": null,
          "modelId": "NL-S100KR",
          "manufacturerName": null,
          "userGuideUrl": null,
          "websiteUrl": null,
          "companionAppUrl": null,
          "configuration": {
            "DeviceInfoDefinedName" : "짱구"
          }
        },
        {
          "availabilities": [],
          "clientId": "7JWI64WVIOy5nOq1rOuTpA",
          "clientName": "LGE_ARCH",
          "companionAppUrl": "clova://companion-app?android=com.lgeha.nuts&ios=lgsmartthinq%3A%2F%2F&ios_appstore=https%3A%2F%2Fitunes.apple.com%2Fkr%2Fapp%2Flg-smartthinq%2Fid993504342",
          "deviceId": "bdddd455671e5642462c580914a1197a",
          "deviceImageUrl": "https://ssl.pstatic.net/static/clova/service/devices/lge_arch/170917.jpg",
          "deviceName": "LG 스마트 씽큐 허브-36C",
          "modelId": "KRFR01",
          "manufacturerName": "LG전자",
          "userGuideUrl": "http://www.lge.co.kr/lgekor/product/accessory/smart-life/productDetail.do?cateId=8200&prdId=EPRD.313033",
          "configuration": {
            "DeviceInfoDefinedName" : ""
          }
        },
        {
          "availabilities": [],
          "clientId": "7JWI64WVIOy5nOq1rOuTpA",
          "clientName": "WAVE",
          "companionAppUrl": null,
          "deviceId": "01c0f228ae77eea8c57b7ed530d40cf72abeae945c",
          "deviceImageUrl": null,
          "deviceName": "WAVE",
          "modelId": "WAVE",
          "manufacturerName": null,
          "userGuideUrl": null,
          "configuration": {
            "DeviceInfoDefinedName" : ""
          }
        }
      ]
    }
  }
}
```

### See also

* [`DeviceControl.RequestDeviceList`](#RequestDeviceList)
* [`DeviceControl.RequestStateSynchronization`](#RequestStateSynchronization)
* [기기 상태 정보 공유하기](/Develop/Guides/Handle_Device_Control.md#HandleDeviceStateReport)

## ReportState event {#ReportState}

기기의 현재 상태를 CLOVA에 보고합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

없음

### Remarks

* CLOVA로부터 [`DeviceControl.ExpectReportState`](#ExpectReportState) 지시 메시지를 받았다면 `DeviceControl.ReportState` 이벤트 메시지를 사용하여 현재 상태를 보고해야 합니다.
* 이 이벤트 메시지를 통해 보고된 상태 정보는 [`DeviceControl.SynchronizeState`](#SynchronizeState) 지시 메시지 통해 사용자 계정에 등록된 모든 클라이언트에 보내집니다.
* 이 이벤트 메시지에 대해 응답으로 반환되는 지시 메시지는 없으며, HTTP 응답이 `204 No Content`로 반환됩니다.

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "ReportState",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {}
  }
}
```

### See also

* [`DeviceControl.ExpectReportState`](#ExpectReportState)
* [`DeviceControl.SynchronizeState`](#SynchronizeState)
* [기기 상태 정보 공유하기](/Develop/Guides/Handle_Device_Control.md#HandleDeviceStateReport)

## RequestDeviceList event {#RequestDeviceList}

사용자 계정에 등록된 클라이언트 기기 목록과 각 클라이언트의 식별 정보를 CLOVA에 요청합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

없음

### Remarks

이 이벤트 메시지에 대해 응답으로 [`DeviceControl.RenderDeviceList`](#RenderDeviceList) 지시 메시지가 반환됩니다.

### Message example

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "RequestDeviceList",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {}
  }
}
```

### See also

* [`DeviceControl.RenderDeviceList`](#RenderDeviceList)
* [기기 상태 정보 공유하기](/Develop/Guides/Handle_Device_Control.md#HandleDeviceStateReport)

## RequestStateSynchronization event {#RequestStateSynchronization}

사용자의 계정에 등록된 다른 클라이언트 기기의 현재 상태를 CLOVA에 요청합니다. CLOVA가 이 이벤트 메시지를 받으면, 사용자의 계정에 등록된 모든 또는 특정 클라이언트에 [`DeviceControl.ExpectReportState`](#ExpectReportState) 지시 메시지를 전송합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 대상 클라이언트 기기의 ID. 클라이언트 기기의 MAC 주소나 생성한 UUID 형태의 값입니다. 이 필드가 생략되면 CLOVA는 사용자 계정에 등록된 모든 클라이언트 기기에 [`DeviceControl.ExpectReportState`](#ExpectReportState) 지시 메시지를 전송합니다. <div class="note"><p><strong>Note!</strong></p><p>이 기능을 사용하려면 사전 협의가 필요합니다. 제휴 담당자에게 연락바랍니다.</p></div> | 선택     |

### Remarks

* 특정 기기의 상태 정보만 업데이트하려면 [`DeviceControl.RenderDeviceList`](#RenderDeviceList) 지시 메세지의 `deviceList[].deviceId` 필드를 이용해야 합니다.
* 이 이벤트 메시지에 대한 결과로 추후 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 [`DeviceControl.SynchronizeState`](#SynchronizeState) 지시 메시지를 받게 됩니다.
* 이 이벤트 메시지에 대해 응답으로 반환되는 지시 메시지는 없으며, HTTP 응답이 `204 No Content`로 반환됩니다.

### Message example

```json
// 예제 1: 전체 기기의 상태 요청
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "RequestStateSynchronization",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {}
  }
}

// 예제 2: 특정 기기의 상태 요청
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "RequestStateSynchronization",
      "messageId": "aa8e5c92-c5cb-46ac-af09-ff3da47e1c40"
    },
    "payload": {
      "deviceId":""
    }
  }
}
```

### See also

* [`DeviceControl.ExpectReportState`](#ExpectReportState)
* [`DeviceControl.RenderDeviceList`](#RenderDeviceList)
* [`DeviceControl.SynchronizeState`](#SynchronizeState)

## SetValue directive {#SetValue}

클라이언트가 스피커 볼륨 또는 화면 밝기를 지정한 값으로 설정하거나 특정 TV 채널로 변경하도록 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | 제어 대상.<ul><li><code>"channel"</code>: TV 채널</li><li><code>"screenbrightness"</code>: 화면 밝기</li><li><code>"volume"</code>: 스피커 볼륨</li></ul> | 항상     |
| `value`       | string  | 변경할 값 또는 TV 채널 번호나 이름        | 항상     |

### Remarks

* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 스피커 볼륨 정보와 화면 밝기 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.
* CLOVA는 보통 기기 제어에 대한 지시 메시지를 클라이언트에 전달할 때 음성 안내([`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지)를 함께 제공합니다. 다만, `target` 필드가 `"volume"`으로 설정된 것처럼 스피커 출력과 관계된 제어이면 [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) 지시 메시지를 통해 안내 문구를 내려보내지 않습니다. 이는 사용자의 음악 감상과 같은 UX를 고려한 사항이며, 이때는 음성 안내 대신 클라이언트 기기의 조명이나 짧은 효과음 통해 볼륨이 조절되었음을 알리도록 구현해야 합니다.

### Message example

```json
// 볼륨 제어
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "SetValue",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume",
      "value": "30"
    }
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.Decrease`](#Decrease)
* [`DeviceControl.Increase`](#Increase)
* [`DeviceControl.SetValue`](#SetValue)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [클라이언트 볼륨 조정하기](/Develop/Guides/Handle_Device_Control.md#HandleDeviceVolume)

## SynchronizeState directive {#SynchronizeState}

클라이언트가 사용자 계정에 등록된 또 다른 클라이언트 기기의 상태 정보를 업데이트하도록 지시합니다. 사용자는 같은 사용자 계정으로 동시에 여러 클라이언트를 사용할 수 있습니다. 예를 들면, 클라이언트의 타입이 앱일 수도 있고 Wave와 같이 CLOVA 전용 기기인 스피커일 수도 있습니다. 앱 타입의 클라이언트는 스피커 타입의 다른 클라이언트를 제어할 수 있으며, 이때 다른 클라이언트를 제어한 결과를 `DeviceControl.SynchronizeState` 지시 메시지로 받을 수 있고, 이를 이용하여 변경된 클라이언트의 상태를 업데이트할 수 있습니다.

클라이언트는 다음과 같은 상황에 이 지시 메시지를 받을 수 있습니다.

* CLOVA가 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 수신하면 사용자 계정에 등록된 모든 클라이언트에 `DeviceControl.SynchronizeState` 지시 메시지를 이용하여 특정 클라이언트 기기의 변경된 상태 정보를 전달합니다.
* CLOVA가 [`DeviceControl.ReportState`](#ReportState) 이벤트 메시지를 수신하면 사용자 계정에 등록된 모든 클라이언트에 `DeviceControl.SynchronizeState` 지시 메시지를 이용하여 특정 클라이언트 기기의 현재 상태 정보를 전달합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 상태가 업데이트된 클라이언트 기기의 ID. | 항상     |
| `deviceState` | [Device.DeviceState](/Develop/References/Context_Objects.md#DeviceState) object | 기기 상태 업데이트 정보가 담긴 객체. 이 필드가 생략되었다면, `deviceId` 필드의 기기 ID 값에 해당하는 클라이언트 기기가 오프라인 상태임을 의미합니다.      | 조건부  |

### Remarks

`DeviceControl.SynchronizeState` 지시 메시지는 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 사용자 계정에 등록된 클라이언트 전체에 브로드캐스팅되며, [대화 ID(`dialogRequestId`)](/Develop/Guides/Manage_Dialog_ID_And_Handle_Tasks.md#HandleDirectivesByDialogID)를 가지지 않습니다.

### Message example

{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "SynchronizeState",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {
      "deviceId": "90de78d7-0735-43a8-8bdc-acc3c8bc4a80",
      "deviceState": {{Device.DeviceState}}
    }
  }
}
```
{% endraw %}

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.ReportState`](#ReportState)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)

## TurnOff directive {#TurnOff}

클라이언트가 지정한 기능이나 모드를 끄거나 비활성화하도록 지시합니다. 예를 들면, 이 지시 메시지를 통해 클라이언트 기기의 블루투스를 끄도록 할 수 있습니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | 제어 대상.<ul><li><code>"airplane"</code>: 비행기 모드</li><li><code>"bluetooth"</code>: 블루투스</li><li><code>"cellular"</code>: 모바일 통신</li><li><code>"energysave"</code>: 에너지 절약 모드</li><li><code>"flashlight"</code>: 플래시 조명</li><li><code>"gps"</code>: GPS</li><li><code>"power"</code>: 전원 상태</li><li><code>"ring"</code>: 벨소리 모드</li><li><code>"silent"</code>: 무음 모드</li><li><code>"vibrate"</code>: 진동 모드</li><li><code>"wifi"</code>: 무선랜</li></ul> | 항상     |

### Remarks

* 일부 제어 대상을 끄거나 비활성화할 때 클라이언트 기기의 정책이나 상황에 맞춰 제어를 수행해야 합니다. 예를 들면, 벨소리 모드를 비활성화했을 때 진동 모드로 진입할지 음소거 모드로 진입할지는 클라이언트 기기에 따라 구현을 달리할 수 있습니다.
* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 기능이나 모드의 상태 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p><code>target</code> 필드의 값이 <code>"power"</code>이면 클라이언트 기기의 전원을 꺼야 합니다.</p>
</div>

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "TurnOff",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "bluetooth"
    }
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.TurnOn`](#TurnOn)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [클라이언트 기기 설정 활성화하기](/Develop/Guides/Handle_Device_Control.md#HandleClientFeatureToggle)

## TurnOn directive {#TurnOn}

클라이언트가 지정한 기능이나 모드를 켜거나 활성화하도록 지시합니다. 예를 들면, 이 지시 메시지를 통해 클라이언트 기기의 블루투스를 켜도록 할 수 있습니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | 제어 대상.<ul><li><code>"airplane"</code>: 비행기 모드</li><li><code>"bluetooth"</code>: 블루투스</li><li><code>"cellular"</code>: 모바일 통신</li><li><code>"energysave"</code>: 에너지 절약 모드</li><li><code>"flashlight"</code>: 플래시 조명</li><li><code>"gps"</code>: GPS</li><li><code>"power"</code>: 전원 상태</li><li><code>"ring"</code>: 벨소리 모드</li><li><code>"silent"</code>: 무음 모드</li><li><code>"vibrate"</code>: 진동 모드</li><li><code>"wifi"</code>: 무선랜</li></ul> | 항상     |

### Remarks

* 클라이언트는 맥락 정보인 [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) 객체를 이용해 수시로 기능이나 모드의 상태 정보를 CLOVA에 전달해야 합니다.
* 클라이언트는 이 지시 메시지에 해당하는 내용을 처리한 후 [`DeviceControl.ActionExecuted`](#ActionExecuted) 또는 [`DeviceControl.ActionFailed`](#ActionFailed) 이벤트 메시지를 이용하여 결과를 CLOVA에 보고해야 합니다.

### Message example

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
      "target": "bluetooth"
    }
  }
}
```

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.TurnOn`](#TurnOn)
* [처리 결과 보고하기](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [클라이언트 기기 설정 활성화하기](/Develop/Guides/Handle_Device_Control.md#HandleClientFeatureToggle)
