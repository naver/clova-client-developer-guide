# TomorrowWeather Template

{% include "/Develop/References/ContentTemplates/TomorrowWeatherSnippet.md" %}

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`                 | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 배경 영상 파일의 URI 정보가 담긴 객체. <div class="warning"><p><strong>Warning!</strong></p><p>이 필드가 제공하는 미디어 콘텐츠는 CLOVA 서비스 외의 용도로 사용하실 수 없습니다.</p></div> |
| `bgDefaultUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 배경 이미지 파일의 URI 정보가 담긴 객체. `bgClipUrl` 필드의 영상을 준비하는 동안 `bgDefaultUrl` 필드의 이미지를 표시할 수 있습니다. 또한, 기술적인 문제로 `bgClipUrl` 필드의 영상을 제공할 수 없을 때 사용할 수 있습니다. <div class="warning"><p><strong>Warning!</strong></p><p>이 필드가 제공하는 미디어 콘텐츠는 CLOVA 서비스 외의 용도로 사용하실 수 없습니다.</p></div> |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 콘텐츠 제공자의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `highTemperature`           | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 내일의 최고 기온 정보가 담긴 객체 |
| `highTempWeather`           | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 기온이 가장 높을 때의 날씨 정보가 담긴 객체  |
| `hourlyWeatherList[]` | object array | 시간대별 날씨 정보를 가지는 객체 배열 |
| `hourlyWeatherList[].hourlyTemperature` | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 시간대별 온도 정보가 담긴 객체 |
| `hourlyWeatherList[].hourlyTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 시간대 정보가 담긴 객체 |
| `hourlyWeatherList[].rainfallProbability` | [PercentageObject](/Develop/References/ContentTemplates/Shared_Objects.md#PercentageObject) | 강수 확률 정보가 담긴 객체. 이 객체의 `value` 필드는 `null` 값을 가질 수도 있습니다.      |
| `hourlyWeatherList[].temperatureImageCode` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 시간대별 [날씨 코드](#WeatherCode) 정보가 담긴 객체 |
| `hourlyWeatherList[].temperatureImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 시간대별 날씨 이미지 파일의 URI 정보가 담긴 객체 |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 날씨 정보가 최종 업데이트된 시간 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `linkUrl`                   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 콘텐츠 링크 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.      |
| `location`                  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 지역 정보가 담긴 객체 |
| `lowTemperature`           | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 내일의 최저 기온 정보가 담긴 객체 |
| `lowTempWeather`           | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 기온이 가장 낮을 때의 날씨 정보가 담긴 객체  |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 참조한 서비스의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 참조한 서비스의 이용 결과 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `type`                      | string | Content template 구분자. `"TomorrowWeather"` 값을 가집니다. |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
{
  "bgClipUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_cloud_daytime.mp4"
  },
  "highTempWeather": {
    "type": "string",
    "value": "구름많음"
  },
  "highTemperature": {
    "type": "temperature-c",
    "value": "32"
  },
  "hourlyWeatherList": [
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "25"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 00:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "23"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 03:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "23"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 06:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "25"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 09:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "27"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 12:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "29"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 15:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "28"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 18:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "26"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 21:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "26"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170728 00:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "24"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170728 03:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "80%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "9"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_09.png"
      }
    }
  ],
  "location": {
    "type": "string",
    "value": "정자1동"
  },
  "lowTempWeather": {
    "type": "string",
    "value": "구름많음"
  },
  "lowTemperature": {
    "type": "temperature-c",
    "value": "23"
  },
  "contentProviderText" : {
    "type" : "string",
    "value" : "기상청"
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
  "type": "TomorrowWeather"
}
```
{% endraw %}

## See also
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
