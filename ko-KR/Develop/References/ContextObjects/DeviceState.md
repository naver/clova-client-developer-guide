## Device.DeviceState {#DeviceState}

`Device.DeviceState`는 클라이언트의 기기의 상태 정보를 CLOVA에 보고할 때 사용하는 메시지 포맷입니다.

### Object structure

{% raw %}
```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    "airplane": {{AirplaneInfoObject}},
    "battery": {{BatteryInfoObject}},
    "bluetooth": {{BluetoothInfoObject}},
    "cellular": {{CellularInfoObject}},
    "channel": {{ChannelInfoObject}},
    "energySavingMode": {{EnergySavingModeInfoObject}},
    "flashLight" {{FlashLightInfoObject}},
    "gps": {{GPSInfoObject}},
    "localTime": {{string}},
    "power": {{PowerInfoObject}},
    "screenAutoBrightness": {{ScreenAutoBrightnessInfoObject}},
    "screenBrightness": {{ScreenBrightnessInfoObject}},
    "soundMode": {{SoundModeInfoObject}},
    "soundOutput": {{SoundOutputInfoObject}},
    "volume": {{VolumeInfoObject}},
    "wifi": {{WifiInfoObject}}
  }
}
```
{% endraw %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `airplane`        | [AirplaneInfoObject](#AirplaneInfoObject)               | 클라이언트 기기의 비행기 모드 설정 정보를 보고할 때 사용하는 객체      | 선택 |
| `battery`         | [BatteryInfoObject](#BatteryInfoObject)                 | 클라이언트 기기의 배터리 정보를 보고할 때 사용하는 객체              | 선택 |
| `bluetooth`       | [BluetoothInfoObject](#BluetoothInfoObject)             | 클라이언트 기기의 블루투스 활성화 상태 및 블루투스 기기 연결 상태 정보를 보고할 때 사용하는 객체       | 선택 |
| `cellular`        | [CellularInfoObject](#CellularInfoObject)               | 클라이언트 기기의 모바일 통신 활성화 상태 정보를 보고할 때 사용하는 객체 | 선택 |
| `channel`         | [ChannelInfoObject](#ChannelInfoObject)                 | 클라이언트 기기의 TV 채널 설정 정보를 보고할 때 사용하는 객체         | 선택 |
| `energySavingMode` | [EnergySavingModeInfoObject](#EnergySavingModeInfoObject) | 클라이언트 기기의 에너지 절약 모드 정보를 보고할 때 사용하는 객체     | 선택 |
| `flashLight`      | [FlashLightInfoObject](#FlashLightInfoObject)           | 클라이언트 기기의 플래시 조명 설정 정보를 보고할 때 사용하는 객체       | 선택 |
| `gps`             | [GPSInfoObject](#GPSInfoObject)                         | 클라이언트 기기의 GPS 설정 정보를 보고할 때 사용하는 객체            | 선택 |
| `localTime`       | string | 클라이언트 기기에 설정된 현지 시간(`"YYYY-MM-DDThh:mm:ss±hh:mm"`, <a href="https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations" target="_blank">ISO 8601 Combined date and time representations</a> )              | 선택 |
| `power`           | [PowerInfoObject](#PowerInfoObject)                     | 클라이언트 기기의 전원 상태 정보를 보고할 때 사용하는 객체            | 선택 |
| `screenAutoBrightness` | [ScreenAutoBrightnessInfoObject](#ScreenAutoBrightnessInfoObject) | 클라이언트 기기의 화면 자동 밝기 설정 정보를 보고할 때 사용하는 객체         | 선택 |
| `screenBrightness` | [ScreenBrightnessInfoObject](#ScreenBrightnessInfoObject) | 클라이언트 기기의 화면 밝기 정보를 보고할 때 사용하는 객체         | 선택 |
| `soundMode`       | [SoundModeInfoObject](#SoundModeInfoObject)             | 클라이언트 기기의 소리 재생 모드 정보를 보고할 때 사용하는 객체           | 선택 |
| `soundOutput`     | [SoundOutputInfoObject](#SoundOutputInfoObject)         | 클라이언트 기기의 소리 출력을 위해 사용되고 있는 재생 장치나 방식에 대한 정보를 보고할 때 사용하는 객체 | 선택 |
| `volume`          | [VolumeInfoObject](#VolumeInfoObject)                   | 클라이언트 기기의 스피커 볼륨 정보를 보고할 때 사용하는 객체           | 선택 |
| `wifi`            | [WifiInfoObject](#WifiInfoObject)                       | 클라이언트 기기의 무선랜(Wi-Fi) 기능 활성화 상태와 무선랜 연결 정보를 보고할 때 사용하는 객체    | 선택 |

### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    "localTime": "2017-04-06T13:34:15.074361+08:28",
    "bluetooth": {
        "actions": [
            "BtConnect",
            "BtDisconnect",
            "BtStartPairing",
            "BtStopPairing",
            "TurnOff",
            "TurnOn"
        ],
        "btlist": [
            {
                "name": "My Phone",
                "address": "44:00:10:f1:1f:f5",
                "connected": false
            },
            {
                "name": "My Speaker",
                "address": "29:01:11:1f:12:89",
                "connected": true
            }
        ],
        "state": "on"
    },
    "wifi": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "networks": [
          {
            "name": "home_wlan",
            "connected": true
          },
          {
            "name": "guest_wlan",
            "connected": false
          }
        ],
        "state": "on"
    },
    "battery": {
        "actions": [],
        "value": 99,
        "charging": true
    },
    "flashLight": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "off"
    }
  }
}
```

### AirplaneInfoObject {#AirplaneInfoObject}

클라이언트 기기의 비행기 모드 설정 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`     | string array | 비행기 모드와 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다.<ul><li>"TurnOff"</li><li>"TurnOn"</li></ul> | 필수 |
| `state`         | string | 비행기 모드 설정 상태.<ul><li><code>"off"</code>: 꺼짐</li><li><code>"on"</code>: 켜짐</li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "airplane": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "on"
    },
    ...
  }
}
```

### BatteryInfoObject {#BatteryInfoObject}

클라이언트 기기의 배터리 상태 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`     | string array | 배터리와 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 현재 지원하는 동작이 없습니다. | 필수 |
| `charging`      | boolean | 충전 중인지 여부.<ul><li><code>true</code>: 충전 중인 상태</li><li><code>false</code>: 충전 중이지 않은 상태</li></ul> | 필수 |
| `value`         | number | 배터리 잔량. 단위는 퍼센트(%)이며, `0`에서 `100` 사이의 정수 값을 입력해야 합니다. | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "battery": {
        "actions": [],
        "value": 98,
        "charging": false
    },
    ...
  }
}
```

### BluetoothInfoObject {#BluetoothInfoObject}

클라이언트 기기의 블루투스 활성화 상태 및 블루투스 기기 연결 상태 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 블루투스 연결과 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li><li><code>"BtConnect"</code></li><li><code>"BtConnectByPINCode"</code></li><li><code>"BtDisconnect"</code></li><li><code>"BtStartPairing"</code></li><li><code>"BtStopPairing"</code></li></ul> | 필수 |
| `btlist[]`              | object array | 페이링된 적이 있는 블루투스 기기 정보를 가지는 객체 배열         | 필수 |
| `btlist[].address`      | string       | 블루투스 기기의 장치 주소                  | 필수 |
| `btlist[].availableServiceClassList` | string array | 블루투스 서비스 클래스 이름을 가지는 배열. `"AudioSource"`, `"AudioSink"`와 같은 값을 가질 수 있습니다. <div class="tip"><p><strong>Tip!</strong></p><p>블루투스 서비스 클래스 이름에 대한 자세한 설명은 <a target="_blank" href="https://www.bluetooth.com/specifications/assigned-numbers/service-discovery/">Bluetooth Service Discovery 문서</a>를 참고합니다. CLOVA가 현재 사용하고 있는 프로파일은 A2DP, AVRCP 입니다.</p></div>           | 필수 |
| `btlist[].connected`    | boolean      | 블루투스 기기와의 연결 여부. <ul><li><code>true</code>: 연결된 상태</li><li><code>false</code>: 연결되어 있지 않은 상태</li></ul> | 필수 |
| `btlist[].name`         | string       | 블루투스 기기의 이름                      | 필수 |
| `btlist[].role`         | string       | 해당 블루투스 기기와 연결 시 클라이언트의 역할.<ul><li><code>"sink"</code>: 오디오 스트림을 수신하는 역할(주로 스피커)</li><li><code>"source"</code>: 오디오 스트림을 송신하는 역할(음원 데이터 전달자)</li></ul> | 필수 |
| `connecting`            | string       | 블루투스 기기와 연결 중인지 여부. <ul><li><code>"on"</code>: 연결 중</li><li><code>"off"</code>: 연결 중이지 않음</li></ul> | 필수 |
| `pairing`               | string       | 블루투스 페어링 모드 켜짐 여부. <ul><li><code>"on"</code>: 페어링 모드 켜짐</li><li><code>"off"</code>: 페어링 모드 꺼짐</li></ul> | 필수 |
| `playerInfo`            | object       | 블루투스 연결을 통해 재생되고 있는 음악의 정보를 가지는 객체  | 선택 |
| `playerInfo.albumTitle` | string       | 블루투스로 재생되고 있는 음악의 앨범 제목                 | 선택 |
| `playerInfo.artistName` | string       | 블루투스로 재생되고 있는 음악의 아티스트 이름                 | 선택 |
| `playerInfo.state`      | string       | 블루투스로 재생되고 있는 음악의 재생 상태. <ul><li><code>"paused"</code>: 재생 일시 정지 상태</li><li><code>"playing"</code>: 재생 중인 상태</li><li><code>"stopped"</code>: 재생 중지 상태</li></ul>                  | 선택 |
| `playerInfo.trackTitle` | string       | 블루투스로 재생되고 있는 음악의 이름                     | 선택 |
| `scanlist[]`            | object array | 스캔된 블루투스 기기 정보를 가지는 객체 배열   | 필수 |
| `scanlist[].address`    | string       | 블루투스 기기의 장치 주소                  | 필수 |
| `scanlist[].name`       | string       | 블루투스 기기의 이름                      | 필수 |
| `scanlist[].role`       | string       | 해당 블루투스 기기와 연결 시 클라이언트의 역할.<ul><li><code>"sink"</code>: 오디오 스트림을 수신하는 역할(주로 스피커)</li><li><code>"source"</code>: 오디오 스트림을 송신하는 역할(음원 데이터 전달자)</li></ul> | 필수 |
| `scanning`              | string       | 블루투스 스캐닝 모드 켜짐 여부. <ul><li><code>"on"</code>: 스캐닝 모드 켜짐</li><li><code>"off"</code>: 스캐닝 모드 꺼짐</li></ul> | 필수 |
| `state`                 | string       | 블루투스 활성화 상태 <ul><li><code>"off"</code>: 꺼짐</li><li><code>"on"</code>: 켜짐</li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "bluetooth": {
      "actions": [
        "BtConnect",
        "BtDisconnect",
        "BtStartPairing",
        "BtStopPairing",
        "TurnOff",
        "TurnOn"
      ],
      "btlist": [
        {
          "name": "My Phone",
          "address": "44:00:10:f1:1f:f5",
          "connected": false,
          "role": "source"
        },
        {
          "name": "My Speaker",
          "address": "29:01:11:1f:12:89",
          "connected": true,
          "role": "sink"
        }
      ],
      "scanlist": [
        {
          "name": "A New Phone",
          "address": "04:11:10:01:1a:45",
          "role": "sink"
        },
        {
          "name": "A New Speaker",
          "address": "19:02:11:6f:3f:74",
          "role": "source"
        }
      ],
      "state": "on",
      "pairing": "on",
      "scanning": "on",
      "connecting": "off"
    },
    ...
  }
}
```

### CellularInfoObject {#CellularInfoObject}

클라이언트 기기의 모바일 통신 활성화 상태 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 모바일 데이터 통신과 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | 필수 |
| `state`              | string       | 모바일 데이터 통신 활성화 여부. <ul><li><code>"off"</code>: 꺼짐</li><li><code>"on"</code>: 켜짐</li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "cellular": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "off"
    },
    ...
  }
}
```

### ChannelInfoObject {#ChannelInfoObject}

클라이언트 기기의 TV 채널 설정 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`     | string array | TV 채널 설정과 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다.<ul><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"SetValue"</code></li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "channel": {
      "actions": [
        "Decrease",
        "Increase",
        "SetValue"
      ]
    },
    ...
  }
}
```

### EnergySavingModeInfoObject {#EnergySavingModeInfoObject}

클라이언트 기기의 에너지 절약 모드 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 절전 모드와 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | 필수 |
| `state`              | string       | 절전 모드 설정 상태. <ul><li><code>"off"</code>: 꺼짐</li><li><code>"on"</code>: 켜짐</li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "energySavingMode": {
      "actions": [
        "TurnOff",
        "TurnOn"
      ],
      "state": "off"
    },
    ...
  }
}
```


### FlashLightInfoObject {#FlashLightInfoObject}

클라이언트 기기의 플래시 조명 설정 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 플래시 조명과 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | 필수 |
| `state`              | string       | 플래시 조명의 현재 상태. <ul><li><code>"off"</code>: 꺼짐</li><li><code>"on"</code>: 켜짐</li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "flashLight": {
      "actions": [
        "TurnOff",
        "TurnOn"
      ],
      "state": "off"
    },
    ...
  }
}
```

### GPSInfoObject {#GPSInfoObject}

클라이언트 기기의 GPS 상태 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | GPS와 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | 필수 |
| `state`              | string       | GPS의 현재 상태. <ul><li><code>"off"</code>: 꺼짐</li><li><code>"on"</code>: 켜짐</li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "gps": {
      "actions": [
        "TurnOff",
        "TurnOn"
      ],
      "state": "off"
    },
    ...
  }
}
```

