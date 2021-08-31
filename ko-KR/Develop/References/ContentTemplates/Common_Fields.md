# 공통 필드
모든 content template은 다음과 같은 공통 필드를 가질 수 있습니다. 공통 필드는 content template 객체의 최상위에 위치하게 됩니다.

| 필드 이름        | 자료형    | 필드 설명                     | 포함 여부 |
|----------------|---------|-----------------------------|:---------:|
| `actionList[]`     | [ActionObject](/Develop/References/ContentTemplates/Shared_Objects.md#ActionObject) array | UI 터치와 같은 사용자 인터랙션에 대응하여 사용자에게 제공해야 할 동작이 무엇인지 정의한 객체 배열입니다. 사용자에게 제공해야 할 동작은 [Action URI scheme](#ActionURIScheme) 형태로 전달됩니다. [CardList](/Develop/References/ContentTemplates/CardList.md) 타입의 content template은 `cardList[]` 필드 하위에 정의될 수 있습니다. | 조건부 |
| `failureMessage`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | UI에 content template를 표시하지 못할 때 보여줄 메시지 정보를 포함합니다. 예를 들면, 클라이언트가 `meta.version`에 명시된 content template의 버전을 지원하지 않거나 템플릿 정보를 표시하는데 문제가 있을 때 보여줄 메시지 입니다. | 항상 |
| `meta`             | object | Content template과 관련된 메타 정보를 포함합니다. | 항상 |
| `meta.version`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | Content template의 버전 정보를 포함합니다. | 항상 |
| `subtitle`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 부제나 보조 정보를 표시하기 위한 텍스트를 포함합니다. | 조건부 |
| `emoji`            | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | <a href="https://home.unicode.org/emoji/about-emoji/" target="_blank">이모지(emoji)</a>를 표시하기 위해 <a href="https://unicode.org/emoji/charts/full-emoji-list.html" target="_blank">유니코드 표</a>에 정의되어 있는 코드를 포함합니다. | 조건부 |

## 공통 필드 Example

{% raw %}
```json
{
  "type": "Atmosphere",
  "valueOfAtmosphere": {
    "type": "number",
    "value": "23㎍/㎥"
  },
  ...
  "failureMessage": {
    "type": "string",
    "value": "경기도 성남시 분당구 정자1동 오늘 미세먼지 지수는 좋음 입니다"
  },
  "subtitle": {
    "type": "string",
    "value": "정자1동 오늘 미세먼지 지수는 좋음 입니다"
  },​
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
  },
  "emoji": {
    "type": "string",
    "value": "U+1F600"
  }
}
```
{% endraw %}

## Action URI scheme {#ActionURIScheme}
공통 필드 중 `actionList` 필드에는 다음과 같은 action URI scheme 형태의 값이 포함되어 있습니다. 이 값은 사용자의 UI 인터렉션에 대응하여 제공해야 할 동작이 무엇인지 정의한 값입니다. 클라이언트는 각 action URI schme 값을 보고 그에 상응하는 동작을 사용자에게 제공해야 합니다.

| Action URI scheme           | 클라이언트가 수행해야 할 동작                                               |
|-----------------------------|------------------------------------------------------------------|
| [clova://app-launch/default-addressbook](#AppLaunchDefaultAddrBook) | 기본 주소록 앱을 실행하는 동작   |
| [clova://app-launch/default-browser](#AppLaunchDefaultBrowser)      | 기본 웹 브라우저를 실행하는 동작 |
| [clova://app-launch/default-camera](#AppLaunchDefaultCamera)        | 기본 카메라 앱을 실행하는 동작   |
| [clova://app-launch/default-email](#AppLaunchDefaultEmail)          | 기본 메일 앱을 실행하는 동작    |
| [clova://app-launch/default-gallery](#AppLaunchDefaultGallery)      | 기본 갤러리 앱을 실행하는 동작   |
| [clova://audio-repeat](#AudioRepeat)                                | 오디오 출력을 수행하는 동작     |
| [clova://device-control](#DeviceControl)                            | 기기 제어를 수행하는 동작       |
| [clova://guide/talking](#GuideTalking)     | 명령 도우미를 제공하는 동작                              |
| [clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search](#NaverSearch)        | {{ book.ServiceEnv.OrientedService }} 앱에서 특정 키워드를 검색하는 동작                    |
| [clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}-maps](#NaverMaps)           | {{ book.ServiceEnv.OrientedService }} 지도 앱을 실행하는 동작                            |
| [clova://ttsRepeat](#TTSRepeat)            | Text to speech 발화를 수행하는 동작                     |
| [clova://webview](#WebView)                | WebView로 웹 페이지를 여는 동작                          |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Action URI scheme이 지시 메시지와 다른 점은 지시 메시지는 클라이언트가 지시 메시지를 받을 때 관련 동작을 바로 수행해야 하지만 action URI scheme은 화면이나 기타 수단으로 제공된 UI에서 사용자의 인터랙션이 발생했을 때 클라이언트가 관련 동작을 제공해야 합니다. 클라이언트는 action URI scheme에 관련된 동작을 제공할 때 위 표에서 정의한대로 용도에 맞는 동작을 제공해야 하며, 임의의 동작을 수행해서는 안됩니다.</p>
</div>

### clova://app-launch/default-addressbook {#AppLaunchDefaultAddrBook}

클라이언트는 이 스키마에 대응하여 기본 주소록 앱을 실행해야 합니다. 이 action URI scheme의 예는 다음과 같습니다.

```
clova://app-launch/default-addressbook
```

### clova://app-launch/default-browser {#AppLaunchDefaultBrowser}

클라이언트는 이 스키마에 대응하여 기본 웹 브라우저를 실행해야 합니다. 이 action URI scheme의 예는 다음과 같습니다.

```
clova://app-launch/default-browser
```

### clova://app-launch/default-camera {#AppLaunchDefaultCamera}

클라이언트는 이 스키마에 대응하여 기본 카메라 앱을 실행해야 합니다. 이 action URI scheme의 예는 다음과 같습니다.

```
clova://app-launch/default-camera
```

### clova://app-launch/default-email {#AppLaunchDefaultEmail}

클라이언트는 이 스키마에 대응하여 기본 메일 앱을 실행해야 합니다. 이 action URI scheme의 예는 다음과 같습니다.

```
clova://app-launch/default-email
```

### clova://app-launch/default-gallery {#AppLaunchDefaultGallery}

클라이언트는 이 스키마에 대응하여 기본 갤러리 앱을 실행해야 합니다. 이 action URI scheme의 예는 다음과 같습니다.

```
clova://app-launch/default-gallery
```

### clova://audio-repeat {#AudioRepeat}

클라이언트는 이 스키마에 대응하여 오디오 파일 재생을 실행해야 합니다.

| 파라미터 이름    | 설명                         | 포함 여부 |
|---------------|-----------------------------|:--------:|
| url           | 오디오 파일 URI                | 항상 |

이 action URI scheme의 예는 다음과 같습니다.

```
clova://audio-repeat?url=https://target.audioFile.url
```

### clova://device-control {#DeviceControl}

클라이언트는 이 스키마에 대응하여 특정 기능을 제어해야 합니다.

| 파라미터 이름    | 설명                         | 포함 여부 |
|---------------|-----------------------------|:--------:|
| command       | 제어 명령. <ul><li>BtConnect</li><li>BtDisconnect</li><li>BtStartPairing</li><li>BtStopPairing</li><li>Decrease</li><li>Increase</li><li>LaunchApp</li><li>OpenScreen</li><li>SetValue</li><li>TurnOff</li><li>TurnOn</li></ul>                      | 항상 |
| target        | 제어 대상. <ul><li><code>"airplane"</code>: 비행기 모드</li><li><code>"app"</code>: 앱</li><li><code>"bluetooth"</code>: 블루투스</li><li><code>"cellular"</code>: 모바일 통신</li><li><code>"channel"</code>: TV 채널</li><li><code>"flashlight"</code>: 플래시 조명</li><li><code>"gps"</code>: GPS</li><li><code>"powersave"</code>: 절전 모드</li><li><code>"screenautobrightness"</code>: 화면 자동 밝기</li><li><code>"screenbrightness"</code>: 화면 밝기</li><li><code>"soundmode"</code>: 사운드 모드</li><li><code>"volume"</code>: 스피커 볼륨</li><li><code>"wifi"</code>: 무선랜</li></ul> | 항상 |
| value         | 설정값. `command` 파라미터가 `setValue`일 때 스피커 볼륨(`"volume"`) 또는 화면 밝기(`"screenbrightness"`)를 설정하거나 TV 채널(`"channel"`)을 설정할 때 지정되는 값입니다. | 조건부 |

이 action URI scheme의 예는 다음과 같습니다.

```
clova://device-control?command=SetValue&target=volume&value=5
clova://device-control?command=TurnOn&target=flashlight
```

### clova://guide/talking {#GuideTalking}

클라이언트는 이 스키마에 대응하여 명령 도우미를 제공해야 합니다. 이 action URI scheme의 예는 다음과 같습니다.


```
clova://guide/talking
```

### clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search {#NaverSearch}

클라이언트는 이 스키마에 대응하여 {{ book.ServiceEnv.OrientedService }} 앱을 실행하고 검색을 수행해야 합니다.

| 파라미터 이름    | 설명                         | 포홤 여부 |
|---------------|-----------------------------|:---------:|
| url           | {{ book.ServiceEnv.OrientedService }} 앱을 통해 열려는 페이지의 URI | 항상 |

이 action URI scheme의 예는 다음과 같습니다.

```
clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search?url=http://target.page.url
```

### clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}-maps {#NaverMaps}

클라이언트는 이 스키마에 대응하여 {{ book.ServiceEnv.OrientedService }} 지도 앱을 실행하고 길찾기 기능을 수행해야 합니다.

| 파라미터 이름    | 설명                         | 포함 여부 |
|---------------|-----------------------------|:---------:|
| url           | {{ book.ServiceEnv.OrientedService }} 지도 앱을 통해 열려는 URI   | 항상 |

이 action URI scheme의 예는 다음과 같습니다.

```
clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}-maps?url=http://target.map.url
```

### clova://ttsRepeat {#TTSRepeat}

클라이언트는 이 스키마에 대응하여 특정 텍스트를 음성으로 출력해야 합니다.

| 파라미터 이름    | 설명                         | 포함 여부 |
|---------------|-----------------------------|:---------:|
| lang          | TTS(Text to Speech) 대상 언어. <ul><li><code>"en"</code>: 영어</li><li><code>"ja"</code>: 일본어</li><li><code>"ko"</code>: 한국어</li></ul> | 항상 |
| text          | 발화할 텍스트                   | 조건부 |

이 action URI scheme의 예는 다음과 같습니다.

```
clova://ttsRepeat?lang=en&text=hello
```

### clova://webview {#WebView}
클라이언트는 이 스키마에 대응하여 WebView에서 특정 페이지를 열어야 합니다.

| 파라미터 이름    | 설명                         | 포함 여부 |
|---------------|-----------------------------|:---------:|
| url           | 대상 페이지의 URI              | 항상 |
| auth_required | 인증 필요 여부. 이 파라미터가 `true`이면, 대상 페이지를 열 때 인증 API를 사용해야 하며, `false`이거나 명시가 안되었다면 인증이 필요하지 않습니다. | 조건부 |

이 action URI scheme의 예는 다음과 같습니다.

```
clova://webview?url=https://target.page.url&auth_required=true
```
