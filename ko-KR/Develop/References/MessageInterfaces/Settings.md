# Settings

Settings 인터페이스는 CLOVA와 클라이언트 사이에서 클라이언트의 설정 정보를 업데이트하거나 동기화해야 할 때 사용되는 네임스페이스입니다. Settins가 제공하는 이벤트 메시지와 지시 메시지는 다음과 같습니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                 |
|------------------|-----------|-------------------------------------------|
| [`ExpectReport`](#ExpectReport) | Directive | CLOVA가 현재의 설정 정보를 보고하도록 클라이언트에 지시합니다. |
| [`Report`](#Report)             | Event     | 클라이언트가 현재의 설정 정보를 CIC에 보고합니다. 클라이언트가 CIC로부터 [`Settings.ExpectReport`](#ExpectReport) 지시 메시지를 받았다면 `Settings.Report` 이벤트 메시지를 CIC로 전송해야 합니다.  |
| [`RequestVersionSpec`](#RequestVersionSpec)   | Event   | 클라이언트가 추가 설정 정보를 CLOVA에 요청하는데 사용합니다. |
| [`Update`](#Update)             | Directive | CLOVA가 `payload`에 포함된 내용을 설정에 반영하도록 클라이언트에 지시합니다.  |
| [`UpdateVersionSpec`](#UpdateVersionSpec)  | Directive | CLOVA가 `payload`에 포함된 설정 항목을 설정 메뉴에 반영하도록 클라이언트에 지시합니다. |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>설정 정보를 업데이트하거나 동기화하는 설명은 <a href="/Develop/Guides/Handle_Settings.md">설정 정보 처리하기</a>를 참조합니다.</p>
</div>

## ExpectReport directive {#ExpectReport}

CLOVA가 현재의 설정 정보를 보고하도록 클라이언트에 지시합니다. 클라이언트는 이 지시 메시지를 받으면 [`Settings.Report`](#Report) 이벤트 메시지를 CIC로 전송해야 합니다.

### Payload fields

없음

### Remarks

* 이 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

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

### See also

* [`Settings.Report`](#Report)
* [설정 정보 동기화하기](/Develop/Guides/Handle_Settings.md#SynchronizeSettings)

## Report event {#Report}

클라이언트가 현재의 설정 정보를 CIC에 보고합니다. 클라이언트가 CIC로부터 [`Settings.ExpectReport`](#ExpectReport) 지시 메시지를 받았다면 `Settings.Report` 이벤트 메시지를 CIC로 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `configuration` | object | 사전 정의된 클라이언트의 설정 정보를 가지는 객체. 이 객체의 하위 필드는 모두 string 타입을 가집니다.<div class="tip"><p><strong>Tip!</strong></p><p>설정 데이터는 클라이언트마다 다르게 정의될 수 있습니다. 클라이언트 설정 정보에 대한 사전 정의는 제휴 담당자에게 문의합니다.</p></div> | 필수   |

### Message example

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

### See also

* [`Settings.ExpectReport`](#ExpectReport)
* [설정 정보 동기화하기](/Develop/Guides/Handle_Settings.md#SynchronizeSettings)

## RequestVersionSpec event {#RequestVersionSpec}

클라이언트가 추가 설정 정보를 CLOVA에 요청하는데 사용합니다. CLOVA는 기기 음성 설정과 같은 일부 설정 항목을 클라이언트에 맞게 제공합니다. 이를 클라이언트 설정 메뉴에 반영하기 위해 클라이언트는 이 이벤트 메시지를 CIC에 전송해야 합니다.

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
      "namespace": "Settings",
      "name": "RequestVersionSpec",
      "messageId": "3e915ea1-d429-4e21-9043-a1e8f403120a"
    },
    "payload": {}
  }
}
```

### See also

* [`Settings.UpdateVersionSpec`](#UpdateVersionSpec)
* [CLOVA 제공 설정 적용하기](/Develop/Guides/Handle_Settings.md#ApplySettingsProvidedByCLOVA)

## Update directive {#Update}

CLOVA가 `payload`에 포함된 내용을 설정에 반영하도록 클라이언트에 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `configuration` | object | 사전 정의된 클라이언트의 설정 정보를 가지는 객체. 이 객체의 하위 필드는 모두 string 타입을 가집니다.<div class="tip"><p><strong>Tip!</strong></p><p>클라이언트 설정 정보에 대한 사전 정의는 제휴 담당자에게 문의합니다.</p></div> | 항상   |

### Remarks

* 이 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "Update",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
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

### See also

* [`Settings.Report`](#Report)
* [설정 정보 동기화하기](/Develop/Guides/Handle_Settings.md#SynchronizeSettings)


## UpdateVersionSpec directive {#UpdateVersionSpec}

CLOVA가 `payload`에 포함된 설정 항목을 설정 메뉴에 반영하도록 클라이언트에 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `clientName`    | string | 요청 또는 인식된 클라이언트 이름  | 항상   |
| `deviceVersion` | string | 전달된 설정 항목을 적용할 수 있는 클라이언트의 최소 버전 | 항상 |
| `os`            | string | 요청 또는 인식된 클라이언트의 운영체제 | 항상 |
| `spec`          | object | `specVersion` 필드에 명시된 버전이 지원하는 설정 정보를 가지는 객체 | 항상 |
| `spec.voiceType`              | string | 음성 설정 정보를 가지는 객체                  | 조건부  |
| `spec.voiceType.name`         | string | 설정 항목의 표기용 이름 | 항상 |
| `spec.voiceType.list[]`       | object array | 설정 가능한 음성 타입 목록을 가지는 객체 배열 | 항상 |
| `spec.voiceType.list[].key`   | string | 음성 타입의 키 | 항상 |
| `spec.voiceType.list[].name`  | string | 음성 타입의 표기용 이름 | 항상 |
| `spec.voiceType.default`      | object | 기본 음성 타입 정보를 가지는 객체 | 항상 |
| `spec.voiceType.default.key`  | string | 기본 음성 타입의 키 | 항상 |
| `spec.voiceType.default.name` | string | 기본 음성 타입의 표기용 이름 | 항상 |
| `specVersion`                 | string | 전달된 설정 정보의 버전. 버전은 날짜와 시간으로 구성되며, `"YYYYMMDDhhmmss"`의 포맷을 가집니다. 특정 버전이 요청되지 않았다면 항상 최신 버전이 전달됩니다. | 항상 |

### Remarks

현재 CLOVA가 클라이언트에 추가 제공하고 있는 설정 항목은 음성 설정(`spec.voiceType`)입니다. 추후 제공되는 설정 항목이 변경될 수 있습니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "UpdateVersionSpec",
      "messageId": "ce7cffe2-b699-4482-b803-c3a6c4dce53e",
    },
    "payload": {
      "clientName": "FRIENDS_SALLY",
      "deviceVersion": "1.2.6",
      "specVersion": "20181019151800",
      "os": "",
      "spec": {
        "voiceType": {
          "name": "음성 설정",
          "list": [
            {
              "key": "V001",
              "name": "여성 목소리"
            },
            {
              "key": "V002",
              "name": "남성 목소리"
            },
            {
              "key": "V003",
              "name": "친절한 목소리"
            },
            {
              "key": "V004",
              "name": "장난스러운 목소리"
            },
            {
              "key": "V005",
              "name": "빠른 목소리"
            },
            {
              "key": "V007",
              "name": "앙증맞은 목소리"
            }
          ],
          "default": {
            "key": "V001",
            "name": "여성 목소리"
          }
        }
      }
    }
  }
}
```

### See also

* [`Settings.RequestVersionSpec`](#RequestVersionSpec)
* [CLOVA 제공 설정 적용하기](/Develop/Guides/Handle_Settings.md#ApplySettingsProvidedByCLOVA)
