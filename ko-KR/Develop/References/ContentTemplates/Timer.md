# Timer Template

{% include "/Develop/References/ContentTemplates/TimerSnippet.md" %}

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 추가된 타이머가 울릴 날짜와 시간 정보를 가지는 객체             |
| `token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 추가된 타이머의 식별자 정보가 담긴 객체                     |
| `type`          | string                                                                              | Content template 구분자. `"Timer"` 값을 가집니다.        |

## Template example

{% raw %}

```json
{
  "type": "Timer",
  "token": {
    "type": "string",
    "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
  },
  "scheduledTime": {
    "type": "datetime",
    "value": "2017-12-24T00:00:00Z"
  }
}
```

{% endraw %}

## See also
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) 인터페이스
* [TimerList](/Develop/References/ContentTemplates/TimerList.md)
