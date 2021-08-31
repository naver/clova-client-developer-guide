# Atmosphere Template
대기 정보를 제공하는 템플릿입니다. 화면에 미세먼지, 초미세먼지, 오존, 자외선, 황사 정보를 표시할 때 사용됩니다.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>대기 정보를 표시한 예는 <a href="#UIExample">UI example</a>을 참조합니다.</p>
</div>

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `announcementOfAtmosphere`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 예보 안내 문구가 담긴 객체. 현재 대기 상태 정보를 보여줄 때는 이 필드가 생략됩니다. 생략될 때 이 객체의 `value` 필드는 빈 문자열(`""`)을 가집니다. |
| `bgClipUrl`     | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 배경 영상 파일의 URI 정보가 담긴 객체. <div class="warning"><p><strong>Warning!</strong></p><p>이 필드가 제공하는 미디어 콘텐츠는 CLOVA 서비스 외의 용도로 사용하실 수 없습니다.</p></div> |
| `bgDefaultUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 배경 이미지 파일의 URI 정보가 담긴 객체. `bgClipUrl` 필드의 영상을 준비하는 동안 `bgDefaultUrl` 필드의 이미지를 표시할 수 있습니다. 또한, 기술적인 문제로 `bgClipUrl` 필드의 영상을 제공할 수 없을 때 사용할 수 있습니다. <div class="warning"><p><strong>Warning!</strong></p><p>이 필드가 제공하는 미디어 콘텐츠는 CLOVA 서비스 외의 용도로 사용하실 수 없습니다.</p></div> |
| `concentrationOfAtmosphere` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 현재 대기 상태나 공기질 수준 정보가 담긴 객체. 예보를 할 때는 이 필드가 생략됩니다. 생략될 때는 이 객체의 `value` 필드는 빈 문자열(`""`)을 가집니다. |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 콘텐츠 제공자의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `halfDayAtmosphereList[]`             | object array | 반나절 단위(오전/오후)로 구분된 시간대별 대기 정보를 가지는 객체 배열                                    |
| `halfDayAtmosphereList[].atmosphereImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | `halfDayAtmosphereList[].durationHalfDay` 필드에 입력된 시간대의 대기 상태를 표시하는 이미지 파일의 URI 정보가 담긴 객체 |
| `halfDayAtmosphereList[].concentrationOfAtmosphere`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | `halfDayAtmosphereList[].durationHalfDay` 필드에 입력된 시간대의 대기 상태나 공기질 수준 정보가 담긴 객체  |
| `halfDayAtmosphereList[].durationHalfDay`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 대기 정보를 나타내는 시간 범위가 담긴 객체. 이 객체의 `value` 필드는 `내일 오전`, `내일 오후`, `모레 오전`, `모레 오후`와 같은 값을 가집니다.  |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 날씨 정보가 최종 업데이트된 시간 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `linkUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 콘텐츠 링크 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `location`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 지역 정보가 담긴 객체 |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 참조한 서비스의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 참조한 서비스의 이용 결과 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `temperatureCode`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | [날씨 코드](#WeatherCode) 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `type`          | string | Content template 구분자. `"Atmosphere"` 값을 가집니다. |
| `valueOfAtmosphere`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 현재 대기의 수치 정보가 담긴 객체. 수치의 단위가 포함되어 있습니다. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
// 현재 대기 정보
{
  "announcementOfAtmosphere": {
    "type": "string",
    "value": ""
  },
  "bgClipUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_clean_daytime.mp4"
  },
  "concentrationOfAtmosphere": {
    "type": "string",
    "value": "좋음"
  },
  "contentProviderText" : {
    "type" : "string",
    "value" : "기상청"
  },
  "failureMessage": {
    "type": "string",
    "value": "경기도 성남시 분당구 정자1동 현재 미세먼지 지수는 좋음 입니다"
  },
  "halfDayAtmosphereList": [
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "보통"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "내일 오전"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "보통"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "내일 오후"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "보통"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "모레 오전"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "보통"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "모레 오후"
      }
    }
  ],
  "location": {
    "type": "string",
    "value": "정자1동"
  },
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
  },
  "lastUpdate" : {
    "type" : "datetime",
    "value" : "2018-02-05T06:29:09Z"
  },
  "referenceText" : {
    "type" : "string",
    "value" : "날씨"
  },
  "referenceUrl" : {
    "type" : "url",
    "value" : "https://weather.contentservice.example.com/"
  },
  "temperatureCode": {
    "type": "string",
    "value": "5"
  },
  "type": "Atmosphere",
  "valueOfAtmosphere": {
    "type": "number",
    "value": "27㎍/㎥"
  }
}

// 내일 대기 정보
{
  "announcementOfAtmosphere": {
    "type": "string",
    "value": "내일 미세먼지에요"
  },
  "bgClipUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_cloud_daytime.mp4"
  },
  "concentrationOfAtmosphere": {
    "type": "string",
    "value": ""
  },
  "failureMessage": {
    "type": "string",
    "value": "경기도 성남시 분당구 정자1동 내일 미세먼지 지수는 보통 입니다"
  },
  "halfDayAtmosphereList": [
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "보통"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "내일 오전"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "보통"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "내일 오후"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "보통"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "모레 오전"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "보통"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "모레 오후"
      }
    }
  ],
  "location": {
    "type": "string",
    "value": "정자1동"
  },
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
  },
  "type": "Atmosphere",
  "valueOfAtmosphere": {
    "type": "number",
    "value": ""
  }
}
```
{% endraw %}

## UI example {#UIExample}
다음은 landscape 화면 형태에서 Atmosphere 템플릿의 내용을 표현한 UI 예제입니다.

* 현재 대기 상태

  ![Content_Template-Atmosphere-Now](/Develop/Assets/Images/Content_Template-Atmosphere-Now.png)

* 내일 대기 상태

  ![Content_Template-Atmosphere-Tomorrow](/Develop/Assets/Images/Content_Template-Atmosphere-Tomorrow.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>화면의 어떤 부분에 어떤 필드가 표시되어야 하는지 알려주는 이미지를 곧 추가할 예정입니다.</p>
</div>

## See also
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
