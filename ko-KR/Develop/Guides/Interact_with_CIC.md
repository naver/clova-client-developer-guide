# CIC 연동하기
사용자가 CLOVA 서비스를 사용하려면 클라이언트(앱이나 기기)가 CIC API를 이용해서 사용자의 요청이나 상황 정보를 CLOVA로 전달해야 합니다. 따라서 클라이언트가 CIC와 어떻게 연결되어야 하고 송수신해야 할 메시지가 무엇이며, 그리고 메시지 송수신 간에 어떤 작업을 처리해야 하는지 이해해야 합니다.

이 문서는 다음과 같은 순서로 클라이언트 개발자가 알아야 할 내용을 전달하고 있습니다.

1. [사전 준비사항](#Preparation)
2. [CIC 연결하기](#ConnectToCIC)
3. [이벤트 메시지 전송하기](#SendEvent)
4. [지시 메시지 처리하기](#HandleDirective)
5. [메시지 큐 관리하기](#ManageMessageQ)

{% include "/Develop/Guides/InteractWithCIC/Preparation.md" %}

{% include "/Develop/Guides/InteractWithCIC/Connect_to_CIC.md" %}

{% include "/Develop/Guides/InteractWithCIC/Send_Event.md" %}

{% include "/Develop/Guides/InteractWithCIC/Handle_Directive.md" %}

{% include "/Develop/Guides/InteractWithCIC/Manage_Message_Q.md" %}
