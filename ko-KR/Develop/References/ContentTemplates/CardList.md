# CardList Template

{% include "/Develop/References/ContentTemplates/CardListSnippet.md" %}

CardList는 각 카드 타입에 따라 `card` 객체의 유효한 필드가 달라질 수 있습니다.

| 카드 타입      | 타입 설명                             | 유효 필드(`card` 객체)                           |
|--------------|------------------------------------|-----------------------------------------------|
| `Type1` | 콘텐츠의 이미지, 제목, 설명을 표시하는 카드 타입입니다.      | `description`, `imageUrl`, `referenceText`, `referenceUrl`, `title`             |
| `Type2` | 콘텐츠의 이미지, 제목, 설명, 링크를 표시하는 카드 타입입니다. | `description`, `imageUrl`, `linkUrl`, `referenceText`, `referenceUrl`, `title`  |
| `Type3` | 비디오 콘텐츠를 표시하는 카드 타입입니다.                 | `imageUrl`, `referenceText`, `referenceUrl`, `title`, `videoUrl`                |
| `Type4` | 뉴스 콘텐츠를 표시하는 카드 타입입니다.                  | `description`, `press`, `pressIconUrl`, `publishDate`, `title`                  |
| `Type5` | 오디오 콘텐츠를 표시하는 카드 타입입니다.                 | `description`, `imageUrl`, `title`, `videoUrl`                                  |
| `Type6` | 미디어의 썸네일을 표시하는 카드입니다.                   | `imageUrl`, `linkUrl`                                                      |

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `cardList[]`                | object array | 카드 목록을 표현하는 객체 배열 |
| `cardList[].contentProviderText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 콘텐츠 제공자의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `cardList[].description[]`    | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 콘텐츠의 설명이 담긴 객체 배열          |
| `cardList[].imageUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 표시해야 할 이미지의 URI가 담긴 객체. 카드 타입에 따라 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `cardList[].linkUrl`        | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 콘텐츠의 URI 정보가 담긴 객체. 카드 타입에 따라 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.         |
| `cardList[].press`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | 언론사의 이름이 담긴 객체. 카드 타입에 따라 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.             |
| `cardList[].pressIconUrl`   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 언론사 아이콘의 URI 정보가 담긴 객체. 카드 타입에 따라 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.    |
| `cardList[].publishDate`    | [DateObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateObject) 또는 [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)         | 기사 게시 시기가 담긴 객체. 이 객체의 `type` 필드가 `"date"`이면 `"YYYY-MM-DD"`와 같은 날짜가 포함되며, `"string"`이면 `"3일 전"`, `"2시간 전"`, `"30분 전"`과 같이 경과 시간이 포함됩니다. 카드 타입에 따라 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.            |
| `cardList[].referenceText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | 참조한 서비스의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `cardList[].referenceUrl`   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 참조한 서비스의 이용 결과 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `cardList[].title`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | 콘텐츠의 제목이 담긴 객체             |
| `cardList[].videoUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 재생해야 할 비디오 혹은 오디오의 URI가 담긴 객체. 카드 타입에 따라 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.    |
| `noticeLink`                | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | `noticeText` 필드에 담긴 내용과 관련하여 자세한 설명 페이지나 출처의 URI가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. **이 객체의 `value` 필드 값이 빈 문자열(`""`)이 아니면 반드시 화면에 링크를 표시해야 합니다.**  |
| `noticeText`                | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | 콘텐츠의 저작권이나 법적인 사항과 관련된 내용이 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. **이 객체의 `value` 필드 값이 빈 문자열(`""`)이 아니면 반드시 화면에 표시해야 합니다.**  |
| `subType`                   | string  | Card 타입 구분자. 다음과 같이 4 가지 타입이 지정됩니다. <ul><li><code>Type1</code></li><li><code>Type2</code></li><li><code>Type3</code></li><li><code>Type4</code></li></ul><div class="note"><p><strong>Note!</strong></p><p>현재 <code>Type1</code>, <code>Type2</code>, <code>Type3</code>, <code>Type5</code>, <code>Type6</code>은 <strong>빈 문자열로 표현</strong>됩니다. 따라서 <code>card</code> 객체의 필드 구성을 보고 타입을 판단해야 합니다.</p></div>                                                    |
| `type`                      | string  | Content template 구분자. `"CardList"` 값을 가집니다.                                                                       |

