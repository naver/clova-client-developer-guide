# CIC API
CIC API는 CIC가 클라이언트에 제공하는 REST API입니다. CIC API에 대해 다음과 같은 내용을 다룹니다.
* [API 기본 정보](#BasicInfo)
* [Downchannel을 구성](#EstablishDownchannel)
* [이벤트 메시지를 전송](#SendEvent)
* [메시지 포맷](#CICMessageFormat)

## API 기본 정보 {#BasicInfo}
CIC API를 사용하기 전에 기본적으로 알아야 할 정보는 다음과 같습니다.
* [Base URI](#BaseURI)
* [Multipart 메시지](#MultipartMessage)

### Base URI {#BaseURI}
CIC API의 base URI는 다음과 같습니다.

<pre><code>{{ book.ServiceEnv.CICBaseURI }}</code></pre>

### Multipart 메시지 {#MultipartMessage}
클라이언트는 HTTP/2를 이용해 [이벤트 메시지](#Event)를 다음과 같은 multipart 메시지로 전송하게 됩니다.

![](/Develop/Assets/Images/HTTP2_Structure.png)

예를 들면, 사용자의 음성 입력을 CIC로 전달하려면 [SpeechRecognizer.Recognize](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) 이벤트 메시지와 함께 녹음한 사용자의 음성 데이터를 함께 전송해야 합니다. 클라이언트는 `Content-Type`을 `multipart/form-data`로 설정하고 첫 번째 메시지 파트에는 이벤트 메시지 정보가 담긴 JSON 데이터를 두 번째 메시지 파트에는 사용자의 음성이 담긴 바이너리 데이터를 담아서 보낼 수 있습니다.

이때, 메시지를 구분하기 위해 `boundary`에 경계 문구(boundary term)를 지정해야 합니다. 경계 문구는 메시지 파트 사이에 사용될 때 경계 문구 왼쪽에 이중의 하이픈(-) 기호를 붙여야 하며, 마지막 메시지 파트 이후에는 경계 문구 양쪽에 이중의 하이픈(-) 기호를 붙여야 합니다. 또한, 경계 문구는 각 메시지 파트의 본문에서 나타나지 않아야 합니다.

다음은 클라이언트가 CIC로 사용자 요청(이벤트 메시지)을 보낼 때 갖추게 되는 일반적인 메시지 형태입니다.

{% raw %}
```
:method = POST
:scheme = https
:path = /v1/events
User-Agent: {{User-Agent_string}}
Authorization: Bearer {{clova-access-token}}
Content-Type: multipart/form-data; boundary=this-is-boundary-text

--this-is-boundary-text
[ Message Header ]
Content-Disposition: form-data; name="metadata"
Content-Type: application/json; charset=UTF-8

[ Message Body ]
{
  "context": [
  ],
  "event": {
    "header": {
      "namespace": {{string}},
      "name": {{string}},
      "dialogRequestId": {{string}},
      "messageId": {{string}}
    },
    "payload": {{object}}
  }
}

--this-is-boundary-text
[ Message Header ]
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[[ binary audio attachment ]]

--this-is-boundary-text--

```
{% endraw %}

일반적인 HTTP 응답은 성공을 의미하는 [HTTP 상태 코드](https://tools.ietf.org/html/rfc7231#section-6)(200)와 함께 [지시 메시지](#Directive)가 전달되며, 다음과 같은 메시지 조합을 가집니다.
* [`Synthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak)은 음성을 출력하는 지시 메시지로 음성 데이터가 추가로 전달됩니다.
* `Synthesizer.Speak` 지시 메시지와 함께 부가 정보를 전달하는 지시 메시지가 전달될 수 있습니다. 예를 들면, 스트리밍 정보를 포함하고 있는 [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) 지시 메시지가 추가로 전달될 수 있습니다.

위 설명과 같이 CIC에서 클라이언트로 전달되는 응답도 지시 메시지와 음성 데이터로 조합된 multipart 메시지가 전달됩니다. 다음과 같은 구조를 가집니다.

{% raw %}
```
HTTP/2 {{HTTP STATUS CODE}}
Content-Type: multipart/related; boundary=this-is-boundary-text;
Date: {{DATETIME}}

--this-is-boundary-text
[ Message Header ]
Content-Disposition: form-data; name="metadata"
Content-Type: application/json; charset=UTF-8

[ Message Body ]
{
  "directive": {
    "header": {
      "namespace": {{string}},
      "name": {{string}},
      "dialogRequestId": {{string}},
      "messageId": {{string}}
    },
    "payload": {{object}}
  }
}

--this-is-boundary-text
[ Message Header ]
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[[ binary audio attachment ]]

--this-is-boundary-text

...

--this-is-boundary-text--

```
{% endraw %}


## Downchannel 구성 {#EstablishDownchannel}

```
GET /v1/directives
```

클라이언트는 가장 먼저 CIC와 downchannel을 구성해야 합니다. Downchannel은 특정 조건이나 필요에 의해 CIC의 주도(Cloud-initiated)로 클라이언트에 보내지는 지시 메시지를 수신할 때 사용됩니다. Downchannel을 구성하는 방법에 대한 자세한 내용은 [처음 연결하기](/Develop/Guides/Interact_with_CIC.md#CreateConnection)를 참조합니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Downchannel을 구성하지 않으면 CIC로 <a href="#SendEvent">이벤트 메시지를 전송</a>할 수 없습니다.</p>
</div>

### Request header

| Request header | 설명                                                                |
|----------------|--------------------------------------------------------------------|
| `Authorization`       | <p>획득한 CLOVA access token을 입력합니다.</p><pre><code>Bearer [CLOVA access token]</code></pre> |
| `User-Agent`          | <p><a href="/Develop/Guides/Interact_with_CIC.md#UserAgentString">User agent string</a>을 입력합니다.</p><pre><code>User-Agent: [User-Agent string]</code></pre>  |
| `X-Clova-Device-Tags` | <p>클라이언트에 지정된 태그를 입력합니다. 태그가 하나 이상이면 쉼표(,)로 구분하여 나열합니다.</p><pre><code>X-Clova-Device-Tags: Tag1,Tag2,Tag3,...,TagN</code></pre> <div class="note"><p><strong>Note!</strong></p><p>태그는 클라이언트에 속성을 부여하는 것으로 특정 태그를 가진 클라이언트를 조회하거나 제어하는 등 다양한 목적으로 사용될 수 있습니다. 태그에 대한 자세한 내용은 제휴 담당자에게 문의하시기 바랍니다.</p></div> |

### Request example

<pre><code>GET /v1/directives HTTP/2
Host: {{ book.ServiceEnv.CICBaseURI }}
User-Agent: MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
Authorization: Bearer XHapQasdfsdfFsdfasdflQQ7w</code></pre>

### Response header

| Response header | 설명                                                                |
|-----------------|--------------------------------------------------------------------|
| `Content-Type`    | <p><a href="#MultipartMessage">Multipart 메시지</a> 타입 및 경계 문구 선언:</p><pre><code>multipart/form-data; boundary=[boundary_term]</code></pre> |

### Response message header

| Response message header | 설명                                                                |
|-------------------------|--------------------------------------------------------------------|
| `Content-Disposition`     | <pre><code>form-data; name="[data]"</code></pre>                   |
| `Content-Type`            | <pre><code>application/json; charset=UTF-8</code></pre>            |

### Response message
CIC는 HTTP 응답으로 클라이언트에 [Clova.Hello](/Develop/References/MessageInterfaces/Clova.md#Hello) 지시 메시지를 보냅니다. 이는 downchannel을 연결 설정이 완료되었음을 의미합니다.

### Status codes

| 상태 코드       | 설명                                  |
|---------------|--------------------------------------|
| 200 OK                    | Downchannel이 정상적으로 연결 및 설정되었을 때 반환되는 상태 코드입니다. 이후 CIC 주도(Cloud-initiated) 지시 메시지가 수신될 것입니다.        |
| 400 Bad Request           | 사용자 요청이 잘못된 형식으로 전달되었을 때 발생하는 오류입니다.                                                                       |
| 401 Unauthorized          | 사용자 인증에 실패했을 때 발생하는 오류입니다. 이때 [CLOVA access token을 갱신](/Develop/References/Client_Auth_API.md#RefreshCLOVAAccessToken)해야 합니다. |
| 429 Too Many Request      | 클라이언트가 동시 또는 매우 짧은 시간 사이에 <code>/v1/directives</code>로 2 건 이상의 연결을 요청할 때 발생하는 오류입니다. 클라이언트가 downchannel 구성을 요청할 때 한 번만 요청하도록 만들어야 합니다.  |
| 500 Internal Server Error | 서버 내부 오류입니다.                                                                                                      |

### Remarks
클라이언트는 CIC와 항시 하나의 downchannel을 유지하도록 해야 합니다. 만약, downchannel이 생성된 상태에서 <code>/v1/directives</code>으로 downchannel 생성하라는 추가 요청이 들어오면 기존 downchannel은 해제됩니다. 또한, 클라이언트가 동시 또는 매우 짧은 시간 사이에 <code>/v1/directives</code>로 2 건 이상의 연결을 시도하면 CIC는 클라이언트에 429 Too Many Request 오류 메시지를 반환합니다.

### Response example

{% raw %}
```
// 요청 성공
HTTP/2 200
Content-Type: multipart/related; boundary=b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba;
date: Fri, 04 Aug 2017 05:27:12 GMT

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
Content-Disposition: form-data; name="helloDirective-836d8db7-5e72-4fb2-9834-7c59291e1f8e"
Content-Type: application/json; charset=utf-8

{
    "directive": {
        "header": {
            "messageId": "2ca2ec70-c39d-4741-8a34-8aedd3b24760",
            "namespace": "Clova",
            "name": "Hello"
        },
        "payload": {}
    }
}
--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba

// 요청 실패
HTTP/2 400
content-type: multipart/related; boundary=883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca;
date: Fri, 04 Aug 2017 09:42:46 GMT

--883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca
Content-Disposition: form-data; name="exception-bde71903-dab4-46c5-9714-416cf12debf0"
Content-Type: application/json; charset=utf-8

{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "Exception",
      "messageId": "369b362b-258c-4104-bdf8-dc276548fe51"
    },
    "payload": {
      "code": 400,
      "description": "Could not decode multipart"
    }
  }
}
--883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca--
```
{% endraw %}

## 이벤트 메시지 전송 {#SendEvent}

```
POST /v1/events
```

클라이언트는 사용자의 음성 입력을 보내거나 클라이언트의 현재 상태 정보를 보낼 때 이벤트 메시지를 전송해야 합니다. HTTP 요청으로 CIC에 이벤트 메시지를 전송하며, HTTP 응답으로 지시 메시지를 받게 됩니다. 이벤트 메시지를 전송하거나 받은 지시 메시지를 처리하는 방법에 대한 자세한 내용은 [이벤트 메시지 전송하기](/Develop/Guides/Interact_with_CIC.md#SendEvent)와 [지시 메시지 처리하기](/Develop/Guides/Interact_with_CIC.md#HandleDirective)를 참조합니다.

### Request header

| Request header  | 설명                                                                |
|-----------------|--------------------------------------------------------------------|
| `Authorization`       | <p>획득한 CLOVA access token을 입력합니다.</p><pre><code>Bearer [CLOVA access token]</code></pre> |
| `Content-Type`        | <p><a href="#MultipartMessage">Multipart 메시지</a> 타입 및 경계 문구 선언:</p><pre><code>multipart/form-data; boundary=[boundary_term]</code></pre>  |
| `User-Agent`          | <p><a href="/Develop/Guides/Interact_with_CIC.md#UserAgentString">User agent string</a>을 입력합니다.</p><pre><code>User-Agent: [User-Agent string]</code></pre>  |
| `X-Clova-Device-Tags` | <p>클라이언트에 지정된 태그를 입력합니다. 태그가 하나 이상이면 쉼표(,)로 구분하여 나열합니다.</p><pre><code>X-Clova-Device-Tags: Tag1,Tag2,Tag3,...,TagN</code></pre> <div class="note"><p><strong>Note!</strong></p><p>태그는 클라이언트에 속성을 부여하는 것으로 특정 태그를 가진 클라이언트를 조회하거나 제어하는 등 다양한 목적으로 사용될 수 있습니다. 태그는 클라이언트에 속성을 부여하는 것으로 특정 태그를 가진 클라이언트를 조회하거나 제어하는 등 다양한 목적으로 사용될 수 있습니다. 태그에 대한 자세한 내용은 제휴 담당자에게 문의하시기 바랍니다.</p></div> |

### Request message header

| Request message header  | 설명                                                                |
|-------------------------|--------------------------------------------------------------------|
| `Content-Disposition`     | <pre><code>form-data; name="[data]"</code></pre>                   |
| `Content-Type`            | <ul><li>JSON 데이터: <code>application/json; charset=UTF-8</code></li><li>바이너리 음성 데이터: <code>application/octet-stream</code></li></ul> |

### Request message
사용자의 요청이나 클라이언트 정보를 CIC에 전달할 때 [이벤트 메시지](#Event)와 부가적인 음성 정보를 [multipart 메시지](#MultipartMessage)로 전달해야 합니다. 이벤트 메시지는 어떤 정보를 전달하느냐에 따라 그 내용과 구성이 달라질 수 있으며, 이를 [메시지 인터페이스](/Develop/References/Message_Interfaces.md)로 구분하고 있습니다.

### Request example

<pre><code>POST /v1/events HTTP/2
Host: {{ book.ServiceEnv.CICBaseURI }}
Accept: */*
User-Agent: MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
Authorization: Bearer XHapQasdfsdfFsdfasdflQQ7w
> Content-Length: 456
> Content-Type: multipart/form-data; boundary=920d6335ba920d6337a319f

--920d6335ba920d6337a319f
Content-Disposition: form-data; name="metadata"
Content-Type: application/json; charset=UTF-8

{
"context": [
  {
    "header": {
      "namespace": "Alerts",
      "name": "AlertsState"
    },
    "payload": {
      "allAlerts": [
        ...
      ],
      "activeAlerts": [
        ...
      ]
    }
  },
  ...
  {
    "header": {
      "namespace": "Speaker",
      "name": "VolumeState"
    },
    "payload": {
      "volume": 25,
      "muted": false
    }
  }
],
  "event": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "Recognize",
      "messageId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "dialogRequestId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "lang": "ko",
      "profile": "CLOSE_TALK",
      "format": "AUDIO_L16_RATE_16000_CHANNELS_1",
      "initiator": {
        "type": "WAKEWORD",
        "inputSource": "SELF",
        "payload": {
          "deviceUUID": "f003af9d-14c5-424b-b1f9-f0134bd0ed86",
          "wakeWord": {
            "name": "hay_clova",
            "confidence": 0.812312,
            "indices": {
              "startIndexInSamples": 0,
              "endIndexInSamples": 16000
            }
          }
        }
      }
    }
  }
}

--920d6335ba920d6337a319f
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[[ binary audio attachment ]]
--920d6335ba920d6337a319f--</code></pre>

### Response header

| Response header | 설명                                                                |
|-----------------|--------------------------------------------------------------------|
| `Content-Type`    | <p><a href="#MultipartMessage">Multipart 메시지</a> 타입 및 경계 문구 선언:</p><pre><code>multipart/form-data; boundary=[boundary_term]</code></pre> |

### Response message header

| Response message header | 설명                                                                |
|-------------------------|--------------------------------------------------------------------|
| `Content-Disposition`     | 내부적 사용을 위한 콘텐츠 메타 정보                                         |
| `Content-ID`              | 메시지 식별자<ul><li>UUID 형태</li><li>클라이언트는 지시 메시지의 <code>payload</code> 필드에 포함된 <code>cid:[UUID]</code> 값으로 처리해야 할 메시지를 식별할 수 있습니다.</li></ul> |
| `Content-Type`            | <ul><li>JSON 데이터: <code>application/json; charset=UTF-8</code></li><li>바이너리 음성 데이터: <code>application/octet-stream</code></li></ul>                     |

### Response message
CIC는 HTTP 응답으로 클라이언트에 동작을 수행하도록 명세한 [지시 메시지](#Directive)와 부가적인 음성 정보를 [multipart 메시지](#MultipartMessage)로 보냅니다. 지시 메시지에 어떤 정보를 담겼는지는 CIC가 내려준 지시 메시지에 따라 그 내용과 구성이 달라질 수 있으며, 이를 [메시지 인터페이스](/Develop/References/Message_Interfaces.md)로 구분하고 있습니다.

### Status codes

| 상태 코드       | 설명                                  |
|---------------|--------------------------------------|
| 200 OK                    | 클라이언트가 보낸 이벤트 메시지를 CIC가 정상적으로 수신했고, 클라이언트가 수행해야 할 지시 메시지가 1 개 이상 응답에 포함되어 있을 때 이 상태 코드가 반환됩니다. |
| 204 No Content            | 클라이언트가 보낸 이벤트 메시지를 CIC가 정상적으로 수신했고, 클라이언트가 수행해야 할 지시 메시지가 없을 때 이 상태 코드가 반환됩니다.                    |
| 400 Bad Request           | 이벤트 메시지가 잘못된 방법이나 올바르지 않은 형식으로 전달되었을 때 이 상태 코드가 반환됩니다.                                                  |
| 401 Unauthorized          | 사용자 인증에 실패했을 때 이 상태 코드가 반환됩니다. 이때 [CLOVA access token을 갱신](/Develop/References/Client_Auth_API.md#RefreshCLOVAAccessToken)해야 합니다. |
| 412 Precondition Failed   | 사용자 요청을 전송하기 위해 필요한 사전 조건(pre-condition)이 만족되지 않은 상황입니다. 주로 클라이언트가 [downchannel을 구성](#EstablishDownchannel)하지 않았거나 [downchannel을 구성할 때 만든 연결](/Develop/Guides/Interact_with_CIC.md#CreateConnection)로 이벤트 메시지를 전송하지 않았을 때 발생합니다.  |
| 500 Internal Server Error | 서버 내부 오류이면 이 상태 코드가 반환됩니다.                                                                                       |

### Response example

{% raw %}
```
// 요청 성공
HTTP/2 200
Content-Type: multipart/related; boundary=b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba;
date: Fri, 04 Aug 2017 05:27:12 GMT

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
Content-Disposition: form-data; name="speakDirective-836d8db7-5e72-4fb2-9834-7c59291e1f8e"
Content-Type: application/json; charset=utf-8

{
  "directive":{
    "header":{
      "namespace":"SpeechSynthesizer",
      "name":"Speak",
      "messageId":"dd4d463e-85a3-4514-927e-c90103c2dd02",
      "dialogRequestId":"4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload":{
      "format":"AUDIO_MPEG",
      "token":"e81f7dec-63fb-453d-8bd8-6944bed9a306",
      "ttsLang":"ko",
      "url":"cid:d329085c-379e-48aa-b871-7ecebdbe831d",
      "x-clova-pause-before":0
    }
  }
}

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
Content-Disposition: form-data; name="attachment-39b2f844-b168-4dc2-bea7-d5c249e446e3"
Content-ID: d329085c-379e-48aa-b871-7ecebdbe831d
Content-Type: application/octet-stream

[[ binary audio attachment ]]

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
Content-Disposition: form-data; name="renderTextDirective-b2c92b0f-27af-4f5c-b7e7-af7a270c464b"
Content-Type: application/json; charset=utf-8

{
  "directive":{
    "header":{
      "namespace":"Clova",
      "name":"RenderText",
      "messageId":"0fa1b36f-e86c-4979-9494-b00a162c4515",
      "dialogRequestId":"4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload":{
      "text":"만나서 반가워요"
    }
  }
}

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba--

// 요청 실패
HTTP/2 400
content-type: multipart/related; boundary=883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca;
date: Fri, 04 Aug 2017 09:42:46 GMT

--883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca
Content-Disposition: form-data; name="exception-bde71903-dab4-46c5-9714-416cf12debf0"
Content-Type: application/json; charset=utf-8

{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "Exception",
      "messageId": "369b362b-258c-4104-bdf8-dc276548fe51"
    },
    "payload": {
      "code": 400,
      "description": "Could not decode multipart"
    }
  }
}
--883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca--
```
{% endraw %}

## 메시지 포맷 {#CICMessageFormat}
CIC API에서 사용되는 메시지는 다음과 같이 구분되며, 각각 다른 포맷을 가집니다.

* [이벤트 메시지(Event)](#Event)
* [지시 메시지(Directive)](#Directive)
* [오류 메시지](#Error)

### 이벤트 메시지(Event) {#Event}
이벤트 메시지는 클라이언트에서 사용자가 발화한 음성 정보 또는 클라이언트 정보를 CIC에 전달할 때 사용됩니다. 대표적인 이벤트 메시지로 사용자의 음성 입력을 받아 인식을 요청하는 [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)가 있습니다.

#### Message structure
{% raw %}
```json
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlaybackState}}
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}}
  ],
  "event": {
    "header": {
      "namespace": {{string}},
      "name": {{string}},
      "dialogRequestId": {{string}},
      "messageId": {{string}}
    },
    "payload": {{object}}
  }
}
```
{% endraw %}

#### Message fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `context[]`                      | object array | CIC에 전달할 클라이언트의 상태 정보를 담고 있는 배열. 다음과 같은 [맥락 정보](/Develop/References/Context_Objects.md) 객체를 이 배열의 원소로 포함시킬 수 있습니다. 이벤트 메시지를 전송할 때 **맥락 정보로 나타낼 수 있는 모든 클라이언트의 상태 정보를 포함시키면 됩니다.**<ul><li><a href="/Develop/References/Context_Objects.md#AlertsState"><code>Alerts.AlertsState</code></a>: 알람 또는 타이머 상태 정보</li><li><a href="/Develop/References/Context_Objects.md#PlaybackState"><code>AudioPlayer.PlaybackState</code></a>: 최근 재생 정보</li><li><a href="/Develop/References/Context_Objects.md#DeviceState"><code>Device.DeviceState</code></a>: 클라이언트의 기기 정보</li><li><a href="/Develop/References/Context_Objects.md#Display"><code>Device.Display</code></a>: 클라이언트의 디스플레이 정보</li><li><a href="/Develop/References/Context_Objects.md#Location"><code>Clova.Location</code></a>: 클라이언트의 위치 정보</li><li><a href="/Develop/References/Context_Objects.md#SavedPlace"><code>Clova.SavedPlace</code></a>: 사전 정의 위치 정보</li><li><a href="/Develop/References/Context_Objects.md#VolumeState"><code>Speaker.VolumeState</code></a>: 스피커 정보</li></ul> | 필수 |
| `event`                        | object       | 이벤트 메시지의 헤더와 필요한 데이터(payload)를 가지고 있는 객체                                                                 | 필수 |
| `event.header`                 | object       | 이벤트 메시지의 헤더                                                                                                 | 필수 |
| `event.header.dialogRequestId` | string       | 대화 ID(Dialogue ID). 클라이언트는 [`SpeechRecognizer.Regcognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)와 [`TextRecognizer.Recognize`](/Develop/References/MessageInterfaces/TextRecognizer.md#Recognize) 이벤트 메시지를 전송할 때 반드시 [대화 ID를 생성](/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md#CreatingDialogueID)하여 이 필드에 입력해야 합니다.| 선택 |
| `event.header.messageId`       | string       | 메시지 ID. 개별 메시지를 구분하기 위해 사용하는 식별자입니다.                                                                 | 필수 |
| `event.header.name`            | string       | 이벤트 메시지의 API 이름                                                                                             | 필수 |
| `event.header.namespace`       | string       | 이벤트 메시지의 API 네임스페이스                                                                                       | 필수 |
| `event.payload`                | object       | 이벤트 메시지와 관련된 정보를 담고 있는 객체. 사용하는 [메시지 인터페이스](/Develop/References/Message_Interfaces.md)에 따라 payload 객체의 구성과 필드 값이 달라집니다. | 필수 |

#### Message example
{% raw %}
```json
{
  "context": [
    {
      "header": {
        "namespace": "Alerts",
        "name": "AlertsState"
      },
      "payload": {
        "allAlerts": [
          ...
        ],
        "activeAlerts": [
          ...
        ]
      }
    },
    ...
    {
      "header": {
        "namespace": "Speaker",
        "name": "VolumeState"
      },
      "payload": {
        "volume": 25,
        "muted": false
      }
    }
  ],
  "event": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "Recognize",
      "messageId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "dialogRequestId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "lang": "ko",
      "profile": "CLOSE_TALK",
      "format": "AUDIO_L16_RATE_16000_CHANNELS_1",
      "initiator": {
        "type": "WAKEWORD",
        "inputSource": "SELF",
        "payload": {
          "deviceUUID": "f003af9d-14c5-424b-b1f9-f0134bd0ed86",
          "wakeWord": {
            "name": "hay_clova",
            "confidence": 0.812312,
            "indices": {
              "startIndexInSamples": 0,
              "endIndexInSamples": 16000
            }
          }
        }
      }
    }
  }
}
```
{% endraw %}

#### See also
* [맥락 정보(Context)](/Develop/References/Context_Objects.md)
* [메시지 인터페이스](/Develop/References/Message_Interfaces.md)

### 지시 메시지(Directive) {#Directive}
지시 메시지는 클라이언트가 요청한 이벤트 메시지에 응답을 하거나 특정 조건에 의해 클라이언트로 정보를 전달할 때 사용됩니다. 이 지시 메시지는 주로 사용자의 음성이 인식된 후 그 의도를 클라이언트가 수행하도록 요청하기 위해 전달됩니다. 클라이언트는 지시 메시지에 담긴 의도에 맞게 결과를 사용자에게 제공하거나 작업을 처리해야 합니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>클라이언트가 지원하지 않기로 결정한 지시 메시지나 알 수 없는 지시 메시지를 수신했다면 해당 지시 메시지를 무시해야 합니다. 특히 주의해야 할 점은 클라이언트는 CIC로부터 multipart 메시지 형태로 한 번에 여러 개의 지시 메시지를 수신할 수 있는데 이때 지원하지 않거나 알 수 없는 지시 메시지가 포함되어 있으면 해당 지시 메시지만 무시해야 합니다.</p>
</div>

#### Message structure
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": {{string}},
      "name": {{string}},
      "dialogRequestId": {{string}},
      "messageId": {{string}}
    },
    "payload": {{object}}
  }
}
```
{% endraw %}


#### Message fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `directive`                        | object | 지시 메시지의 헤더와 필요한 데이터(`payload`)를 가지고 있는 객체                                                           | 항상     |
| `directive.header`                 | object | 지시 메시지의 헤더                                                                                                 | 항상     |
| `directive.header.dialogRequestId` | string | 대화 ID(Dialogue ID). 클라이언트 쪽에서 어떤 대화의 응답인지 파악하기 위해 사용됩니다. 지시 메시지가 [`SpeechRecognizer.Regcognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) 이벤트 메시지에 대한 응답이 아니면 이 필드가 지시 메시지에 포함되어 있지 않을 수도 있습니다.  | 조건부  |
| `directive.header.messageId`       | string | 메시지 ID. 개별 메시지를 구분하기 위해 사용하는 식별자입니다.                                                                | 항상     |
| `directive.header.name`            | string | 지시 메시지의 API 이름                                                                                             | 항상     |
| `directive.header.namespace`       | string | 지시 메시지의 API 네임스페이스                                                                                       | 항상     |
| `directive.payload`                | object | 지시 메시지와 관련된 정보를 담고 있는 객체. 사용하는 [메시지 인터페이스](/Develop/References/Message_Interfaces.md)에 따라 `payload` 객체의 구성과 필드 값이 달라집니다. | 항상     |

