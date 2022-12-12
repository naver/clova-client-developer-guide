## 지시 메시지 처리하기 {#HandleDirective}
지시 메시지는 CIC로부터 수신되며 클라이언트에 특정 동작을 요구합니다. 지시 메시지는 [이벤트 메시지](#SendEvent)의 응답이나 downchannel을 통해 전달됩니다. 응답은 주로 [multipart 메시지](/Develop/References/CIC_API.md#MultipartMessage) 형태이고, JSON 포맷 형태의 [지시 메시지](/Develop/References/CIC_API.md#Directive)가 첫 번째 메시지로 먼저 수신된 후 [CIC API](/Develop/References/CIC_API.md)에 따라 주로 다음과 같이 부가적인 정보(음성 데이터, 콘텐츠 정보)를 담은 메시지가 추가로 수신될 수 있습니다.

| 콘텐츠 유형            | 설명                                             |
|---------------------|-------------------------------------------------|
| 음성 데이터            | 클라이언트 스피커를 통해 출력하려는 음성 정보                  |
| JSON 형식의 콘텐츠 정보 | <ul><li>클라이언트의 화면을 통해 표시하려는 데이터(<a href="/Develop/References/Content_Templates.md">content template</a> 참조)</li><li>음악과 같이 재생하려는 콘텐츠의 위치와 인증 정보가 담긴 데이터</li></ul> |

클라이언트는 다음과 절차와 같이 지시 메시지를 처리해야 합니다.

<ol>
  <li>특정 이벤트 메시지의 응답이나 downchannel을 통해 전달받는 지시 메시지를 미리 정해 둔 <a href="#ManageMessageQ">메시지 큐</a>에 저장하십시오.</li>
  <li>수신된 <a href="/Develop/References/CIC_API.md#Directive">지시 메시지</a>의 메시지 헤더를 분석(parsing)하십시오.<br />
  일반적으로 <code>dialogRequestId</code>는 사용자 요청, <code>namespace</code>와 <code>name</code>은 <a href="/Develop/References/CIC_API.md">API</a>를 구분하는데 사용합니다. 다음은 수신된 지시 메시지의 예입니다.
    <pre><code>{
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
}</code></pre>
  </li>
  <li>수신한 지시 메시지의 <a href="/Develop/Guides/Manage_Dialog_ID_And_Handle_Tasks.md">대화 ID</a>(<code>dialogRequestId</code>)가 클라이언트가 보관하고 있는 대화 ID와 대응되는지 확인하십시오.
    <ul>
      <li><strong>클라이언트가 보관하고 있는 마지막 대화 ID와 일치하면</strong>, API 레퍼런스에 따라 필요한 동작을 수행하십시오. 지시 메시지의 <code>payload</code>에 포함된 <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak"><code>cid</code> 값을 이용</a>하여 클라이언트 동작에 필요한 부가 정보(음성 데이터)를 <a href="#ManageMessageQ">메시지 큐</a>에서 선별해 낼 수 있습니다. <code>cid</code>는 다음과 같이 multipart 메시지 중 한 부분으로 전달된 음성 데이터의 <code>Content-ID</code> 메시지 헤더를 의미합니다.
        <pre><code>--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
Content-Disposition: form-data; name="attachment-39b2f844-b168-4dc2-bea7-d5c249e446e3"
Content-ID: d329085c-379e-48aa-b871-7ecebdbe831d
Content-Type: application/octet-stream

[[ binary audio attachment ]]

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba</code></pre>
      </li>
      <li><strong>클라이언트가 보관하고 있는 대화 ID와 일치하지 않으면</strong>, 해당 지시 메시지와 관련된 모든 메시지를 무시하고 큐에서 제거하십시오.</li>
    </ul>
  </li>
</ol>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>클라이언트가 지원하지 않기로 결정한 지시 메시지나 알 수 없는 지시 메시지를 수신하게 되면 해당 지시 메시지를 무시해야 합니다. 특히 주의해야 할 점은 클라이언트는 CIC로부터 multipart 메시지 형태로 한 번에 여러 개의 지시 메시지를 수신할 수 있는데 이때 지원하지 않거나 알 수 없는 지시 메시지가 포함되어 있으면 해당 지시 메시지만 무시해야 합니다.</p>
</div>
