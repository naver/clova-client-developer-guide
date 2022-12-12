<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

<!-- Start of the shared content: Glossary -->

# 용어 및 약어

<div class="note">
  <p><strong>Note!</strong></p>
  <p>이 페이지는 계속 업데이트되고 있습니다.</p>
</div>

<dl>
  <dt><strong>CEK</strong></dt>
  <dd><a href="#CEK">CLOVA Extensions Kit</a>의 약어</dd>
  <dt><strong>CIC</strong></dt>
  <dd><a href="#CIC">CLOVA Interface Connect</a>의 약어</dd>
  <dt id="CICAPI"><strong>CIC API</strong></dt>
  <dd>CIC가 클라이언트에 제공하는 REST API로 클라이언트는 CIC API를 사용하여 CLOVA와 정보를 교환할 때 사용됩니다.</dd>
  <dt id="CLOVA"><strong>CLOVA</strong></dt>
  <dd><a target="_blank" href="https://clova.ai">CLOVA</a>는 {{ book.ServiceEnv.OrientedService }}가 개발 및 서비스하고 있는 인공지능 플랫폼입니다. CLOVA 사용자의 음성이나 이미지를 인식하고 이를 분석하여 사용자가 원하는 정보나 서비스를 제공합니다. 3rd party 개발자는 CLOVA가 가진 기술을 활용하여 인공 지능 서비스를 제공하는 기기 또는 가전 제품을 만들거나 보유하고 있는 콘텐츠나 서비스를 CLOVA를 통해 사용자에게 제공할 수 있습니다.</dd>
  <dt id="CLOVAAccessToken"><strong>CLOVA access token</strong></dt>
  <dd>클라이언트가 <a href="#CIC">CLOVA Interface Connect</a>로 <a href="#Event">이벤트 메시지</a>를 보낼 때 CLOVA가 클라이언트를 인증하는 수단입니다. 자세한 내용은 <a href="/Develop/Guides/Interact_with_CIC.md#ObtainCLOVAAccessToken">CLOVA access token 획득하기</a> 문서를 참조합니다.</dd>
  <dt id="CLOVADeveloperConsole"><strong>CLOVA developer console</strong></dt>
  <dd>CLOVA 플랫폼과 연동하는 클라이언트 기기나 <a href="#CLOVAExtension">CLOVA extension</a>을 개발하는 개발자에게 다음과 같은 내용을 제공하는 <a target="_blank" href="{{ book.ServiceEnv.DeveloperConsoleURI }}">웹 도구</a>입니다.
    <ul>
      <li>클라이언트 기기 등록</li>
      <li>클라이언트 인증 정보 제공</li>
    </ul>
    <div class="note">
      <p><strong>Note!</strong></p>
      <p>CLOVA client용 CLOVA developer console는 현재 개발 중에 있습니다.</p>
    </div>
  </dd>
  <dt id="CLOVAExtension"><strong>CLOVA extension</strong></dt>
  <dd>음악, 쇼핑, 금융 같은 외부 서비스(3rd party service)나 집 안 IoT 기기의 원격 제어와 같이 사용자가 CLOVA를 통해 더 많은 서비스와 기능을 경험할 수 있도록 CLOVA에 확장된 기능을 제공하는 애플리케이션입니다. 짧게 extension이라 부르기도 하며, CLOVA 플랫폼은 현재 다음과 같은 두 종류의 extension을 지원공하고 있습니다. 일반 사용자에게는 "Skill"이라는 이름으로 제공되고 있습니다.
    <ul>
      <li><a href="#CustomExtension">Custom extension</a></li>
      <li><a href="#CLOVAHomeExtension">CLOVA Home extension</a></li>
    </ul>
  </dd>
  <dt id="CEK"><strong>CLOVA Extensions Kit (CEK)</strong></dt>
  <dd>CLOVA extension을 개발 및 배포할 때 필요한 도구와 인터페이스를 제공하는 플랫폼으로 CLOVA와 extension 사이의 커뮤니케이션을 지원합니다.</dd>
  <dt id="CLOVAHomeExtension"><strong>CLOVA Home extension</strong></dt>
  <dd>IoT 기기 제어 서비스를 제공하기 위한 <a href="#CLOVAExtension">extension</a>입니다. 자세한 내용은 <a target="_blank" href="{{ book.DocMeta.CLOVAHomeExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Clova_Home_Extension.{{ book.DocMeta.FileExtensionForExternalLink }}">CLOVA Home extension 만들기</a> 문서를 참조합니다.</dd>
  <dt id="CIC"><strong>CLOVA Interface Connect(CIC)</strong></dt>
  <dd>인공 지능 비서 서비스를 제공하려는 PC/모바일용 앱, 모바일 또는 가전 기기의 클라이언트에 CLOVA와 연동할 수 있는 인터페이스를 제공하는 플랫폼입니다. 자세한 내용은 <a href="/Develop/CIC_Overview.md">CIC 개요</a> 문서를 참조합니다.</dd>
  <dt id="CLOVAApp"><strong>CLOVA 앱</strong></dt>
  <dd>{{ book.ServiceEnv.OrientedService }}가 개발하여 iOS나 Android 플랫폼으로 배포한 CLOVA 앱입니다. CLOVA에 명령을 내릴 수 있을 뿐만 아니라 클라이언트 기기를 등록하고 관리할 수 있는 앱입니다.</dd>
  <dt id="ContentTemplate"><strong>Content template</strong></dt>
  <dd>CIC를 통해 전달되는 콘텐츠 정보를 일정 범주에 맞게 정형화한 것입니다. 자세한 내용은 <a href="/Develop/References/Content_Templates.md">content template</a> 문서를 참조합니다.</dd>
  <dt id="ContextObjects"><strong>Context objects</strong></dt>
  <dd>클라이언트의 현재 <a href="#Context">맥락 정보</a>를 표현하는 객체입니다. 자세한 내용은 <a href="/Develop/References/Context_Objects.md">맥락 정보(Context)</a> 문서를 참조합니다.</dd>
  <dt id="CustomExtension"><strong>Custom extension</strong></dt>
  <dd>임의의 확장된 기능을 제공하는 <a href="#CLOVAExtension">extension</a>입니다. Custom extension을 사용하면 음악, 쇼핑, 금융과 같은 외부 서비스의 기능을 제공할 수 있습니다. 자세한 내용은 <a target="_blank" href="{{ book.DocMeta.CLOVACustomExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Custom_Extension.{{ book.DocMeta.FileExtensionForExternalLink }}">CLOVA custom extension 만들기</a> 문서를 참조합니다.</dd>
  <dt id="Downchannel"><strong>Downchannel</strong></dt>
  <dd>Downchannel은 클라이언트가 <a href="#CIC">CLOVA Interface Connect</a>로부터 지시 메시지를 받을 때 사용되는 <a href="#HTTP2">HTTP/2</a> 스트림입니다. 자세한 내용은 <a href="/Develop/Guides/Interact_with_CIC.md#ConnectToCIC">CIC 연결하기</a> 문서를 참조합니다.</dd>
  <dt id="Extension"><strong>Extension</strong></dt>
  <dd><a href="#CLOVAExtension">CLOVA extension</a>의 다른 표현</dd>
  <dt id="HTTP2"><strong>HTTP/2</strong></dt>
  <dd>HTTP 프로토콜의 두 번째 버전이다. <a target="_blank" href="https://en.wikipedia.org/wiki/SPDY">SPDY</a>에 기반하고 있으며, 국제 인터넷 표준화 기구(IETF)에서 개발되고 있다. 1997 년 RFC 2068로 표준이 된 HTTP 1.1을 개선한 것으로, 2014 년 12 월 표준안 제안(Proposed Standard)으로 고려되어, 2015 년 2 월 17 일 IESG에서 제안안으로 승인되었다. 2015 년 5 월, <a href="https://tools.ietf.org/html/rfc7540" target="_blank">RFC 7540</a>]로 공개되었다.</dd>
  <dt><strong>OAuth 2.0</strong></dt>
  <dd>접근 권한을 위임하기 위한 공개 표준으로 인터넷 사용자가 다른 웹 서비스나 응용 프로그램에 사용자 계정에 접근할 수 있는 권한을 부여하는 규약입니다. CLOVA 플랫폼에서는 클라이언트가 <a href="#CLOVAAccessToken">CLOVA access token</a>을 획득하거나 사용자가 특정 extension을 사용 시 자신의 계정을 연결할 때 사용됩니다. 자세한 내용은 <a target="_blank" href="https://tools.ietf.org/html/rfc6749">https://tools.ietf.org/html/rfc6749</a>를 참고합니다.</dd>
  <dt id="DialogID"><strong>대화 ID</strong></dt>
  <dd>대화 ID는 사용자가 새로운 발화를 시작할 때마다 생성되며, 클라이언트가 <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">Recognize</a> <a href="#Event">이벤트 메시지</a>를 <a href="#CIC">CLOVA Interface Connect</a>에 전달할 때 포함됩니다. 대화 ID는 서버측 응답을 내려줄 때 어떤 이벤트 메시지에 대한 응답인지 연결할 때 사용되며, <a href="#Directive">지시 메시지</a>에도 포함됩니다. 클라이언트는 지시 메시지에 포함된 대화 ID를 보고 어떤 이벤트 메시지의 응답인지 판단해야 하며, 만약 클라이언트가 현재 가지고 있는 대화 ID와 지시 메시지의 대화 ID가 다르면 수신한 지시 메시지를 무시해야 합니다. 자세한 내용은 <a href="/Develop/Guides/Manage_Dialog_ID_And_Handle_Tasks.md">대화 모델</a> 문서를 참조합니다.</dd>
  <dt id="Context"><strong>맥락 정보(Context)</strong></dt>
  <dd>맥락 정보(Context)는 클라이언트의 다양한 상태 정보를 의미하며 <a href="#ContextObjects">context objects</a>로 표현됩니다. 자세한 내용은 <a href="/Develop/References/Context_Objects.md">맥락 정보(Context)</a> 문서를 참조합니다.</dd>
  <dt id="#MessageID"><strong>메시지 ID</strong></dt>
  <dd>메시지 ID는 개개의 메시지를 구분하기 위한 식별자이며, <a href="#Event">이벤트 메시지</a>와 <a href="#Directive">지시 메시지</a>는 모두 개개의 메시지 ID를 가집니다.</dd>
  <dt id="Event"><strong>이벤트 메시지 (Event)</strong></dt>
  <dd>이벤트 메시지는 클라이언트에서 <a href="#CIC">CLOVA Interface Connect</a>로 전달하는 메시지이며, 사용자 요청(음성 입력)을 전달하거나 클라이언트의 상태 값이 변경된 것을 알릴 때 이 메시지를 전송합니다.</dd>
  <dt id="Directive"><strong>지시 메시지 (Directive)</strong></dt>
  <dd>지시 메시지는 <a href="#CIC">CLOVA Interface Connect</a>가 클라이언트의 행동을 제어하도록 명세한 메시지입니다. 지시 메시지는 클라이언트가 요청한 이벤트 메시지에 응답을 하거나 특정 조건에 의해 클라이언트로 정보를 전달할 때 사용됩니다.</dd>
  <dt id="CLOVAAuthAPI"><strong>클라이언트 인증 API</strong></dt>
  <dd>클라이언트가 <a href="#CLOVAAccessToken">CLOVA access token</a>을 획득하기 위해 사용해야 하는 API입니다. 자세한 내용은 <a href="/Develop/References/Client_Auth_API.md">클라이언트 인증 API</a> 문서를 참조합니다.</dd>
  <dt id="ClientCredentialInfo"><strong>클라이언트 인증 정보</strong></dt>
  <dd><a href="#CLOVADeveloperConsole">CLOVA developer console</a>를 통해 클라이언트를 등록하고 획득한 인증 정보이며, <a href="#CLOVAAccessToken">CLOVA access token</a>을 획득하는데 사용됩니다. 자세한 내용은 <a href="/Develop/Guides/Interact_with_CIC.md#ObtainCLOVAAccessToken">CLOVA access token 획득하기</a> 문서를 참조합니다.</dd>
</dl>

<!-- End of the shared content -->