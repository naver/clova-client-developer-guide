# Popup Template
Toast, alert, popup으로 표시해야 할 텍스트나 버튼에 대한 정보를 제공하는 템플릿입니다. 표시 형태에 따라 유효한 필드가 달라질 수 있습니다.

| 표시 형태       | 설명                      | 유효 필드                         |
|---------------|-----------------------------|-----------------------------|
| Toast         | 문장과 관련 링크로 구성된 toast입니다.    | `toastLinkText`, `toastLinkUrl`, `toastText`                  |
| Alert         | 문장과 확인 버튼으로 구성된 alert입니다.   | `alertText`                                                   |
| Popup(버튼 1 개) | 제목, 문장, 버튼(link)으로 구성된 popup입니다. | `mainText`, `positiveButtonText`, `positiveButtonUrl`, `title`   |
| Popup(버튼 2 개) | 제목, 문장, 두 개의 버튼으로 구성된 popup입니다. | `negativeButtonText`, `negativeButtonUrl`, `mainText`, `positiveButtonText`, `positiveButtonUrl`, `title` |

<div class="tip">
<p><strong>Tip!</strong></p>
<p>Popup 템플릿의 표시 형태는 <a href="#UIExample">UI example</a>을 참조합니다.</p>
</div>

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `alertText`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | Alert에 표시할 주의 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `displayType`      | string                                                                          | 표시할 화면의 종류. 다음과 같은 값을 가질 수 있습니다.<ul><li><code>"POPUP"</code></li><li><code>"ALERT"</code></li><li><code>"TOAST"</code></li></ul>  |
| `negativeButtonText`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | Popup에서 **아니오**와 같이 부정의 의미에 해당하는 버튼에 표시할 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `negativeButtonUrl`    | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | Popup에서 **아니오**와 같이 부정의 의미에 해당하는 버튼에 연결될 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `mainText`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | Popup에 표시할 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `positiveButtonText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | Popup에서 **예**와 같이 긍정에 해당하는 버튼에 표시할 문구가 담긴 객체. 버튼 한 개짜리 popup이면 **확인**과 같은 의미의 버튼에 표시할 문구로 이 객체를 사용합니다. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `positiveButtonUrl`   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | Popup에서 **예**와 같이 긍정에 해당하는 버튼에 연결될 URI 정보가 담긴 객체. 버튼 한 개짜리 popup이면 **확인**과 같은 의미의 버튼에 연결될 URI로 이 객체를 사용합니다. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `title`            | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | Popup에 표시할 제목이 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `toastLinkText`    | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | Toast에 표시할 링크의 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `toastLinkUrl`     | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | Toast에 표시할 링크의 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `toastText`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | Toast에 표시할 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `type`             | string                                                                          | Content template 구분자. `"Popup"` 값을 가집니다.     |

## Template example

{% raw %}
```json
// 예제 1. Toast 형태
{
  "type": "Popup",
  "displayType": "TOAST",
  "toastText": {
    "type": "string",
    "value": "1분 미리듣기 중입니다. 음악 취향 길들이기에 참여하고 뮤직 100곡 이용권 받으세요!"
  },
  "toastLinkText": {
    "type": "string",
    "value": "이벤트 참여 >"
  },
  "toastLinkUrl": {
    "type": "url",
    "value": "https://..."
  },
  "alertText": {
    "type": "string",
    "value": ""
  },
  "title": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonUrl": {
    "type": "url",
    "value": ""
  },
  "positiveButtonText": {
    "type": "string",
    "value": ""
  },
  "positiveButtonUrl": {
    "type": "url",
    "value": ""
  }
}

// 예제 2. Alert 형태
{
  "type": "Popup",
  "displayType": "ALERT",
  "toastText": {
    "type": "string",
    "value": ""
  },
  "toastLinkText": {
    "type": "string",
    "value": ""
  },
  "toastLinkUrl": {
    "type": "url",
    "value": ""
  },
  "alertText": {
    "type": "string",
    "value": "다른 기기에서 재생을 시작하여 음악이 중지되었습니다."
  },
  "title": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonUrl": {
    "type": "url",
    "value": ""
  },
  "positiveButtonText": {
    "type": "string",
    "value": ""
  },
  "positiveButtonUrl": {
    "type": "url",
    "value": ""
  }
}

// 예제 3. 버튼 한 개짜리 popup 형태
{
  "type": "Popup",
  "displayType": "POPUP",
  "toastText": {
    "type": "string",
    "value": ""
  },
  "toastLinkText": {
    "type": "string",
    "value": ""
  },
  "toastLinkUrl": {
    "type": "url",
    "value": ""
  },
  "alertText": {
    "type": "string",
    "value": ""
  },
  "title": {
    "type": "string",
    "value": "취향파악 완료!"
  },
  "mainText": {
    "type": "string",
    "value": "이제 뮤직 100곡 무료 이용권으로 클로바의 추천 음악을 즐기세요!"
  },
  "negativeButtonText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonUrl": {
    "type": "url",
    "value": ""
  },
  "positiveButtonText": {
    "type": "string",
    "value": "뮤직 이용권 받기"
  },
  "positiveButtonUrl": {
    "type": "url",
    "value": "https://..."
  }
}

// 예제 4. 버튼 두 개짜리 popup 형태
{
  "type": "Popup",
  "displayType": "POPUP",
  "toastText": {
    "type": "string",
    "value": ""
  },
  "toastLinkText": {
    "type": "string",
    "value": ""
  },
  "toastLinkUrl": {
    "type": "url",
    "value": ""
  },
  "alertText": {
    "type": "string",
    "value": ""
  },
  "title": {
    "type": "string",
    "value": "취향파악 완료!"
  },
  "mainText": {
    "type": "string",
    "value": "고객님의 음악 취향을 알게되어서 추천을 더 잘할 수 있겠어요."
  },
  "negativeButtonText": {
    "type": "string",
    "value": "계속"
  },
  "negativeButtonUrl": {
    "type": "url",
    "value": "https://..."
  },
  "positiveButtonText": {
    "type": "string",
    "value": "종료"
  },
  "positiveButtonUrl": {
    "type": "url",
    "value": "https://..."
  }
}
```
{% endraw %}

## UI example {#UIExample}
다음은 landscape 화면 형태에서 Popup 템플릿의 내용을 표현한 UI 예제입니다.

* Toast 형태

  ![Content_Template-Toast](/Develop/Assets/Images/Content_Template-Toast.png)

* Alert 형태

  ![Content_Template-Alert](/Develop/Assets/Images/Content_Template-Alert.png)

* Popup 형태(버튼 1개)

  ![Content_Template-Popup_with_One_Button](/Develop/Assets/Images/Content_Template-Popup_with_One_Button.png)

* Popup 형태(버튼 2개)

  ![Content-Template-Popup_with_Two_Buttons](/Develop/Assets/Images/Content_Template-Popup_with_Two_Buttons.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>화면의 어떤 부분에 어떤 필드가 표시되어야 하는지 알려주는 이미지를 곧 추가할 예정입니다.</p>
</div>

## See also
* [CardList](/Develop/References/ContentTemplates/CardList.md)
* [ImageList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
