# Notifier

Notifier 인터페이스는 CLOVA가 알림이 있음을 표시하도록 지시하거나 사용자 계정에 등록된 모든 클라이언트에 알림 표시를 없애도록 클라이언트에 지시합니다. CIC는 클라이언트가 사용자 알림 확인 상태를 최신으로 유지하게 만들기 위해 클라이언트 요청이 없어도 다음과 같은 지시 메시지를 전달합니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`ClearIndicator`](#ClearIndicator)         | Directive | CLOVA가 알림을 나타내는 표시를 모두 끄도록 클라이언트에 지시합니다.             |
| [`Notify`](#Notify)                         | Directive | CLOVA가 알림 내용을 사용자에게 전달하도록 클라이언트에 지시합니다.              |
| [`SetIndicator`](#SetIndicator)             | Directive | CLOVA가 사용자가 읽지 않은 알림이 있음을 표시하도록 클라이언트에 지시합니다.      |

## ClearIndicator directive {#ClearIndicator}

CLOVA가 알림을 나타내는 표시를 모두 끄도록 클라이언트에 지시합니다. 클라이언트는 알림을 표시하는 조명이나 소리 효과를 모두 꺼야 합니다.

### Payload fields

없음

### Remarks

해당 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Notifier",
      "name": "ClearIndicator",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {}
  }
}
```

### See also

* [`Notifier.Notify`](#Notify)
* [`Notifier.SetIndicator`](#SetIndicator)

## Notify directive {#Notify}

CLOVA가 알림 내용을 사용자에게 전달하도록 클라이언트에 지시합니다. 클라이언트는 지정된 방식으로 알림용 조명을 켜고 알림과 관련된 오디오 콘텐츠를 순서대로 재생해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `assetPlayOrder[]`   | string array | `assets[]` 필드에 등록된 알림음을 어떤 순서로 재생해야 하는지 표현하고 있는 문자열 배열입니다. 배열에 저장된 오디오 콘텐츠 식별자의 순서대로 알림음을 재생하면 됩니다.            | 항상  |
| `assets[]`           | object array | 알림과 관련된 오디오 콘텐츠를 담고 있는 객체 배열                          | 항상 |
| `assets[].assetId`   | string       | 오디오 콘텐츠를 구분하는 식별자                                        | 항상 |
| `assets[].url`       | string       | 오디오 콘텐츠의 URI 정보입니다. 다음과 같은 scheme 값이나 오디오 콘텐츠의 URI 값을 가집니다.<ul><li><code>"clova://notifier/sound/default"</code>: 기본 알림음을 지칭하는 scheme입니다. 미리 정의된 기본 알림음을 재생합니다.</li><li>오디오 콘텐츠의 URI(<code>"http(s):&ZeroWidthSpace;//~"</code>): 알림 내용이 담긴 오디오 콘텐츠의 URI. 해당 URI의 오디오 콘텐츠를 재생합니다.</li></ul>    | 항상 |
| `light`              | string       | 조명 설정 정보<ul><li><code>"DEFAULT"</code>: 알림 표시용 조명을 점등해야 합니다.</li><li><code>"NONE"</code>: 알림 표시용 조명을 점등하지 않습니다.</li></ul>   | 항상  |

### Remarks

해당 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Notifier",
      "name": "Notify",
      "messageId": "688a3d72-06c4-4628-b238-9ac454d3ea65",
    },
    "payload": {
      "light": "DEFAULT",
      "assets": [
        {
          "assetId": "e5179318-d061-42f9-af1b-417180142934",
          "url": "clova://notifier/sound/default"
        },
        {
          "assetId": "9d3df3c1-d0b4-4375-84a6-67c7ae000292",
          "url": "https://streaming.example.com/3325-b5c75045b4ae426885343f9b6abd0bfc-1508160634257"
        }
      ],
      "assetPlayOrder": [
        "e5179318-d061-42f9-af1b-417180142934",
        "9d3df3c1-d0b4-4375-84a6-67c7ae000292"
      ]
    }
  }
}
```

### See also

* [`Notifier.ClearIndicator`](#ClearIndicator)
* [`Notifier.SetIndicator`](#SetIndicator)
* [조명](/Design/UI/Light.md)
* [소리](/Design/UI/Audio.md)

## SetIndicator directive {#SetIndicator}

CLOVA가 사용자가 읽지 않은 알림이 있음을 표시하도록 클라이언트에 지시합니다. 클라이언트는 알림용 조명을 켜거나 지정된 알림음을 재생해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `assetPlayOrder[]`   | string array | `assets[]` 필드에 등록된 알림음을 어떤 순서로 재생해야 하는지 표현하고 있는 문자열 배열입니다. 배열에 저장된 오디오 콘텐츠 식별자의 순서대로 알림음을 재생하면 됩니다.            | 항상  |
| `assets[]`           | object array | 알림과 관련된 오디오 콘텐츠를 담고 있는 객체 배열                          | 항상 |
| `assets[].assetId`   | string       | 오디오 콘텐츠를 구분하는 식별자                                        | 항상 |
| `assets[].url`       | string       | 오디오 콘텐츠의 URI 정보입니다. 다음과 같은 scheme 값이나 오디오 콘텐츠의 URI 값을 가집니다.<ul><li><code>"clova://notifier/sound/default"</code>: 기본 알림음을 지칭하는 scheme입니다. 미리 정의된 기본 알림음을 재생합니다.</li><li>오디오 콘텐츠의 URI(<code>"http(s):&ZeroWidthSpace;//~"</code>): 알림 내용이 담긴 오디오 콘텐츠의 URI. 해당 URI의 오디오 콘텐츠를 재생합니다.</li></ul>    | 항상 |
| `light`              | string       | 조명 설정 정보<ul><li><code>"DEFAULT"</code>: 알림 표시용 조명을 점등해야 합니다.</li><li><code>"NONE"</code>: 알림 표시용 조명을 점등하지 않습니다.</li></ul> | 항상    |
| `new`                | boolean      | 새로운 알림에 대한 지시 메시지인지 알려주는 필드입니다. <ul><li><code>true</code>: 새로운 알림일 때</li><li><code>false</code>: 새로운 알림이 아닐 때</li></ul> | 항상    |

### Remarks

해당 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Notifier",
      "name": "SetIndicator",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {
      "assets": [{
        "assetId": "3ea201e8-135f-42fd-a75c-f125331ff9bd",
        "url": "clova://notifier/sound/default"
      }],
      "assetPlayOrder": ["3ea201e8-135f-42fd-a75c-f125331ff9bd"],
      "new": true,
      "light": "DEFAULT"
    }
  }
}
```

### See also

* [`Notifier.ClearIndicator`](#ClearIndicator)
* [`Notifier.Notify`](#Notify)
* [조명](/Design/UI/Light.md)
* [소리](/Design/UI/Audio.md)
