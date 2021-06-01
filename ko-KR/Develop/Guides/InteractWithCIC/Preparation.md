## 사전 준비사항 {#Preparation}
클라이언트는 HTTP/2 프로토콜을 사용하여 CIC에 연결해야 합니다. 따라서, CIC에 연결하기 전에 다음을 미리 준비해야 합니다.

* HTTP/2 프로토콜 연결을 위한 [라이브러리](#RequiredLibrary)
* 클라이언트 기기의 [User-Agent string](#UserAgentString)
* CLOVA access token 획득을 위한 [클라이언트 인증 정보](#ClientAuthInfo)


### 필요 라이브러리 {#RequiredLibrary}
HTTP/2 프로토콜 연결을 위해 클라이언트 개발 시 다음과 같은 라이브러리 사용을 권장합니다.

| 개발 언어 | 라이브러리                            |
|---------|------------------------------------|
| C/C++   | [nghttp2](https://nghttp2.org/), [libcurl](https://curl.haxx.se/libcurl/) |
| Java    | [OkHttp](https://square.github.io/okhttp/), [Netty](https://netty.io/), [Jetty](https://www.eclipse.org/jetty/) |


### User-Agent string {#UserAgentString}

클라이언트는 CLOVA 및 CLOVA와 관련된 웹 서비스를 이용할 때 User-Agent string을 HTTP 헤더에 첨부해야 합니다. 클라이언트는 다음과 같은 작업을 처리하기 위해 HTTP 통신을 시도하게 되며, User-Agent string은 아래의 모든 상황에서 적극 활용됩니다.

* [CIC 서버와 통신](#ConnectToCIC) 할 때
* 앱에서 WebView로 페이지를 열 때
* UI에 필요한 이미지를 다운로드할 때
* 미디어 재생기에서 오디오 스트림을 요청할 때

<div class="note">
  <p><strong>Note!</strong></p>
  <p>HTTP 헤더를 통해 User-Agent string을 보내지 않거나 규칙에 맞지 않는 User-Agent string을 보내게 되면 클라이언트의 연결이나 요청이 거부될 수 있습니다.</p>
</div>

User-Agent string을 작성할 때 지켜야하는 문법([BNF 문법](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form))은 다음과 같습니다.

```
letter     = "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J" | "K" | "L" | "M"
           | "N" | "O" | "P" | "Q" | "R" | "S" | "T" | "U" | "V" | "W" | "X" | "Y" | "Z"
           | "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m"
           | "n" | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x" | "y" | "z" ;
digit      = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
whitespace = " " ;
number     = digit , { digit } ;
character  = letter | digit | "_" | "-" | "." ;
word       = character , { character } ;
string     = word
           | word , { whitespace | word } , word ;
safe_char  = letter | digit | "_" ;
safe_word  = safe_char , { safe_char } ;

urlencoded_character = character | "%" ;
urlencoded_string    = urlencoded_character , { urlencoded_character } ;

slash               = "/" ;
slash_delimiter     = [ whitespace ] , slash , [ whitespace ] ;
semicolon           = ";" ;
semicolon_delimiter = [ whitespace ] , semicolon , [ whitespace ] ;

build      = safe_word
           | safe_word , "." , build ;
release    = safe_word
           | safe_word , "." , release ;
version    = number , "." , number , "." , "number"
           | number , "." , number , "." , "number" , "-" , release
           | number , "." , number , "." , "number" , "+" , build
           | number , "." , number , "." , "number" , "-" , release , "+" , build ;

Manufacturer_Identifier = string ;
Product_Identifier      = string ;
Firmware_Version        = version ;
OS_Information          = string ;
HW_Information          = string ;
property                = word , "=" , urlencoded_string ;
extra_information       = property , { semicolon_delimiter , property } ;

User_Agent = Manufacturer_Identifier , slash_delimiter ,
             Product_Identifier , slash_delimiter ,
             Firmware_Version ,
             [ whitespace ] , "(" ,
             OS_Information , semicolon_delimiter ,
             HW_Information ,
             [ semicolon_delimiter , extra_informations ] , ")" ;
```

이 문법을 적용하여 User-Agent string을 작성한 예는 다음과 같습니다. **참고로 `Firmware_Version`에 대한 버전 번호를 입력할 때 [Semantic Versioning](https://semver.org/) 규칙을 따라야 합니다.**

```
MyCompanyName/MyProductName/1.0.1 (KindOfOS 0.9;CustomHardwareInfo;sampleKey1=sampleValue1;sampleKey2=sampleValue2)
MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
AbbreviationOfCompanyName/MyClient/0.8.2+build (Linux 7.0;Raspberry_pi;domain=Sample%40Clova;type=Test%20Type)
MCN/SampleClient/1.7.0 (SampleIoTOS 2.1;SmartHub)
```

<div class="note">
  <p><strong>Note!</strong></p>
  <p>User-Agent string은 추후 CLOVA developer console를 통해 등록해야 합니다. CIC용 CLOVA developer console이 개발 중이므로 User-Agent string 등록은 제휴 담당자에게 문의합니다.</p>
</div>

### 클라이언트 인증 정보 {#ClientAuthInfo}
사용자는 클라이언트에서 {{ book.ServiceEnv.TargetServiceForClientAuth }} 계정을 인증해야 CLOVA가 제공하는 서비스를 사용할 수 있습니다. 클라이언트는 사용자로부터 입력받은 {{ book.ServiceEnv.TargetServiceForClientAuth }}의 계정 정보를 토대로 {{ book.ServiceEnv.TargetServiceForClientAuth }} 계정 access token을 획득할 수 있습니다. 이 access token을 다시 클라이언트 인증 서버로 전달하여 [CLOVA access token를 획득](#ObtainCLOVAAccessToken)해야 합니다.

{{ book.ServiceEnv.TargetServiceForClientAuth }} 계정 access token뿐만 아니라 CLOVA developer console을 통해 획득한 클라이언트 인증 정보를 함께 인증 서버로 전달해야 합니다([클라이언트 인증 API](/Develop/References/Client_Auth_API.md) 사용). 따라서 CLOVA developer console을 통해 클라이언트 인증 정보를 미리 확보해야 해야 합니다. 클라이언트 인증 정보는 다음과 같습니다.

| 인증 정보                   | 설명                                              |
|---------------------------|--------------------------------------------------|
| `CLOVA_OAUTH_CLIENT_ID`     | CLOVA developer console에 등록한 클라이언트 ID         |
| `CLOVA_OAUTH_CLIENT_SECRET` | CLOVA developer console을 통해 획득한 클라이언트 secret |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>클라이언트용 CIC - CLOVA developer console을 개발하는 중입니다. 따라서, 클라이언트 인증 정보는 제휴 담당자를 통해 확보해야 합니다.</p>
</div>
