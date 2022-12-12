# Text Template

{% include "/Develop/References/ContentTemplates/TextSnippet.md" %}

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `bgUrl`                  | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 백그라운드로 표시할 이미지의 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.               |
| `emotionCode`            | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 감정 표현이 정의된 코드. 감정 코드를 활용하여 클라이언트 기기에서 미리 정의된 감정 표현을 표시할 수 있습니다. 기기에 감정 표현하는 기능이 존재하지 않으면 이 코드를 무시하면 됩니다. <div class="note"><p><strong>Note!</strong></p><p>감정 코드에 대한 자세한 스펙은 관련 제휴 담당자에게 별도 문의하시기 바랍니다.</p></div> |
| `highlightText`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) 또는 [NumberObject](/Develop/References/ContentTemplates/Shared_Objects.md#NumberObject) | 강조할 텍스트 또는 숫자 정보가 담긴 객체. 숫자 정보이면 구분 단위 기호를 넣어 표현할 수 있습니다. 이 객체의 `value` 필드는 빈 문자열(`""`) 또는 `null` 값을 가질 수도 있습니다. |
| `imageUrl`               | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 이미지의 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                              |
| `linkUrl`                | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 지도 이미지가 포함되었을 때 웹 지도로 이동하는 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `mainText`               | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 메인 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                                     |
| `motionCode`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 동작이 정의된 코드. 동작 코드를 활용하여 클라이언트 기기에서 미리 정의된 움직임을 수행할 수 있습니다. 기기에 동작을 표현하는 기능이 존재하지 않으면 이 코드를 무시하면 됩니다. <div class="note"><p><strong>Note!</strong></p><p>동작 코드에 대한 자세한 스펙은 관련 제휴 담당자에게 별도 문의하시기 바랍니다.</p></div> |
| `paragraphText`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 문단 형태의 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                                |
| `referenceText`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 참조한 서비스의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `referenceUrl`           | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 참조한 서비스의 이용 결과 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `sentenceText`           | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 문장 형태의 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                                |
| `subText`                | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 보조 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                                     |
| `tableList[]`           | object array                                                                    | 표 형태의 문구가 담긴 객체 배열. 행이 두 개 또는 세 개인 표를 구성합니다.     |
| `tableList[].item1`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 첫 번째 행에 표시할 텍스트 정보를 담은 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                    |
| `tableList[].item2`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 두 번째 행에 표시할 텍스트 정보를 담은 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                    |
| `tableList[].item2Link` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) 또는 [PhoneNumberObject](/Develop/References/ContentTemplates/Shared_Objects.md#PhoneNumberObject) | 두 번째 행에 표시된 텍스트의 링크 URI 또는 전화 번호를 담은 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `tableList[].item3`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 세 번째 행에 표시할 텍스트 정보를 담은 객체. 이 필드는 생략될 수 있습니다. |
| `type`                   | string                                                                          | Content template 구분자. `"Text"` 값을 가집니다.             |

## Template example

{% raw %}

```json
// 예제 1.
// 사용자 요청: 1 달러 지금 얼마야? (강조하는 형태의 텍스트 표시)
{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "number",
    "value": "1,119"
  },
  "mainText": {
    "type": "string",
    "value": "원"
  },
  "paragraphText": {
    "type": "string",
    "value": ""
  },
  "referenceText": {
    "type": "string",
    "value": "KEB하나은행"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=1%eb%8b%ac%eb%9f%ac+%ed%99%98%ec%9c%a8"
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": "-0.13%"
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": ""
      },
      "item2": {
        "type": "string",
        "value": ""
      },
      "item2Link": {
        "type": "",
        "value": ""
      }
    }
  ],
  "emotionCode": {
    "type": "string",
    "value": ""
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}

// 예제 2.
// 사용자 요청: 토트넘 감독이 누구야? (문단 형태의 텍스트 표시)
{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "paragraphText": {
    "type": "string",
    "value": "토트넘 홋스퍼 FC 감독\n 마우리시오 포체티노"
  },
  "referenceText": {
    "type": "string",
    "value": "검색결과"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ed%86%a0%ed%8a%b8%eb%84%98+%ea%b0%90%eb%8f%85%ec%9d%b4+%eb%88%84%ea%b5%ac%ec%95%bc?"
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": ""
      },
      "item2": {
        "type": "string",
        "value": ""
      },
      "item2Link": {
        "type": "",
        "value": ""
      }
    }
  ],
  "emotionCode": {
    "type": "string",
    "value": ""
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}

// 예제 3.
// 사용자 요청: 꽃집 전화번호 알려줘 (표 형태의 텍스트 표시)
{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "paragraphText": {
    "type": "string",
    "value": ""
  },
  "referenceText": {
    "type": "string",
    "value": "검색결과"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ea%bd%83%ec%a7%91"
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": "터지머지플라워 분당 정자점"
      },
      "item2": {
        "type": "string",
        "value": "031-716-6676"
      },
      "item2Link": {
        "type": "phoneNum",
        "value": "031-716-6676"
      }
    },
    {
      "item1": {
        "type": "string",
        "value": "서머셋플라워"
      },
      "item2": {
        "type": "string",
        "value": "031-712-3310"
      },
      "item2Link": {
        "type": "phoneNum",
        "value": "031-712-3310"
      }
    },
    ...
  ],
  "emotionCode": {
    "type": "string",
    "value": ""
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}

// 예제 4.
// 사용자 요청: 미안해. (감정 표현)

{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "paragraphText": {
    "type": "string",
    "value": "전혀 미안해 할 거 없어요."
  },
  "referenceText": {
    "type": "string",
    "value": ""
  },
  "referenceUrl": {
    "type": "url",
    "value": ""
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": ""
      },
      "item2": {
        "type": "string",
        "value": ""
      },
      "item2Link": {
        "type": "",
        "value": ""
      }
    }
  ],
  "emotionCode": {
    "type": "string",
    "value": "EmotionCode7"
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}

// 예제 5.
// 사용자 요청: 춤춰줘. (동작 표현)

{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "paragraphText": {
    "type": "string",
    "value": "제 능력 밖의 일입니다."
  },
  "referenceText": {
    "type": "string",
    "value": ""
  },
  "referenceUrl": {
    "type": "url",
    "value": ""
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": ""
      },
      "item2": {
        "type": "string",
        "value": ""
      },
      "item2Link": {
        "type": "",
        "value": ""
      }
    }
  ],
  "emotionCode": {
    "type": "string",
    "value": "MotionDance"
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}
```

{% endraw %}

## See also
* [CardList](/Develop/References/ContentTemplates/CardList.md)
* [ImageList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
