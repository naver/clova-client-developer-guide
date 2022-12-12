# ReminderList Template

{% include "/Develop/References/ContentTemplates/ReminderListSnippet.md" %}

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `reminderList[]`               | object array  | 사용자가 등록한 리마인더 목록을 가지는 객체 배열                                                                                          |
| `reminderList[].content`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | **(Deprecated)** 추가한 리마인더에 사용자가 입력한 내용이 담긴 객체. `label` 필드로 대체될 예정입니다. |
| `reminderList[].label`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 추가한 리마인더에 사용자가 입력한 내용이 담긴 객체. |
| `reminderList[].repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 매주 반복되는 리마인더이면 반복할 요일 정보를 가지고 있는 객체 배열 |
| `reminderList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 반복 주기 정보를 가지는 객체입니다. 이 객체의 `value` 필드는 다음과 같은 값을 가집니다. <ul><li>빈 문자열(<code>""</code>): 일회성 리마인더</li><li><code>"daily"</code>: 매일 반복되는 리마인더</li><li><code>"weekly"</code>: 매주 반복되는 리마인더</li></ul> |
| `reminderList[].status`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 리마인더의 처리 여부를 나타내는 객체입니다. 이 객체의 `value` 필드는 다음과 같은 값을 가집니다. <ul><li><code>"TODO"</code>: 미완료된 리마인더</li><li><code>"DONE"</code>: 완료된 리마인더</li></ul> |
| `reminderList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 리마인더가 울릴 날짜와 시간 정보를 가지는 객체      |
| `reminderList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 리마인더의 식별자 정보가 담긴 객체  |
| `type`                         | string                                                                              | Content template 구분자. `"ReminderList"` 값을 가집니다.             |

## Template example

{% raw %}

```json
{
  "type": "ReminderList",
  "reminderList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-09T09:00:00Z"
      },
      "repeatPeriod": {
        "type": "string",
        "value": ""
      },
      "repeatDay": [],
      "content": {
        "type": "string",
        "value": "입금하기"
      },
      "label": {
        "type": "string",
        "value": "입금하기"
      },
      "status": {
        "type": "string",
        "value": "DONE"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-09T09:00:00Z"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "daily"
      },
      "repeatDay": [],
      "content": {
        "type": "string",
        "value": "비타민 먹기"
      },
      "label": {
        "type": "string",
        "value": "비타민 먹기"
      },
      "status": {
        "type": "string",
        "value": "TODO"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-09T09:00:00Z"
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
      ],
      "content": {
        "type": "string",
        "value": "청소하기"
      },
      "label": {
        "type": "string",
        "value": "청소하기"
      },
      "status": {
        "type": "string",
        "value": "TODO"
      }
    }
  ]
}
```

{% endraw %}

## See also
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) 인터페이스
* [Reminder](/Develop/References/ContentTemplates/Reminder.md)