### PowerInfoObject {#PowerInfoObject}

클라이언트 기기의 전원 상태 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 전원 상태와 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | 필수 |
| `state`              | string       | 전원 상태. <ul><li><code>"active"</code>: 클라이언트 기기 켜짐</li><li><code>"idle"</code>: 클라이언트 기기 꺼짐</li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "power": {
      "actions": [
        "TurnOff",
        "TurnOn"
      ],
      "state": "active"
    },
    ...
  }
}
```

### ScreenAutoBrightnessInfoObject {#ScreenAutoBrightnessInfoObject}
클라이언트 기기의 화면 자동 밝기 설정 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 화면 자동 밝기와 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | 필수 |
| `state`              | string       | 화면 자동 밝기 설정 상태    <ul><li><code>"off"</code>: 꺼짐</li><li><code>"on"</code>: 켜짐</li></ul>               | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "screenAutoBrightness": {
        "actions": [
            "TurnOn",
            "TurnOff"
        ],
        "state": "on"
    },
    ...
  }
}
```

### ScreenBrightnessInfoObject {#ScreenBrightnessInfoObject}

클라이언트 기기의 화면 밝기 상태 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 화면 밝기와 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"SetValue"</code></li></ul> | 필수 |
| `max`                | number       | 클라이언트 기기 화면에 설정할 수 있는 밝기의 최대치. 최소치보다 큰 정수 값을 입력합니다. `100`을 권장합니다.    | 필수 |
| `min`                | number       | 클라이언트 기기 화면에 설정할 수 있는 밝기의 최소치. 최대치보다 작은 정수 값을 입력합니다. `0`를 권장합니다.    | 필수 |
| `value`              | number       | 현재 클라이언트 기기의 화면 밝기. 최대치 이하 최소치 이상의 정수 값을 입력합니다.  | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "screenBrightness": {
      "actions": [
        "Decrease",
        "Increase",
        "SetValue"
      ],
      "min": 0,
      "max": 100,
      "value": 70
    },
    ...
  }
}
```

### SoundModeInfoObject {#SoundModeInfoObject}

클라이언트 기기의 소리 재생 모드 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 사운드 모드와 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul>  | 필수 |
| `state`              | string       | 사운드 모드 설정 상태. <ul><li><code>"ring"</code>: 벨소리 모드</li><li><code>"silent"</code>: 무음 모드</li><li><code>"vibrate"</code>: 진동 모드</li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "soundMode": {
      "actions": [
        "TurnOff",
        "TurnOn"
      ],
      "state": "vibrate"
    },
    ...
  }
}
```

