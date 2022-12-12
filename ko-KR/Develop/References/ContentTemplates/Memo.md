# Memo Template

{% include "/Develop/References/ContentTemplates/MemoSnippet.md" %}

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `content`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 메모의 내용이 담긴 객체  |
| `timestamp`   | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 메모 생성 시간 정보가 담긴 객체 |
| `token`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 추가한 메모의 식별자 정보가 담긴 객체  |
| `type`        | string                                                                              | Content template 구분자. `"Memo"` 값을 가집니다.             |

## Template example

{% raw %}

```json
{
  "type": "Memo",
  "token": {
    "type": "string",
    "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
  },
  "content": {
    "type": "string",
    "value": "내 와이파이 비밀번호: 12345678"
  },
  "timestamp": {
    "type": "datetime",
    "value": "2017-12-24T00:00:00Z"
  }
}
```

{% endraw %}

## See also
* [MemoList](/Develop/References/ContentTemplates/MemoList.md)
