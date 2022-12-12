# ActionTimerList Template

{% include "/Develop/References/ContentTemplates/ActionTimerListSnippet.md" %}
## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `actionTimerList[]`               | object array  | 사용자가 등록한 액션 타이머 목록을 가지는 객체 배열                                              |
| `actionTimerList[].action`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 액션 타이머에 사용자가 설정한 동작이 담긴 객체. **현재 빈 문자열(`""`)이 입력되며 추후 확장을 위해 예약해 둔 필드입니다.** |
| `actionTimerList[].label`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 사용자가 입력한 동작 내용이 담긴 객체 |
| `actionTimerList[].repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 매주 반복되는 액션 타이머이면 반복할 요일 정보를 가지고 있는 객체 배열 |
| `actionTimerList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 반복 주기 정보를 가지는 객체입니다. 이 객체의 `value` 필드는 다음과 같은 값을 가집니다. <ul><li>빈 문자열(<code>""</code>): 일회성 액션 타이머</li><li><code>"daily"</code>: 매일 반복되는 액션 타이머</li><li><code>"weekly"</code>: 매주 반복되는 액션 타이머</li></ul> |
| `actionTimerList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 액션 타이머가 울릴 날짜와 시간 정보를 가지는 객체      |
| `actionTimerList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 액션 타이머의 식별자 정보가 담긴 객체              |
| `type`        | string                                                                                                | Content template 구분자. `"ActionTimerList"` 값을 가집니다.             |

## Template example

{% raw %}

```json
{
  "type": "ActionTimerList",
  "actionTimerList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-01T14:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "음악재생"
      },
      "repeatPeriod": {
        "type": "string",
        "value": ""
      },
      "repeatDay": []
    },
    {
      "token": {
        "type": "string",
        "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-02T09:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "음악재생"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "daily"
      },
      "repeatDay": []
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-03T11:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "음악재생"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "weekly"
      },
      "repeatDay": [
        {
          "type": "string",
          "value": "monday"
        }
      ]
    }
  ]
}
```

{% endraw %}

## See also
* [ActionTimer](/Develop/References/ContentTemplates/ActionTimer.md)
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) 인터페이스
