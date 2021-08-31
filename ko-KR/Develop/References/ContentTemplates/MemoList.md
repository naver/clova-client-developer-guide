# MemoList Template
CIC는 사용자가 메모의 목록을 요청하면 사용자에게 등록된 메모의 목록을 MemoList 템플릿 형태로 클라이언트에 전달합니다. 클라이언트는 이 템플릿을 사용하여 사용자가 등록한 메모 목록을 화면에 표시해야 합니다.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>MemoList 템플릿은 현재 다음과 같은 제약 사항이 있습니다.</p>
  <ul>
    <li>사용자는 음성으로 메모 등록과 메모 목록 조회만 요청할 수 있습니다.</li>
    <li>사용자가 메모를 편집하거나 삭제하려면 CLOVA 앱을 이용해야 합니다.</li>
  </ul>
</div>

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

## UI example {#UIExample}

다음은 landscape 화면 형태에서 MemoList 템플릿의 내용을 표현한 UI 예제입니다.

![Content_Template-MemoList](/Develop/Assets/Images/Content_Template-MemoList.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>화면의 어떤 부분에 어떤 필드가 표시되어야 하는지 알려주는 이미지를 곧 추가할 예정입니다.</p>
</div>

## See also
* [Memo](/Develop/References/ContentTemplates/Memo.md)
