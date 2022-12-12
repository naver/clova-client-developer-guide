# MemoList Template

{% include "/Develop/References/ContentTemplates/MemoListSnippet.md" %}

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `memoList[]`              | object array  | 사용자가 등록한 메모 목록을 가지는 객체 배열                                        |
| `memoList[].content`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 메모의 내용이 담긴 객체  |
| `memoList[].lastModified` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 메모가 마지막으로 수정된 시간 정보가 담긴 객체 |
| `memoList[].token`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 메모의 식별자 정보가 담긴 객체  |
| `type`                    | string                                                                              | Content template 구분자. `"MemoList"` 값을 가집니다.             |

## Template example

{% raw %}

```json
{
  "type": "MemoList",
  "memoList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "content": {
        "type": "string",
        "value": "내 와이파이 비밀번호: 12345678"
      },
      "lastModified": {
        "type": "datetime",
        "value": "2017-12-24T00:00:00Z"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
      },
      "content": {
        "type": "string",
        "value": "할 일 목록: 숙제하기, 여친 만들기"
      },
      "lastModified": {
        "type": "datetime",
        "value": "2017-12-24T01:00:00Z"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "content": {
        "type": "string",
        "value": "버킷 리스트: 100억 써보기, 아무것도 안하기, 72시간 잠자기"
      },
      "lastModified": {
        "type": "datetime",
        "value": "2017-12-24T02:00:00Z"
      }
    }
  ]
}
```

{% endraw %}

## See also
* [Memo](/Develop/References/ContentTemplates/Memo.md)
