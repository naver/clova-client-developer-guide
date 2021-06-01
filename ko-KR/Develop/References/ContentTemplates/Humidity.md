# Humidity Template
습도 정보를 제공하는 템플릿입니다. 화면에 습도 정보를 표시할 때 사용됩니다.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>습도 정보를 표시한 예는 <a href="#UIExample">UI example</a>을 참조합니다.</p>
</div>

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`     | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 배경 영상 파일의 URI 정보가 담긴 객체. <div class="warning"><p><strong>Warning!</strong></p><p>이 필드가 제공하는 미디어 콘텐츠는 CLOVA 서비스 외의 용도로 사용하실 수 없습니다.</p></div> |
| `bgDefaultUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 배경 이미지 파일의 URI 정보가 담긴 객체. `bgClipUrl` 필드의 영상을 준비하는 동안 `bgDefaultUrl` 필드의 이미지를 표시할 수 있습니다. 또한, 기술적인 문제로 `bgClipUrl` 필드의 영상을 제공할 수 없을 때 사용할 수 있습니다. <div class="warning"><p><strong>Warning!</strong></p><p>이 필드가 제공하는 미디어 콘텐츠는 CLOVA 서비스 외의 용도로 사용하실 수 없습니다.</p></div> |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 콘텐츠 제공자의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `humidity`      | [PercentageObject](/Develop/References/ContentTemplates/Shared_Objects.md#PercentageObject) | 습도 정보가 담긴 객체. |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 날씨 정보가 최종 업데이트된 시간 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `linkUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 콘텐츠 링크 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `location`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 지역 정보가 담긴 객체 |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 참조한 서비스의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 참조한 서비스의 이용 결과 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `temperatureCode`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | [날씨 코드](#WeatherCode) 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `type`          | string | Content template 구분자. `"Humidity"` 값을 가집니다. |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
{
  "bgImageUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_cloud_night.mp4"
  },
  "humidity": {
    "type": "percentage",
    "value": "60%"
  },
  "location": {
    "type": "string",
    "value": "정자1동"
  },
  "contentProviderText" : {
    "type" : "string",
    "value" : "기상청"
  },
  "temperatureCode": {
    "type": "string",
    "value": "5"
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
  "type": "Humidity"
}
```
{% endraw %}

## UI example {#UIExample}
다음은 landscape 화면 형태에서 Humidity 템플릿의 내용을 표현한 UI 예제입니다.

![Content_Template-Humidity](/Develop/Assets/Images/Content_Template-Humidity.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>화면의 어떤 부분에 어떤 필드가 표시되어야 하는지 알려주는 이미지를 곧 추가할 예정입니다.</p>
</div>

## See also
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
