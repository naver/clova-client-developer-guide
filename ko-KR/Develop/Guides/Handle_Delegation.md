# 위임된 사용자 요청 처리하기

사용자는 CLOVA 앱을 사용할 때 요청에 대한 처리 결과를 CLOVA 앱이 아닌 사용자가 가진 다른 클라이언트 기기가 결과를 받아서 처리하도록 지정할 수 있습니다. 예를 들면, CLOVA 앱으로 특정 아티스트의 노래를 재생해달라고 할 때 노래가 사용자의 스마트 폰이 아닌 CLOVA 프렌즈 스피커에서 재생되도록 요청할 수 있습니다. 이런 동작을 가리켜 CLOVA 앱이 사용자의 요청을 다른 클라이언트 기기로 위임했다고 이야기합니다.

CLOVA 앱이 사용자 요청 처리를 위임하면 위임을 받게되는 클라이언트가 downchannel을 통해 불시에 지시 메시지를 받게 됩니다. 다음은 사용자가 요청을 위임할 때의 동작 구조를 설명합니다. **아래 설명 중 위임받는 클라이언트의 동작을 구현해야 합니다.**

![](/Develop/Assets/Images/CIC_Handle_Event_Delegation.svg)

<ol>
  <li>CLOVA 앱은 사용자 요청을 CLOVA에 전송할 때 다른 클라이언트 기기로 위임을 요청합니다.</li>
  <li>CLOVA는 요청 처리를 위임받은 클라이언트 기기에 다음과 같은 <a href="/Develop/References/MessageInterfaces/Clova.md#HandleDelegatedEvent"><code>Clova.HandleDelegatedEvent</code></a> 지시 메시지를 <a href="/Develop/Guides/Interact_with_CIC.md#CreateConnection">downchannel</a>로 전송합니다.
    <pre><code>{
  "directive": {
    "header": {
      "messageId": "b17df741-2b5b-4db4-a608-85ecb1307b33",
      "namespace": "Clova",
      "name": "HandleDelegatedEvent"
    },
    "payload": {
      "delegationId": "99e86204-710a-4e94-b949-a763e78348a7"
    }
  }
}</code></pre>
  </li>
  <li>클라이언트는 위임된 요청의 처리 결과를 CLOVA로 부터 받기 위해 <a href="/Develop/References/MessageInterfaces/Clova.md#ProcessDelegatedEvent"><code>Clova.ProcessDelegatedEvent</code></a> 이벤트 메시지를 CLOVA에 전송해야 합니다.<br />
  이때, 2 번 단계에서 받은 <code>delegationId</code> 필드의 값을 그대로 <code>payload</code> 필드에 입력해야 합니다.
    <pre><code>{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Clova",
      "name": "ProcessDelegatedEvent",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "delegationId": "99e86204-710a-4e94-b949-a763e78348a7"
    }
  }
}</code></pre>
  </li>
  <li>CLOVA는 클라이언트에 <a href="/Develop/References/MessageInterfaces/Clova.md#ProcessDelegatedEvent"><code>Clova.ProcessDelegatedEvent</code></a> 이벤트 메시지의 응답으로 사용자가 위임할 때 했던 요청의 처리 결과를 돌려줍니다.</li>
  <li>클라이언트는 일반적인 <a href="/Develop/Guides/Interact_with_CIC.md#HandleDirective">지시 메시지</a>를 처리하듯이 응답으로 받은 지시 메시지를 처리하면 됩니다.</li>
</ol>