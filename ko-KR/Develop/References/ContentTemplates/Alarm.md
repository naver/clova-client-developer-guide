# Alarm Template

{% include "/Develop/References/ContentTemplates/AlarmSnippet.md" %}

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 매주 반복되는 알람이면 반복할 요일 정보를 가지고 있는 객체 배열     |
| `repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 반복 주기 정보를 가지는 객체입니다. 이 객체의 `value` 필드는 다음과 같은 값을 가집니다. <ul><li>빈 문자열(<code>""</code>): 일회성 알람 </li><li><code>"daily"</code>: 매일 반복되는 알람</li><li><code>"weekly"</code>: 매주 반복되는 알람</li></ul> |
| `scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 알람이 울릴 날짜와 시간 정보를 가지는 객체                         |
| `token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 추가한 알람의 식별자 정보가 담긴 객체                            |
| `type`          | string                                                                              | Content template 구분자. `"Alarm"` 값을 가집니다.             |

## Template example

{% raw %}

```json

// 일회성 알람
{
  "type": "Alarm",
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
  "repeatDay": []
}

// 매일 반복되는 알람
{
  "type": "Alarm",
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
  "repeatDay": []
}

// 매주 반복되는 알람
{
  "token": {
    "type": "string",
    "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
  },
  "type": "Alarm",
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
  ]
}
```

{% endraw %}

## See also
* [AlarmList](/Develop/References/ContentTemplates/AlarmList.md)
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) 인터페이스