### SoundOutputInfoObject {#SoundOutputInfoObject}

클라이언트 기기의 소리 출력을 위해 사용되고 있는 재생 장치나 방식에 대한 정보를 보고할 때 사용하는 객체

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `type`        | string  | 소리 출력을 위해 사용되고 있는 재생 장치나 방식. <ul><li><code>"builtin"</code>: 내장 스피커</li><li><code>"aux"</code>: 유선 단자</li><li><code>"bluetooth"</code>: 블루투스</li><li><code>"airplay"</code>: <a target="_blank" href="https://support.apple.com/en-gb/HT204289">AirPlay</a></li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "soundOutput": {
      "type": "builtin"
    },
    ...
  }
}
```

### VolumeInfoObject {#VolumeInfoObject}

클라이언트 기기의 스피커 볼륨 크기 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 스피커 볼륨 크기와 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"SetValue"</code></li></ul> | 필수 |
| `max`                | number       | 클라이언트 기기 스피커에 설정할 수 있는 볼륨의 최대치. 최소치보다 큰 정수 값을 입력합니다. `10`을 권장합니다.    | 필수 |
| `min`                | number       | 클라이언트 기기 스피커에 설정할 수 있는 볼륨의 최소치. 최대치보다 작은 정수 값을 입력합니다. `0`을 권장합니다.    | 필수 |
| `value`              | number       | 클라이언트 기기의 현재 스피커 볼륨 크기. 최대치 이하 최소치 이상의 정수 값을 입력합니다.               | 필수 |
| `warning`            | number       | 클라이언트 기기 스피커에 특정 수치 이상 설정하려고 할 때 경고할 값. 이 필드의 값이 `8`이고, 사용자가 `8` 이상의 값을 볼륨으로 설정하게 되면 사용자에게 "볼륨 10은 무척 큰 소리에요. 변경을 원하시나요?"라고 되묻습니다. 최대치 이하 최소치 이상의 정수 값을 입력합니다. | 선택 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "volume": {
      "actions": [
        "Decrease",
        "Increase",
        "SetValue"
      ],
      "min": 0,
      "max": 10,
      "warning": 8,
      "value": 6
    },
    ...
  }
}
```

