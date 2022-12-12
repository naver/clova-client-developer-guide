## 이벤트 메시지 전송하기 {#SendEvent}
클라이언트는 CIC로 [이벤트 메시지](/Develop/References/CIC_API.md#Event)를 전송할 수 있습니다. 이벤트 메시지는 클라이언트 요청을 CIC로 전달할 때 사용됩니다. 메시지는 JSON 포맷의 이벤트 메시지뿐만 아니라 사용자 음성 입력을 [multipart 형태의 메시지](/Develop/References/CIC_API.md#MultipartMessage)로 전송합니다.

클라이언트에서 사용자의 음성 데이터를 CIC로 보낼 때 [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) 이벤트 메시지를 사용합니다. 다음은 `SpeechRecognizer.Recognize`를 이용해 CIC로 이벤트 메시지를 전송하는 방법을 설명합니다.

<ol>
  <li>이벤트 메시지 전송을 위해 클라이언트에 <a href="#RequiredLibrary">HTTP/2용 라이브러리</a>와 <a href="#Authorization">CLOVA access token</a>을 준비하십시오.</li>
  <li>다음과 같이 <a href="/Develop/References/CIC_API.md#SendEvent">CIC API</a>에 맞게 HTTP 헤더를 준비한 값으로 채우고 HTTP/2용 라이브러리를 이용해 요청을 전달하십시오.
    <pre><code>:method = POST
:scheme = https
:path = /v1/events
User-Agent: MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
Authorization: Bearer XHapQasdfsdfFsdfasdflQQ7w-Example
Content-Type: multipart/form-data; boundary=Boundary-Text</code></pre>
  </li>
  <li>이벤트 메시지에 포함시킬 <a href="/Develop/Guides/Manage_Dialog_ID_And_Handle_Tasks.md">대화 ID</a>(<code>dialogRequestId</code>)와 메시지 ID(<code>messageId</code>)를 UUID 포맷으로 생성하십시오. 추후 <a href="#ManageMessageQ">메시지 큐</a>에서 지시 메시지를 선별하는데 이용해야 합니다.</li>
  <li>첫 번째 메시지 파트에 <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize"><code>SpeechRecognizer.Recognize</code></a> API 스펙에 맞게 작성된 JSON 포맷의 이벤트 메시지와 메시지 헤더를 함께 입력한 후 CIC로 전송하십시오.
    <pre><code>--Boundary-Text
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
--Boundary-Text--</code></pre>
  </li>
  <li>두 번째 메시지 파트부터 사용자가 입력한 음성 데이터를 200 밀리초(milliseconds) 간격으로 끊어서 전송하십시오. 데이터 형식 변경에 따라 메시지 헤더도 아래와 같이 작성합니다.
    <pre><code>--Boundary-Text
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[[ binary audio attachment ]]
--Boundary-Text--</code></pre>
  </li>
  <li>CIC로부터 <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#StopCapture"><code>SpeechRecognizer.StopCapture</code></a> 지시 메시지가 전달될 때까지 음성 데이터를 계속 전송하십시오. 전송이 완료되면 CIC로부터 HTTP 응답 메시지가 수신됩니다.</li>
</ol>

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p><a href="/Develop/References/MessageInterfaces/TextRecognizer.md#Recognize"><code>TextRecognizer.Recognize</code></a>를 사용하면 사용자의 텍스트 입력을 처리할 수도 있습니다.</p>
</div>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>이벤트 메시지를 보낼 때 반드시 <a href="#CreateConnection">Downchannel을 구성할 때 만든 연결</a>로 CIC 이벤트 메시지를 전송해야 합니다.</p>
</div>