#### Message example
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "dialogRequestId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "format": "AUDIO_MPEG",
      "token": "b5fa5144-1e55-4193-8628-c70283083d9b",
      "ttsLang": "ko",
      "url": "cid:9d5d37a3-0e70-41a6-a671-e1a40c7ea4d8",
      "x-clova-pause-before": 0
    }
  }
}
```
{% endraw %}

#### See also
* [메시지 인터페이스](/Develop/References/Message_Interfaces.md)

### 오류 메시지 {#Error}
잘못된 방법이나 올바르지 않은 형식으로 [이벤트 메시지](#Event)를 전송하거나 서버측 내부 오류와 같은 이유로 CLOVA가 제대로 서비스를 제공할 수 없을 수 있습니다. 이때 CIC는 오류 메시지를 클라이언트로 전송합니다. 클라이언트는 오류 메시지를 보고 그에 상응하는 UX/UI를 제공해야 합니다.

#### Message structure
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "Exception",
      "messageId": {{string}}
    },
    "payload": {
      "code": {{number}},
      "description": {{string}}
    }
  }
}
```
{% endraw %}

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>오류 메시지는 <a href="#Directive">지시 메시지</a>와 비슷한 구조의 메시지로 구성됩니다.</p>
</div>

#### Message fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `directive`                        | object | 오류 메시지의 헤더와 필요한 데이터(`payload`)를 가지고 있는 객체       | 항상 |
| `directive.header`                 | object | 오류 메시지의 헤더                                             | 항상 |
| `directive.header.messageId`       | string | 메시지 ID. 개별 메시지를 구분하기 위해 사용하는 식별자입니다.            | 항상 |
| `directive.header.name`            | string | 오류 메시지의 이름. `"Exception"`으로 고정됩니다.                | 항상 |
| `directive.header.namespace`       | string | 오류 메시지의 네임스페이스. `"System"`으로 고정됩니다.             | 항상 |
| `directive.payload`                | object | 오류와 관련된 정보를 담고 있는 객체                                | 항상 |
| `directive.payload.code`           | number | 오류 코드. 해당 메시지의 HTTP 응답 코드와 같은 값을 가집니다.           | 항상 |
| `directive.payload.description`    | string | 오류 메시지                                                  | 항상 |

#### Error code reference

| 오류 코드 | 설명                             |
|---------|---------------------------------|
| 400     | [이벤트 메시지](#Event)가 잘못된 방법이나 올바르지 않은 형식으로 전달되었을 때 발생하는 오류입니다.                                                |
| 401     | 사용자 인증에 실패했을 때 발생하는 오류입니다. 이때 [CLOVA access token을 갱신](/Develop/References/Client_Auth_API.md#RefreshCLOVAAccessToken)해야 합니다. |
| 500     | 서버 내부 오류입니다.                                                                                |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>위 오류 코드는 응답 메시지의 HTTP 상태 코드(status codes)와 같습니다. 또한, 오류 코드는 계속 추가될 수 있습니다.</p>
</div>

### Message example
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "Exception",
      "messageId": "369b362b-258c-4104-bdf8-dc276548fe51"
    },
    "payload": {
      "code": 400,
      "description": "Could not decode multipart"
    }
  }
}
```
{% endraw %}
