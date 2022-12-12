# ImageList Template

{% include "/Develop/References/ContentTemplates/ImageListSnippet.md" %}

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `thumbImageUrlList[]`                | object array | 이미지 목록을 표현하는 객체 배열                        |
| `thumbImageUrlList[].imageReference` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 이미지의 출처 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.      |
| `thumbImageUrlList[].imageTitle`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 이미지 제목이 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.           |
| `thumbImageUrlList[].imageUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 이미지의 URL 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.      |
| `thumbImageUrlList[].referenceText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 참조한 서비스의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `thumbImageUrlList[].referenceUrl`   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 참조한 서비스의 이용 결과 URL 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `thumbImageUrlList[].thumbImageUrl`  | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 썸네일 이미지의 URL 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.      |
| `type`                       | string       | Content template 구분자. `"ImageList"` 값을 가집니다.        |

## Template example

```json
// 사용자 요청: 자동차 사진 보여줘
{
  "type": "ImageList",
  "thumbImageUrlList": [
    {
      "imageReference": {
        "type": "string",
        "value": ""
      },
      "imageTitle": {
        "type": "string",
        "value": "창원대리운전 번개처럼 빠르게"
      },
      "imageUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m_image&mode=default&query=%EC%9E%90%EB%8F%99%EC%B0%A8%20%EC%9D%B4%EB%AF%B8%EC%A7%80#imgId=post7533909_3"
      },
      "referenceText": {
        "type": "string",
        "value": "검색결과"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%9e%90%eb%8f%99%ec%b0%a8+%ec%82%ac%ec%a7%84+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "thumbImageUrl": {
        "type": "url",
        "value": "https://search.pstatic.example.net/common/?src=http%3A%2F%2Fpost.phinf.contentservice.example.net%2FMjAxNzA1MDZfMTg4%2FMDAxNDk0MDYyNDAwMDY3.C6LJCKXrha2u8dIqOOX0RhQNGrVVfkp3WbLO8U-xzRwg.IEYdykQp6xguEy4bnQ83JhDy1QZOtO4n1Lx5MBwivFwg.JPEG%2FIz2FmvAaRVzSf2Z-sNWzYQVU5z6Q.jpg&type=b360"
      }
    },
    {
      "imageReference": {
        "type": "string",
        "value": ""
      },
      "imageTitle": {
        "type": "string",
        "value": "자동차"
      },
      "imageUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m_image&mode=default&query=%EC%9E%90%EB%8F%99%EC%B0%A8%20%EC%9D%B4%EB%AF%B8%EC%A7%80#imgId=gallery2004021016070294818_1"
      },
      "referenceText": {
        "type": "string",
        "value": "검색결과"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%9e%90%eb%8f%99%ec%b0%a8+%ec%82%ac%ec%a7%84+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "thumbImageUrl": {
        "type": "url",
        "value": "https://search.pstatic.example.net/common/?src=http%3A%2F%2Fthumb.photo.contentservice.example.net%2Fdata15%2Fgallery%2F2004-02%2F10%2F07%2F18m2948m0.jpg&type=b360"
      }
    },
    ...
  ]
}

```

## See also
* [CardList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