## Template example

```json
// Type1, Type2 예제:
// 사용자 요청: 공포 영화 추천해줘

{
  "subType": "",
  "type": "CardList",
  "cardList": [{
      "contentProviderText": {
        "type": "string",
        "value": "영화"
      },
      "description": [{
          "type": "string",
          "value": "공포, 스릴러"
        },
        {
          "type": "string",
          "value": "아론 에크하트, 데이비드 매주즈, 캐리스 밴 허슨, 카타리나 산디노 모레노, 키어 오도넬, 매트 네이블, 존 피루첼로, 엠제이 안소니, 카롤리나 위드라, 마크 스테거, 토마스 아라나, 페트라 스프레처, 마크 헨리, 애슐리 그린 엘리자베스"
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://movie.phinf.contentservice.example.net/20170410_12/1491786049305s4W0n_JPEG/movie_image.jpg?type=w640_2"
      },
      "linkUrl": {
        "type": "url",
        "value": "https://movie.contentservice.example.com/movie/bi/mi/basic.nhn?code=118965"
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "검색결과"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=+%ec%98%81%ed%99%94"
      },
      "title": {
        "type": "string",
        "value": "인카네이트"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    {
      "contentProviderText": {
        "type": "string",
        "value": "영화"
      },
      "description": [{
          "type": "string",
          "value": "공포"
        },
        {
          "type": "string",
          "value": "마틸다 안나 잉그리드 루츠, 알렉스 로, 자니 갈렉키, 빈센트 도노프리오, 에이미 티가든, 보니 모건, 로라 위긴스, 잭 로어리그, 리지 브로체르"
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://movie.phinf.contentservice.example.net/20170317_53/1489741954272MquSW_JPEG/movie_image.jpg?type=w640_2"
      },
      "linkUrl": {
        "type": "url",
        "value": "https://movie.contentservice.example.com/movie/bi/mi/basic.nhn?code=137909"
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "검색결과"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=+%ec%98%81%ed%99%94"
      },
      "title": {
        "type": "string",
        "value": "링스"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    ...
  ]
}

// Type3 예제
// 사용자 요청: 축구 동영상 보여줘
{
  "subType": "",
  "type": "CardList",
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "TV"
      },
      "description": [
        {
          "type": "string",
          "value": "04:14"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://hol.phinf.contentservice.example.net/00/587/820/58782072_0.jpg"
      },
      "linkUrl": {
        "type": "url",
        "value": ""
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "검색결과"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%b6%95%ea%b5%ac+%eb%8f%99%ec%98%81%ec%83%81+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "title": {
        "type": "string",
        "value": "[해외반응] U20 월드컵, 이탈리아 일본, 해외네티즌 \"최악의 더러운 경기\" - 해외네티즌반응"
      },
      "videoUrl": {
        "type": "url",
        "value": "https://m.tv.contentservice.example.com/v/1720910"
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "TV"
      },
      "description": [
        {
          "type": "string",
          "value": "06:31"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://hol.phinf.contentservice.example.net/00/587/815/58781581_0.jpg"
      },
      "linkUrl": {
        "type": "url",
        "value": ""
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "검색결과"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%b6%95%ea%b5%ac+%eb%8f%99%ec%98%81%ec%83%81+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "title": {
        "type": "string",
        "value": "FA컵 결승: 아스널 v 첼시 경기 후 인터뷰"
      },
      "videoUrl": {
        "type": "url",
        "value": "https://m.sports.contentservice.example.com/video.nhn?id=310230"
      }
    },
    ...
  ]
}

// Type4 예제
// 사용자 요청: 최신 뉴스 보여줘
{
  "subType": "Type4",
  "type": "CardList",
  "noticeText": {
    "type": "string",
    "value": "자동 추출 기술로 요약된 내용으로, 본문의 주요 내용이 제외될 수 있습니다."
  },
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "뉴스"
      },
      "description": [
        {
          "type": "string",
          "value": "다음 달이면 말 많고 탈 많았던 '4대강'의 보 수문이 열립니다. 문재인 대통령이 지난 22일 '4대강 사업'에 대한 감사 지시를 하면서 이뤄진 결정입니다. 이를 계기로 4대강 사업 논란이 다시 수면 위로 떠올랐습니"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": ""
      },
      "linkUrl": {
        "type": "url",
        "value": "https://news.contentservice.example.com/main/read.nhn?mode=LSD&mid=sec&sid1=001&oid=025&aid=0002720454"
      },
      "press": {
        "type": "string",
        "value": "중앙일보"
      },
      "publishDate": {
        "type": "date",
        "value": "2017-05-28"
      },
      "referenceText": {
        "type": "string",
        "value": ""
      },
      "referenceUrl": {
        "type": "url",
        "value": ""
      },
      "title": {
        "type": "string",
        "value": "[시민마이크]'녹차라떼 4대강' 감사, 시민들의 생각은?"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "뉴스"
      },
      "description": [
        {
          "type": "string",
          "value": "5월 마지막 주말 나들이객 줄이어더위 식히고 모래작품 보고(부산=연합뉴스) 조정호 기자 = 초여름 날씨를 보인 28일 부산 해운대 모래축제가 열린 해운대해수욕장에서 나들이객들이 가로 25ｍ, 높이 5ｍ 크기의 대형 모래작품을 구경하고 있습니다. 2017.5.28(전국종합=연합뉴스) 5월의 마지막 주말인 28일 초여름 날씨가..."
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": ""
      },
      "linkUrl": {
        "type": "url",
        "value": "https://news.contentservice.example.com/main/read.nhn?mode=LSD&mid=sec&sid1=001&oid=001&aid=0009297247"
      },
      "press": {
        "type": "string",
        "value": "연합뉴스"
      },
      "publishDate": {
        "type": "date",
        "value": "2017-05-28"
      },
      "referenceText": {
        "type": "string",
        "value": ""
      },
      "referenceUrl": {
        "type": "url",
        "value": ""
      },
      "title": {
        "type": "string",
        "value": "단오제 등 전국 곳곳 축제…초여름 날씨에 때이른 피서 행렬"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    ...
  ]
}

// Type5 예제
// 사용자 요청: ASMR 들려줘
{
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "뮤직"
      },
      "description": [
        {
          "type": "string",
          "value": "07:25"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://tvcast1.phinf.contentservice.example.net/20180105_40/rYaFz_1515134168871cxwhn_JPEG/1515134043644.jpg"
      },
      "linkUrl": {
        "type": "url",
        "value": ""
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "검색결과"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=asmr+%ec%b0%be%ea%b8%b0"
      },
      "title": {
        "type": "string",
        "value": "[<mark>ASMR</mark>] 커피 한잔하실래요?"
      },
      "videoUrl": {
        "type": "url",
        "value": "https://m.tv.contentservice.example.com/v/2509121"
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "뮤직"
      },
      "description": [
        {
          "type": "string",
          "value": "05:05"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://tvcast2.phinf.contentservice.example.net/20180104_140/7QzKq_15150467287668gEkL_JPEG/1515046724731.jpg"
      },
      "linkUrl": {
        "type": "url",
        "value": ""
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "검색결과"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=asmr+%ec%b0%be%ea%b8%b0"
      },
      "title": {
        "type": "string",
        "value": "[<mark>ASMR</mark>] 물 끓는 소리 영상"
      },
      "videoUrl": {
        "type": "url",
        "value": "https://m.tv.contentservice.example.com/v/2503662"
      }
    },
    ...
  ],
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
  },
  "subType": "",
  "type": "CardList"
}
```

## See also
* [ImageList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
