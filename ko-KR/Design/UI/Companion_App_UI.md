# 연동 앱 UI

연동 앱(Companion app)은 사용자 인증과 파트너사 기기 설정을 할 수 있는 UI를 제공할 수 있습니다. 이 페이지는 각 UI에 대해 설명합니다.

* [사용자 인증 UI](#UserAuthUI)
  * [세로 화면 예시](#PortraitVersionExamples)
  * [가로 화면 예시](#LandscapeVersionExamples)
* [파트너사 기기 설정 UI 예시](#PartnerDevicesSettingsUIExamples)
* [위치 설정 UI 제공](#LocationSettingsUI)
* [UI 리소스](#UIResources)

## 사용자 인증 UI {#UserAuthUI}

연동 앱에서 [사용자를 인증](/Develop/Guides/Interact_with_CIC.md#ObtainCLOVAAccessToken)할 때 {{ book.ServiceEnv.TargetServiceForClientAuth }} 아이디로 로그인 기능을 사용해야 합니다. {{ book.ServiceEnv.TargetServiceForClientAuth }} ID로 로그인하기 기능은 <a href="{{ book.ServiceEnv.LoginAPIofTargetService }}" target="_blank">관련 문서</a>를 참고하여 제공하십시오. {{ book.ServiceEnv.TargetServiceForClientAuth }} 아이디로 로그인 기능을 구현했다면 아래와 같은 흐름으로 UI를 제공하는지 확인하십시오.

![](/Design/UI/CompanionApp/NAVER_Login_Flow_For_Companion_App.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>사용자가 {{ book.ServiceEnv.TargetServiceForClientAuth }} ID로 로그인하면 <a href="/Develop/Guides/Interact_with_CIC.md#ObtainCLOVAAccessToken">CLOVA access token을 발급</a>받아야 합니다. 또, 사용자가 원할 경우 사용자 인증을 해제할 수 있으며, 이때 <a href="/Develop/References/Client_Auth_API.md#DeleteCLOVAAccessToken">CLOVA access token을 삭제</a>하거나 관련 기능을 제공해야 합니다. 이에 대한 내용음 제휴 담당자에게 문의합니다.</p>
</div>

### 세로 화면 예시 {#PortraitVersionExamples}

연동 앱이 스마트 폰과 같은 모바일 기기에서 동작한다면 사용자 인증 UI는 모바일 기기의 세로 화면 형태(portrait screen)에 맞게 제공되어야 합니다. 다음은 사용자 인증 UI가 모바일 플랫폼별, 인증 프로세스 진행 상태별로 세로 화면에 제공된 예를 보여줍니다.

![](/Design/UI/CompanionApp/NAVER_Login_UI_For_Portrait_Screen.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>사용자 인증 UI의 상세한 내용은 <a href="CompanionApp/Companion_App_UI.sketch.zip" target="_blank">이 파일</a>을 참고하십시오. <a href="https://www.sketch.com/" target="_blank">Sketch 앱</a>으로 열람할 수 있습니다.</p>
</div>

### 가로 화면 {#LandscapeVersionExamples}

연동 앱이 가로로 긴 화면 형태를 가진 기기에서 동작한다면 사용자 인증 UI는 기기의 가로 화면 형태(landscape screen)에 맞게 제공되어야 합니다. 다음은 사용자 인증 UI가 인증 프로세스 진행 상태별, 색상 버전별로 가로 화면에 제공된 예를 보여줍니다.

![](/Design/UI/CompanionApp/NAVER_Login_UI_For_Landscape_Screen.png)

## 파트너사 기기 설정 UI 예시 {#PartnerDevicesSettingsUIExamples}

연동 앱에서 파트너 기기 설정 UI를 제공할 때 아래와 같은 예시를 참고하여 UI를 제공하십시오.

![](/Design/UI/CompanionApp/UI_Examples_For_Partner_Devices.png)

## 위치 설정 UI 제공 {#LocationSettingsUI}

사용자의 위치 정보를 기반으로 제공되는 서비스는 위치 정보가 없으면 제대로 작동하지 않습니다. 만약, GPS 기능이 탑재되지 않은 기기라면, 사용자가 직접 위치 정보를 입력할 수 있도록 UI를 제공할 수 있습니다. 주소 검색과 같은 UI를 이용해 사용자에게 주소를 입력받고 해당 주소의 위치 정보(경도, 위도 값)를 [이벤트 메시지](/Develop/Guides/Interact_with_CIC.md#SendEvent)의 [맥락 정보](/Develop/References/Context_Objects.md)에 포함하여 CLOVA에 전달해야 합니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>입력 받는 위치 정보가 정확할수록 제공되는 위치 기반 서비스 품질이 향상됩니다.</p>
</div>

## UI 리소스 {#UIResources}

연동 앱을 만들 때 UI 요소는 CLOVA가 제공하는 UI 리소스를 사용하거나 권장하는 가이드라인을 따라야 합니다. 다음과 같은 문서를 참고하십시오.

* [로고](/Design/Brand/Logo.md)
* [심볼](/Design/Brand/Symbol.md)
* [배지](/Design/Brand/Badge.md)
* [타이포그래피(폰트)](/Design/DesignFoundation/Typography.md)
* [아이콘](/Design/DesignFoundation/Icons.md)
* [콘텐츠 아이콘](/Design/DesignFoundation/Content_Icons.md)