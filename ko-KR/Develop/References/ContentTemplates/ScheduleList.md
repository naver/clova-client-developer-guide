# ScheduleList Template
CIC는 사용자가 캘린더 일정의 목록을 요청하면 사용자에게 등록된 일정의 목록을 ScheduleList 템플릿 형태로 클라이언트에 전달합니다. 클라이언트는 이 템플릿을 사용하여 사용자가 등록한 일정 목록을 화면에 표시해야 합니다.

  <div class="tip">
  <p><strong>Tip!</strong></p>
  <p>ScheduleList 템플릿은 현재 다음과 같은 제약 사항이 있습니다.</p>
  <ul>
    <li>사용자가 캘린더 계정을 등록, 수정, 삭제하려면 CLOVA 앱을 이용해야 합니다.</li>
    <li>사용자는 음성으로 캘린더의 일정 등록과 일정 목록 조회만 요청할 수 있습니다.</li>
    <li>사용자가 캘린더의 일정을 수정하거나 삭제하려면 CLOVA 앱을 이용해야 합니다.</li>
  </ul>
</div>

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `scheduleList[]`        | object array | 사용자가 등록한 일정 목록을 가지는 객체 배열   |
| `scheduleList[].content`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 추가한 일정에 사용자가 입력한 내용이 담긴 객체 |
| `scheduleList[].end`           | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) 또는 [DateObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateObject)  | 추가한 일정의 종료 날짜와 시간. 종일 일정이면 DateObject 형태의 자료형을 가지며, 날짜 정보만 가집니다. |
| `scheduleList[].start`         | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) 또는 [DateObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateObject)  | 추가한 일정의 시작 날짜와 시간. 종일 일정이면 DateObject 형태의 자료형을 가지며, 날짜 정보만 가집니다. |
| `scheduleList[].repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 매주 반복되는 일정이면 반복할 요일 정보를 가지고 있는 객체 배열 |
| `scheduleList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 반복 주기 정보를 가지는 객체입니다. 이 객체의 `value` 필드는 다음과 같은 값을 가집니다. <ul><li>빈 문자열(<code>""</code>): 일회성 일정 </li><li><code>"daily"</code>: 매일 반복되는 일정</li><li><code>"weekly"</code>: 매주 반복되는 일정</li></ul> |
| `scheduleList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 일정의 식별자 정보가 담긴 객체  |
| `type`        | string                                                                              | Content template 구분자. `"ScheduleList"`로 고정             |

## Template example

{% raw %}

```json
{
  "type": "ScheduleList",
  "scheduleList": [
    {
      "type": "Schedule",
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "start": {
        "type": "datetime",
        "dateTime": "2017-09-30T12:00:00+09:00"
      },
      "end": {
        "type": "datetime",
        "dateTime": "2017-09-30T13:00:00+09:00"
      },
      "content": {
        "type": "string",
        "value": "친구 결혼식"
      },
      "repeatPeriod": {
        "type": "string",
        "value": ""
      },
      "repeatDay": []
    },
    {
      "type": "Schedule",
      "token": {
        "type": "string",
        "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
      },
      "start": {
        "type": "datetime",
        "dateTime": "2017-08-02T10:00:00+09:00"
      },
      "end": {
        "type": "datetime",
        "dateTime": "2017-08-02T11:00:00+09:00"
      },
      "content": {
        "type": "string",
        "value": "데일리 스크럼"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "daily"
      },
      "repeatDay": []
    },
    {
      "type": "Schedule",
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "start": {
        "type": "datetime",
        "dateTime": "2017-08-02T10:00:00+09:00"
      },
      "end": {
        "type": "datetime",
        "dateTime": "2017-08-02T11:00:00+09:00"
      },
      "content": {
        "type": "string",
        "value": "주간 회의"
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
    },
    {
      "type": "Schedule",
      "token": {
        "type": "string",
        "value": "5c8b4f7b-d8bd-4817-a1c3-eb9c9522277e"
      },
      "start": {
        "type": "date",
        "dateTime": "2017-09-29"
      },
      "end": {
        "type": "datetime",
        "dateTime": "2017-09-29"
      },
      "content": {
        "type": "string",
        "value": "플레이숍"
      },
      "repeatPeriod": {
        "type": "string",
        "value": ""
      },
      "repeatDay": []
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

다음은 landscape 화면 형태에서 ScheduleList 템플릿의 내용을 표현한 UI 예제입니다.

![Content_Template-ScheduleList](/Develop/Assets/Images/Content_Template-ScheduleList.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>화면의 어떤 부분에 어떤 필드가 표시되어야 하는지 알려주는 이미지를 곧 추가할 예정입니다.</p>
</div>

## See also
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) 인터페이스
* [ScheduleList](/Develop/References/ContentTemplates/ScheduleList.md)
