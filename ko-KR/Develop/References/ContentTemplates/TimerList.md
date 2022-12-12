# TimerList Template

{% include "/Develop/References/ContentTemplates/TimerListSnippet.md" %}

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `timerList[]`               | object array  | 사용자의 등록한 타이머 목록을 가지는 객체 배열                                                                                         |
| `timerList[].label`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 타이머의 라벨. 이 객체의 `value` 필드는 빈 문자열(`""`) 또는 `null` 값을 가질 수도 있습니다.    |
| `timerList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 타이머가 울릴 날짜와 시간 정보를 가지는 객체                    |
| `timerList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 타이머의 식별자 정보가 담긴 객체                             |
| `type`                      | string                                                                              | Content template 구분자. `"TimerList"` 값을 가집니다.      |

## Template example

{% raw %}

```json
{
  "type": "Timer",
  "timerList": [
    {
      "label": {
        "type": "string",
        "value": "1분"
      },
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-24T00:00:10Z"
      }
    },
    {
      "label": {
        "type": "string",
        "value": "컵라면 기다리기"
      },
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-24T00:01:00Z"
      }
    }
  ]
}
```

{% endraw %}

## See also
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) 인터페이스
* [Timer](/Develop/References/ContentTemplates/Timer.md)