### WifiInfoObject {#WifiInfoObject}

클라이언트 기기의 무선랜(Wi-Fi) 기능 활성화 상태와 무선랜 연결 정보를 보고할 때 사용하는 객체입니다.

#### Object fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`            | string array | 무선랜과 관련하여 수행할 수 있는 [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) API 목록. 다음 동작 목록 중 클라이언트 기기가 실제로 수행할 수 있는 동작을 입력합니다. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | 필수 |
| `networks[]`           | object array | 검색된 무선랜 정보를 가지는 객체 배열 | 필수 |
| `networks[].connected` | boolean      | 무선랜 연결 여부. <ul><li><code>true</code>: 연결된 상태</li><li><code>false</code>: 연결되어 있지 않은 상태</li></ul> | 필수 |
| `networks[].name`      | string       | 무선랜 이름(SSID)               | 필수 |
| `state`                | string       | 무선랜 기능 활성화 상태. <ul><li><code>"off"</code>: 꺼짐</li><li><code>"on"</code>: 켜짐</li></ul> | 필수 |

#### Object example

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "wifi": {
      "actions": [
        "TurnOff",
        "TurnOn"
      ],
      "networks": [
        {
          "name": "home_wlan",
          "connected": true
        },
        {
          "name": "guest_wlan",
          "connected": false
        }
      ],
      "state": "on"
    },
    ...
  }
}
```

### See also

* [`DeviceControl` API](/Develop/References/MessageInterfaces/DeviceControl.md)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
