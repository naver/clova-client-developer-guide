# 문서 정보
이 문서는 CLOVA 클라이언트를 개발할 수 있도록 디자인 가이드라인과 CIC 플랫폼에 대한 개발 가이드 및 API 레퍼런스를 제공합니다. 대상 독자는 CIC를 사용하여 CLOVA 서비스와 연동되는 전자 기기, 앱을 개발하려는 클라이언트 개발자입니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>CLOVA는 개발이 계속 진행되고 있습니다. 따라서, 문서의 내용은 언제든지 변경될 수 있습니다.</p>
</div>

## 연락처
문서와 관련하여 궁금한 사항은 지정된 CLOVA 제휴 담당자나 <a href="{{ book.ServiceEnv.DeveloperCenterForumURI }}" target="_blank">{{ book.ServiceEnv.DeveloperCenterName }} 포럼</a>에 문의합니다.

## 문서 변경 이력

이 문서의 변경 이력은 다음과 같습니다.

<table>
  <thead>
    <tr>
      <th style="width:12%">배포 일자</th><th style="width:88%">이력 사항</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2021-09-24</td>
      <td>
        <ul>
          <li>문서 내 용어, 표현 등을 검수 후 반영</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2021-02-03</td>
      <td>
        <ul>
          <li>CLOVA inside <a href="/Design/Brand/Badge.md">배지</a>에 대한 사용 가이드라인을 변경함.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2021-01-07</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CIC_API.md">CIC API</a>의 HTML 태그에 클라이언트 속성을 부여할 수 있도록 X-Clova-Device-Tags 요청 헤더를 추가함.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-11-10</td>
      <td>
        <ul>
          <li>기기 설정 중 일부 내용을 CLOVA가 제공하는 설정을 적용할 수 있도록 관련 내용을 문서에 추가함
            <ul>
              <li><a href="/Develop/Guides/Handle_Settings.md#ApplySettingsProvidedByCLOVA">CLOVA 제공 설정 적용하기</a></li>
              <li><a href="/Develop/References/MessageInterfaces/Settings.md#RequestVersionSpec">RequestVersionSpec</a> 이벤트 메시지, <a href="/Develop/References/MessageInterfaces/Settings.md#UpdateVersionSpec">UpdateVersionSpec</a> 지시 메시지</li>
            </ul>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-09-24</td>
      <td>
        <ul>
          <li>CLOVA 앱이나 연동앱에서 사용자 계정에 등록된 클라이언트 기기 목록 정보를 조회할 수 있도록 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#RequestDeviceList">DeviceControl.RequestDeviceList</a> 이벤트 메시지와 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#RenderDeviceList">DeviceControl.RenderDeviceList 지시 메시지</a>를 추가함</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-08-13</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress">음원 재생 경과 보고하기</a> 절의 설명을 업데이트함</li>
          <li><a href="/Develop/References/MessageInterfaces/Clova.md#RenderSuggestion">RenderSuggestion</a> 지시 메시지추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-07-22</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlaylist">TemplateRuntime.RenderPlaylist</a> 지시 메시지 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/TemplateRuntime.md#SelectCommandIssued">TemplateRuntime.SelectCommandIssued</a> 이벤트 메시지 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-06-24</td>
      <td>
        <ul>
          <li>Clova의 표기를 CLOVA로 수정</li>
          <li>디자인 가이드라인 분류에 <a href="/Design/Brand.md">브랜드</a> 항목을 추가하고 아래와 같은 문서를 추가 및 재구조화함.
            <ul>
              <li><a href="/Design/Brand/Logo.md">로고</a></li>
              <li><a href="/Design/Brand/Symbol.md">심볼</a></li>
              <li><a href="/Design/Brand/Co-branding.md">협력사 브랜드 조합</a></li>
              <li><a href="/Design/Brand/Badge.md">배지</a></li>
            </ul>
          </li>
          <li>디자인 가이드라인 분류에 <a href="/Design/Design_Foundation.md">디자인 기초</a> 항목을 추가하고 아래와 같은 문서를 추가함.
            <ul>
              <li><a href="/Design/DesignFoundation/Color.md">색상</a></li>
              <li><a href="/Design/DesignFoundation/Typography.md">타이포그래피</a></li>
              <li><a href="/Design/DesignFoundation/Icons.md">아이콘</a></li>
            </ul>
          </li>
          <li>디자인 가이드라인 분류에 <a href="/Design/UI.md">UI</a> 항목을 추가하고 기존 문서를 아래와 같이 문서를 재구조화함.
            <ul>
              <li><a href="/Design/UI/Client_State_And_Event.md">클라이언트 상태와 이벤트</a></li>
              <li><a href="/Design/UI/Button.md">버튼</a></li>
              <li><a href="/Design/UI/Light.md">조명</a></li>
              <li><a href="/Design/UI/Audio.md">오디오</a></li>
              <li><a href="/Design/UI/Voice_User_Interface.md">Voice User Interface</a></li>
            </ul>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-06-17</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/Alerts.md">Alerts API</a>에 알람 삭제 요청을 위한 <a href="/Develop/References/MessageInterfaces/Alerts.md#RequestDeleteAlert">Alerts.RequestDeleteAlert</a> 이벤트 메시지를 추가하고 <a href="/Develop/Guides/Handle_Alerts.md#EditAlert">알람 수정 또는 삭제하기</a> 절의 설명을 업데이트함</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-04-06</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/Alerts.md#SetAlert">Alerts.SetAlert</a> 지시 메시지의 asset[] 필드를 사용할 수 있는 타입에 TIMER 타입이 추가됨</li>
          <li>Content template <a href="/Develop/References/ContentTemplates/Common_Fields.md">공통 필드</a>에 이모지를 표시할 수 있도록 emoji 필드를 추가함</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-03-09</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#ReportPlaybackState">AudioPlayer.ReportPlaybackState</a> 이벤트 메시지의 stream 필드와 token 필드 설명에 노트 박스를 추가하여 필드 값이 생략될 수 있는 조건을 명시</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-03-04</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Handle_Audio_Playback.md#ManageStreamInfo">음원 정보 관리하기</a> 절의 설명을 개선</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-02-26</td>
      <td>
        <ul>
          <li><a href="/Design/UI/Client_State_And_Event.md">Client 상태</a> 중 'Mute on' 상태에 대한 표기를 'Mic. off'로 변경하고 그에 따른 버튼, 조명, 효과음에 대한 디자인 가이드라인을 업데이트함</li>
          <li><a href="/Develop/References/Context_Objects.md">맥락 정보</a>의 <a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>에 화면 자동 밝기와 관련된 정보 객체 <a href="/Develop/References/Context_Objects.md#ScreenAutoBrightnessInfoObject">ScreenAutoBrightnessInfoObject</a>를 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-02-17</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Handle_Alerts.md">알람 처리하기</a> 설명을 업데이트하고 클라이언트가 표준 시간 동기화를 수시로 수행하도록 주의 문구 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2020-01-06</td>
      <td>
        <ul>
          <li>더보기 버튼(More)을 표시하기 위해 <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md#MoreCommandIssued">TemplateRuntime.MoreCommandIssued</a> 이벤트 메시지와 <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlayerInfo">TemplateRuntime.RenderPlayerInfo</a> 지시 메시지의 playableItems[].controls[].name 필드에 "MORE" 항목 추가</li>
          <li><a href="/Develop/References/Content_Templates.md">Content template</a>의 예제 화면을 landscape 화면 형태의 UI 예제로 업데이트 (추후 모식도 제공 예정)</li>
          <li>블루투스 서비스 클래스 이름 정보를 공유하기 위해 <a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a> 맥락 정보의 <a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a> 객체에 btlist[].availableServiceClassList 필드를 추가</li>
          <li>클라이언트 지원 오디오 포맷을 보고 하기 위해 <a href="/Develop/References/Context_Objects.md">맥락 정보</a>에 <a href="/Develop/References/Context_Objects.md#Audio">Device.Audio</a> 객체 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-08-26</td>
      <td>
        <ul>
          <li>noticeText 필드의 내용과 더불어 상세 설명 페이지나 출처에 대한 링크를 제공하기 위해 <a href="/Develop/References/ContentTemplates/CardList.md">CardList</a> 템플릿에 noticeLink 필드 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-08-19</td>
      <td>
        <ul>
          <li>화면이 있는 기기에서 콘텐츠를 제공할 때 서비스 제약에 따른 콘텐츠 변형을 알리는 법적 고지를 위해 <a href="/Develop/References/ContentTemplates/CardList.md">CardList</a> 템플릿에 noticeText 필드 추가</li>
          <li>Client 기능 구현 가이드, CIC API의 인터페이스와 맥락 정보 레퍼런스의 목차 수준을 한 단계씩 올림</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-08-05</td>
      <td>
        <ul>
          <li>클라이언트 기기 디자인 가이드라인 문서를 <a href="/Design/UI/Client_State_And_Event.md">클라이언트 상태와 이벤트</a>, <a href="/Design/UI/Button.md">버튼</a>, <a href="/Design/UI/Light.md">조명</a>, <a href="/Design/UI/Audio.md">소리</a>, 화면, CLOVA inside 페이지로 분리함</li>
          <li><a href="/Design/UI/Audio.md#SupportedAudioFormat">플랫폼 지원 오디오 포맷</a> 내용에 CLOVA가 지원하는 컨테이너 포맷 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-07-31</td>
      <td>
        <ul>
          <li>미디어 재생의 제어 대상을 지정하기 위해 <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a> 네임스페이스의 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectNextCommand">ExpectNextCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPauseCommand">ExpectPauseCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPreviousCommand">ExpectPreviousCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectResumeCommand">ExpectResumeCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectStopCommand">ExpectStopCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Pause">Pause</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Resume">Resume</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Stop">Stop</a> 지시 메시지에 target과 target.namespace 필드를 추가함</li>
          <li><a href="/Design/UI/Audio.md#SoundEffect">디자인 가이드라인의 효과음</a> 항목에서 Attending 상태 진입 효과음 출력을 필수가 아닌 선택으로 수정</li>
          <li>디자인 가이드라인에 CLOVA inside 관련 내용 추가</li>
          <li>CLOVA developer guide에서 CLOVA client guide 문서로 분리됨</li>
          <li>일부 문서 오류 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-07-02</td>
      <td>
        <ul>
          <li>디자인 가이드라인의 <a href="/Design/UI/Light.md#LightEffect">조명 효과</a> 설명에서 대기 시간 초과(timeout)와 관련된 조명 효과 구현 여부를 선택으로 변경</li>
          <li><a href="/Develop/References/Client_Auth_API.md">CLOVA 인증 API</a>의 <a href="/Develop/References/Client_Auth_API.md#RequestAuthorizationCode">Authorization code 요청(/authorize)</a>의 redirect_uri 설명 업데이트</li>
          <li>미디어 재생의 제어 대상을 지정하기 위해 <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a> 네임스페이스의 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#NextCommandIssued">NextCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued">PauseCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PreviousCommandIssued">PreviousCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ResumeCommandIssued">ResumeCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#StopCommandIssued">StopCommandIssued</a> 이벤트 메시지에 source와 source.namespace 필드를 추가함</li>
          <li>일부 예제 오탈자 교정 및 노트 상자 수준 조정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-06-10</td>
      <td>
        <ul>
          <li>디자인 가이드라인의 Green Dot 설명에 예제 추가 및 로고 업데이트 반영</li>
          <li>특정 기기의 상태 공유를 받기 위해 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#RequestStateSynchronization">RequestStateSynchronization</a> 이벤트 메시지에 deviceId 필드 추가</li>
          <li>클라이언트 기기가 등록되지 않았거나 오프라인 상태를 표현하기 위해 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#SynchronizeState">SynchronizeState</a> 이벤트 메시지의 deviceState 필드를 조건부로 수정</li>
          <li><a href="/Develop/References/Client_Auth_API.md">CLOVA 인증 API</a>에서 model_id를 전부 항목으로 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-05-27</td>
      <td>
        <ul>
          <li>디자인 가이드라인의 Voice Agent 설명을 제거하고 Green Dot 설명을 추가</li>
          <li>CLOVA 클라이언트가 CIC로 <a href="/Develop/References/Context_Objects.md">맥락 정보(context)</a>를 전송할 때 임의의 정보를 선택적으로 전송하는 것이 아니라 전송 가능한 모든 상태 정보를 올리도록 설명을 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-05-20</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Handle_Audio_Playback.md#ManageStreamInfo">음원 정보 관리하기</a> 절을 <a href="/Develop/Guides/Handle_Audio_Playback.md">음원 재생 처리하기</a> 가이드에 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-05-04</td>
      <td>
        <ul>
          <li>잘못 기술되었던 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PlayCommandIssued">PlaybackController.PlayCommandIssued</a>와 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#SetRepeatModeCommandIssued">PlaybackController.SetRepeatModeCommandIssued</a> 이벤트 메시지에 대한 설명을 변경</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-04-19</td>
      <td>
        <ul>
          <li>디자인 가이드라인의 <a href="/Design/UI/Audio.md#AudioInterruptionRule">기본 오디오 재생 규칙</a>에서 오디오 콘텐츠를 구분할 수 있도록 각 오디오 콘텐츠 타입에 관련된 CIC API 네임스페이스를 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-04-08</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Client_Auth_API.md">CIC 인증 API</a>의 <a href="/Develop/References/Client_Auth_API.md#RequestAuthorizationCode">Authorization code 요청</a> 설명 중 응답의 'redirect_uri' 필드에 'error' 파라미터에 대한 설명 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-03-25</td>
      <td>
        <ul>
          <li>HLS 음원 제공을 위해 contentType 필드를 CIC API의 <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak">SpeechSynthesizer.Speak</a> 지시 메시지에 추가</li>
          <li>일부 링크 오류 및 예제 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-03-13</td>
      <td>
        <ul>
          <li>일부 레퍼런스 문서의 내용 순서 수정</li>
          <li>내외부 피드백을 문서에 반영</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-02-26</td>
      <td>
        <ul>
          <li>클라이언트 기능 구현하기의 내용을 각각의 페이지로 분리</li>
          <li>UI 요소를 제외한 것에 대해 URL이라는 표현 대신 URI로 수정함</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-25</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Interact_with_CIC.md">CIC 연동하기</a> 문서의 일부 설명 개선</li>
          <li>다이어그램 일부 스타일 수정 및 표의 열 간격 조정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-07</td>
      <td>
        <ul>
          <li>문서에 사용되 일부 UML 다이어그램의 스타일 통일</li>
          <li>문서 이력의 일부 표기 오류 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-04</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Handle_Device_Control.md">클라이언트 동작 제어 처리하기</a> 가이드 문서 추가</li>
          <li><a href="/Develop/Guides/Handle_Bluetooth_Control.md">클라이언트 블루투스 제어 처리하기</a> 가이드 문서 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer API</a>에 재생 대기열 초기화 동작을 보고하기 위한 <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#PlaybackQueueCleared">AudioPlayer.PlaybackQueueCleared</a> 이벤트 메시지 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-12-24</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Handle_Device_Control.md">클라이언트 동작 제어 처리하기</a> 가이드 문서 추가</li>
          <li><a href="/Develop/Guides/Handle_Bluetooth_Control.md">클라이언트 블루투스 제어 처리하기</a> 가이드 문서 추가</li>
          <li><a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a> 템플릿에 현재 날씨 정보와 관련된 nowTemperatureImageCode 필드와 nowTemperatureImageUrl 필드를 추가</li>
          <li><a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a> 템플릿에 문서 오류 수</li>
          <li>일부 시퀀스 다이어그램에 잘못 표기된 노드의 유형을 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-11-30</td>
      <td>
        <ul>
          <li>대화 모델의 설명을 <a href="/Develop/CIC_Overview.md#IndirectDialog">간접 대화 구조</a>와 <a href="/Develop/Guides/Manage_Dialog_ID_And_Handle_Tasks.md">대화 ID 관리 및 작업 처리하기</a>로 설명을 나누고 내용을 보완함</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md#Decrease">DeviceControl.Decrease</a>와 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#Increase">DeviceControl.Increase</a> 지시 메시지에 value 필드를 추가하여 특정 크기 만큼 기기의 화면 밝기나 볼륨을 조정할 수 있도록 지원함</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-11-16</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Handle_Settings.md">설정 정보 처리하기</a> 가이드 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-11-09</td>
      <td>
        <ul>
          <li>음원 재생 및 재생 제어와 관련한 <a href="//Develop/Guides/Handle_Audio_Playback.md">음원 재생 처리하기</a> 가이드 추가(다음과 같은 내용 포함)
            <ul>
              <li><a href="/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream">음원 재생하기</a></li>
              <li><a href="/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress">음원 재생 경과 보고하기</a></li>
              <li><a href="/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback">음원 재생 제어하기</a></li>
              <li><a href="/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState">음원 재생 상태 공유하기</a></li>
            </ul>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-10-20</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> 네임스페이스에 블루투스 기기를 재탐지(rescan)하거나 블루투스 기기의 제거하게 하는 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtDelete">DeviceControl.BtDelete</a>와 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtRescan">DeviceControl.BtRescan</a> 지시 메시지 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> 네임스페이스에 블루투스 기기를 통해 음악을 재생하도록 하는 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtPlay">DeviceControl.BtPlay</a> 지시 메시지 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> 네임스페이스의 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtConnect">DeviceControl.BtConnect</a>와 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtDisconnect">DeviceControl.BtDisconnect</a> 지시 메시지에 필드를 추가하여 특정 역할을 가진 기기나 특정 기기를 연결하거나 연결 해지할 수 있게 함</li>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a> 맥락 객체의 <a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a>에 connecting, pairing, playerinfo, scanning 필드를 추가하여 클라이언트의 블루투스 관련 상태 정보를 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-10-05</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CIC_API.md#Error">CIC 오류 메시지</a>의 구조 및 예제를 실제 구현에 맞게 교정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-09-21</td>
      <td>
        <ul>
          <li>콘텐츠의 MIME type을 명시하기 위해 <a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a> <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a>의 payload에 format 필드를 추가</li>
          <li>미디어 콘텐츠 재생 시 좋아요 및 구독 기능을 처리하기 위해 <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md">TemplateRuntime</a> 네임스페이스에 SubscribeCommandIssued, UnsubscribeCommandIssued 이벤트 메시지와 UpdateLike, UpdateSubscribe 지시 메시지에 추가</li>
          <li>미디어 콘텐츠 재생 시 표시해야 하는 버튼이나 제어 UI의 종류를 <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlayerInfo">TemplateRuntime.RenderPlayerInfo</a> 지시 메시지에 추가</li>
          <li>일부 잘못된 코드 예제를 수정</li>
          <li>일부 잘못된 링크를 수정</li>
          <li>일부 사용자 접점에 있는 Extension 표기를 Skill로 변경(UI 캡처 이미지 함께 업데이트)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-09-07</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Handle_Alerts.md">알람 처리하기</a> 절의 링크 오류, 코드 예제 표기 오류 수정</li>
          <li>예제 설명 중 "yourdomain.com"으로 표시된 예제를 문서 작성용 도메인 이름인 "example.com"으로 변경</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-29</td>
      <td>
        <ul>
          <li>디자인 가이드라인에 <a href="/Design/UI/Light.md#LightColor">조명 색상</a>의 RGB 값 변경</li>
          <li>클라이언트 기능 구현하기 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a>, <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md">TemplateRuntime</a> 네임스페이스에 일부 필드 업데이트</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-24</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/Alerts.md#StopAlert">Alerts.StopAlert</a> 지시 메시지의 예제에서 오류 수정</li>
          <li>표기에 따른 혼동을 피하기 위해 <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> 지시 메시지의 initiator.inputSource 필드의 설명을 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-09</td>
      <td>
        <ul>
          <li>대화 모델에 대한 설명을 보완</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-07-23</td>
      <td>
        <ul>
          <li>디자인 가이드라인의 <a href="/Design/UI/Audio.md#SoundEffect">효과음</a> 중 Attending 상태 진입에 대한 효과음 업데이트</li>
          <li><a href="/Develop/References/Client_Auth_API.md">CIC 인증 API</a>의 <a href="/Develop/References/Client_Auth_API.md#RequestAuthorizationCode">Authorization code 요청</a> 설명에 423 Locked 상태 코드 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-07-09</td>
      <td>
        <ul>
          <li>클라이언트 기기 설정 정보를 업데이트 및 동기화하기 위해 <a href="/Develop/References/MessageInterfaces/Settings.md">Settings</a> 네임스페이스 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-06-17</td>
      <td>
        <ul>
          <li>실시간 방송 콘텐츠를 구분하기 위해 <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlayerInfo">TemplateRuntime.RenderPlayerInfo</a>에 isLive 필드 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-21</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a> 맥락 객체의 <a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a>에 빠진 필드(btlist[].role) 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-14</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/Clova.md#LaunchURI">LaunchURI</a> 지시 메시지를 DeviceControl 네임스페이스에서 <a href="/Develop/References/MessageInterfaces/Clova.md">CLOVA</a> 네임스페이스로 이전</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-07</td>
      <td>
        <ul>
          <li>DeviceControl 네임스페이스에 LaunchURI 지시 메시지 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> 네임스페이스의 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#LaunchApp">LaunchApp</a> 지시 메시지와 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#OpenScreen">OpenScreen</a> 지시 메시지의 지원을 중지(Deprecated)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-16</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> 이벤트 메시지의 wakeWord 필드 설명 및 Audio data 설명 업데이트</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> 네임스페이스에 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#Open">Open</a> 지시 메시지 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-09</td>
      <td>
        <ul>
          <li>클라이언트 기기 디자인 가이드라인에서 부팅 화면 관련 설명 및 예제 이미지를 업데이트</li>
                  </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-02</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a> 네임스페이스에 메시지 스펙 추가 및 일부 필드 업데이트
            <ul>
              <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#ExpectReportPlaybackState">AudioPlayer.ExpectReportPlaybackState</a> 지시 메시지, <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#ReportPlaybackState">AudioPlayer.ReportPlaybackState 이벤트 메시지</a> 추가</li>
              <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play">AudioPlayer.Play</a> 지시 메시지의 payload 필드 내용 업데이트</li>
              <li>ProgressReportXXX, PlayXXX 형식의 이름을 가진 이벤트 필드에 token 필드 값 필수로 추가</li>
            </ul>
          </li>
          <li><a href="/Develop/References/Context_Objects.md#PlaybackState">AudioPlayer.PlaybackState</a> 맥락 객체에 repeatMode 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a> 네임스페이스에 총 12 건의 메시지 스펙 추가
            <ul>
              <li><a href="/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued">PlaybackController.PauseCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PlayCommandIssued">PlaybackController.PlayCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ResumeCommandIssued">PlaybackController.ResumeCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#SetRepeatModeCommandIssued">PlaybackController.SetRepeatModeCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#StopCommandIssued">PlaybackController.StopCommandIssued</a> 이벤트 메시지 추가</li>
              <li><a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectNextCommand">PlaybackController.ExpectNextCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPauseCommand">PlaybackController.ExpectPauseCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPlayCommand">PlaybackController.ExpectPlayCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPreviousCommand">PlaybackController.ExpectPreviousCommand</a>,
<a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectResumeCommand">PlaybackController.ExpectResumeCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectStopCommand">PlaybackController.ExpectStopCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#SetRepeatMode">PlaybackController.SetRepeatMode</a> 지시 메시지 추가</li>
              <li><a href="/Develop/References/MessageInterfaces/PlaybackController.md#TurnOnRepeatMode">PlaybackController.TurnOnRepeatMode</a> 지시 메시지와 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#TurnOffRepeatMode">PlaybackController.TurnOffRepeatMode</a>는 사라질 예정</li>
            </ul>
          </li>
          <li>미디어 스트림을 위한 정보와 재생 목록을 표시하기 위한 재생 메타 정보를 분리하기 위해 <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md">TemplateRuntime</a> 네임스페이스 추가</li>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>의 <a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a>에 scanlist 필드 추가</li>
          <li>PIN 코드를 사용하는 외부 블루투스 기기와 연결할 수 있도록 <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> 네임 스페이스에 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtConnectByPINCode">BtConnectByPINCode</a> 지시 메시지와 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtRequestForPINCode">BtRequestForPINCode</a>, <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtRequestToCancelPINCodeInput">BtRequestToCancelPINCodeInput</a> 이벤트 메시지 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> 네임 스페이스의 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtConnect">BtConnect</a> 지시 메시지에 payload 추가</li>
          <li>CLOVA developer console의 일부 UI 업데이트 적용</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-03-19</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>에 <a href="/Develop/References/Context_Objects.md#SoundOutputInfoObject">SoundOutputInfoObject</a> 추가</li>
          <li>사용자가 설정한 임의의 명령을 실행할 수 있는 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#CustomCommandIssued">CustomCommandIssued</a> 이벤트 메시지를 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#CustomCommandIssued">PlaybackController</a> 네임스페이스에 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-03-05</td>
      <td>
        <ul>
          <li><a  href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> 이벤트 메시지 initiator 필드의 설명을 수정</li>
          <li>디자인 가이드라인에서 클라이언트 상태 중 Hearing 상태의 이름을 Listening으로 수정</li>
          <li>클라이언트 기기 디자인 가이드라인의 <a href="/Design/UI/Audio.md">소리</a>에서 오디오 콘텐츠 타입으로 Feedback 타입을 추가하고 설명에 관련 규칙을 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-26</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> 이벤트 메시지 initiator 필드에 deviceUUID 필드를 추가</li>
          <li>알람 동기화와 관련된 <a href="/Develop/References/MessageInterfaces/Alerts.md#RequestSynchronizeAlert">RequestSynchronizeAlert</a> 이벤트 메시지와 <a href="/Develop/References/MessageInterfaces/Alerts.md#SynchronizeAlert">SynchronizeAlert</a> 지시 메시지를 <a href="/Develop/References/MessageInterfaces/Alerts.md">Alerts</a> 네임스페이스에 추가</li>
          <li>System 네임스페이스에서 알람 동기화와 관련된 일부 필드를 제거할 예정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-19</td>
      <td>
        <ul>
          <li>사용자의 호출을 정확히 판단하기 위해 <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> 이벤트 메시지에 initiator 필드를 추가</li>
          <li>리마인더 및 동작 예약의 내용을 확인하기 위해 <a href="/Develop/References/MessageInterfaces/Alerts.md#SetAlert">Alerts.SetAlert</a> 지시 메시지에 label 필드를 추가</li>
          <li>리마인더 및 동작 예약의 내용을 표시하기 위해 <a href="/Develop/References/ContentTemplates/ActionTimer.md">ActionTimer</a>, <a href="/Develop/References/ContentTemplates/ActionTimerList.md">ActionTimerList</a>, <a href="/Develop/References/ContentTemplates/Reminder.md">Reminder</a>, <a href="/Develop/References/ContentTemplates/ReminderList.md">ReminderList</a> 템플릿에 label 필드를 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-05</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a>의 durationInMilliseconds 필드에 대한 설명 수정</li>
          <li><a href="/Develop/References/ContentTemplates/Atmosphere.md">Atmosphere</a>, <a href="/Develop/References/ContentTemplates/CardList.md">CardList</a>, <a href="/Develop/References/ContentTemplates/Humidity.md">Humidity</a>, <a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a>, <a href="/Develop/References/ContentTemplates/TomorrowWeather.md">TomorrowWeather</a>, <a href="/Develop/References/ContentTemplates/WeeklyWeather.md">WeeklyWeather</a>, <a href="/Develop/References/ContentTemplates/WindSpeed.md">WindSpeed</a> 템플릿에 출처 관련 필드 등 내용 추가</li>
          <li>일부 문서 오류 교정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-29</td>
      <td>
        <ul>
          <li>디자인 가이드라인에 <a href="/Design/UI/Audio.md#SoundEffect">Reminder용 효과음</a> 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/Notifier.md">Notifier</a> 네임스페이스에 <a href="/Develop/References/MessageInterfaces/Notifier.md#Notify">Notifier.Notify</a> 이벤트 메시지 추가 및 해당 네임스페이스 메시지의 payload 필드 업데이트</li>
          <li><a href="/Develop/References/Context_Objects.md#SpeechState">SpeechSynthesizer.SpeechState</a> 및 <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md">SpeechSynthesizer</a> 네임스페이스에 <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md#SpeechFinished">SpeechFinished</a>, <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md#SpeechStarted">SpeechStarted</a>, <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md#SpeechStopped">SpeechStopped</a> 이벤트 메시지 추가</li>
          <li>Multi-turn 대화를 위해 <a href="/Develop/References/MessageInterfaces/TextRecognizer.md">TextRecognizer.Recognize</a> 이벤트 메시지에 speechId, explicit 필드 추가</li>
          <li>CLOVA developer console의 일부 UI 업데이트 적용</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-22</td>
      <td>
        <ul>
          <li><a href="/Design/UI/Audio.md#SupportedAudioFormat">플랫폼 지원 오디오 포맷</a>을 디자인 가이드라인에 추가</li>
          <li>UML 다이어그램의 이미지 포맷 변경</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-15</td>
      <td>
        <ul>
          <li>디자인 가이드라인에 알람, 리마인더, 타이머에 대한 조명 효과 및 효과음 가이드라인 설명 추가 </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-08</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Handle_Delegation.md">위임된 사용자 요청 처리하기</a> 절 추가 및 <a href="/Develop/References/MessageInterfaces/Clova.md#HandleDelegatedEvent">Clova.HandleDelegatedEvent</a> 지시 메시지와 <a href="/Develop/References/MessageInterfaces/Clova.md#ProcessDelegatedEvent">Clova.ProcessDelegatedEvent</a> 이벤트 메시지 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/PlaybackController.md#NextCommandIssued">PlaybackController.NextCommandIssued</a>와 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PreviousCommandIssued">PlaybackController.PreviousCommandIssued</a> 이벤트 메시지에 <a href="/Develop/References/Context_Objects.md#PlaybackState">AudioPlayer.PlaybackState</a> 맥락 정보를 포함하도록 설명 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/Alerts.md">Alerts</a> API의 동작 구조에 대한 설명 개선</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> API의 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#DeviceControlWorkFlow">동작 구조</a>에 대한 설명 추가</li>
          <li>일부 content template 및 공유 객체에 대한 오류 교정 내용 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-02</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CIC_API.md#EstablishDownchannel">Downchannel 구성</a>절 내용에 429 오류 코드 및 관련 설명 Remarks 항목에 추가</li>
          <li>길찾기 템플릿(CarRoute, TransportationRoute) 제거, 길찾기에 대한 UI 표현이 ImageText 템플릿으로 대체됨.</li>
          <li>일부 문서 오류, 오타 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-18</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md">SpeechRecognizer</a> 인터페이스에서 ExpectSpeechTimedOut 이벤트 메시지 제거</li>
          <li><a href="/Develop/References/Context_Objects.md">맥락 정보(context)</a>에서 Clova.FreetalkState 개체 제거</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-11</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a> 인터페이스에 <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#ClearQueue">ClearQueue</a> 지시 메시지 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-04</td>
      <td>
        <ul>
          <li><a href="/Design/UI/Audio.md#AudioInterruptionRule">오디오 재생 규칙(audio interruption rule)</a>을 디자인 가이드라인에 추가</li>
          <li>디자인 가이드라인의 이미지 개선</li>
          <li>CIC 연동하기의 사전 준비사항에 <a href="/Develop/Guides/Interact_with_CIC.md#UserAgentString">User-Agent string</a>을 추가</li>
          <li><a href="/Develop/References/CIC_API.md">CIC API</a>의 <a href="/Develop/References/CIC_API.md#SendEvent">이벤트 메시지 전송</a> 절에 412 Precondition Failed 상태 코드 설명 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-20</td>
      <td>
        <ul>
          <li>디자인 가이드라인 추가</li>
          <li>오디오 콘텐츠 및 이미지 썸네일 표시를 위해 <a href="/Develop/References/ContentTemplates/CardList.md">CardList 템플릿</a>의 subType 값에 Type5, Type6를 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-13</td>
      <td>
        <ul>
          <li>볼륨 제어 관련 지시 메시지(<a href="/Develop/References/MessageInterfaces/DeviceControl.md#Decrease">DeviceControl.Decrease</a>, <a href="/Develop/References/MessageInterfaces/DeviceControl.md#Increase">DeviceControl.Increase</a>, <a href="/Develop/References/MessageInterfaces/DeviceControl.md#SetValue">DeviceControl.SetValue</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Mute">PlaybackController.Mute</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Unmute">PlaybackController.Unmute</a>)의 Remarks 항목에 UX 관련 내용 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-06</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#KeepRecording">SpeechRecognizer.KeepRecording</a> 지시 메시지 추가</li>
          <li><a href="/Develop/References/Context_Objects.md#Display">Device.Display</a> 맥락 정보 추가</li>
          <li><a href="/Develop/References/ContentTemplates/ActionTimer.md">ActionTimer</a>, <a href="/Develop/References/ContentTemplates/ActionTimerList.md">ActionTimerList</a>, <a href="/Develop/References/ContentTemplates/Alarm.md">Alarm</a>, <a href="/Develop/References/ContentTemplates/AlarmList.md">AlarmList</a>, <a href="/Develop/References/ContentTemplates/Memo.md">Memo</a>, <a href="/Develop/References/ContentTemplates/MemoList.md">MemoList</a>, <a href="/Develop/References/ContentTemplates/Reminder.md">Reminder</a>, <a href="/Develop/References/ContentTemplates/ReminderList.md">ReminderList</a>, <a href="/Develop/References/ContentTemplates/Schedule.md">Schedule</a>, <a href="/Develop/References/ContentTemplates/ScheduleList.md">ScheduleList</a>, <a href="/Develop/References/ContentTemplates/Timer.md">Timer</a>, <a href="/Develop/References/ContentTemplates/TimerList.md">TimerList</a> 템플릿에 token 필드 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-23</td>
      <td>
        <ul>
          <li><a href="/Develop/References/ContentTemplates/Text.md">Text</a> 템플릿에 emotionCode 필드와 motionCode 필드를 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/Alerts.md#SetAlert">Alerts.SetAlert</a> 지시 메시지의 assets[].url 필드 내용 변경</li>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#StreamRequested">AudioPlayer.StreamRequested</a> 이벤트 메시지의 예제 오류 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-16</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a> 네임스페이스에 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Replay">Replay</a> 지시 메시지 추가</li>
          <li>알람 동기화에 대한 보충 설명을 알람 동작 구조 절에 추가</li>
          <li>content 필드를 <a href="/Develop/References/Context_Objects.md#AlertsState">Alert.AlertsState</a> 문맥 정보의 <a href="/Develop/References/Context_Objects.md#AlertInfoObject">AlertInfoObject</a>에서 제거</li>
          <li>일부 문서 이미지 수정 및 문서 오류 교정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-02</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/Alerts.md">Alerts</a> 네임스페이스 및 알람 관련 인터페이스 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/System.md">System</a> 네임스페이스 및 알람 관련 인터페이스 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a> 지시 메시지에 expectContentType 필드 추가</li>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>의 <a href="/Develop/References/Context_Objects.md#VolumeInfoObject">VolumeInfoObject</a>에 warning 필드 추가</li>
          <li><a href="/Develop/References/ContentTemplates/ActionTimer.md">ActionTimer</a>, <a href="/Develop/References/ContentTemplates/ActionTimerList.md">ActionTimerList</a>, <a href="/Develop/References/ContentTemplates/Alarm.md">Alarm</a>, <a href="/Develop/References/ContentTemplates/AlarmList.md">AlarmList</a>, <a href="/Develop/References/ContentTemplates/Memo.md">Memo</a>, <a href="/Develop/References/ContentTemplates/MemoList.md">MemoList</a>, <a href="/Develop/References/ContentTemplates/Reminder.md">Reminder</a>, <a href="/Develop/References/ContentTemplates/ReminderList.md">ReminderList</a>, <a href="/Develop/References/ContentTemplates/Schedule.md">Schedule</a>, <a href="/Develop/References/ContentTemplates/ScheduleList.md">ScheduleList</a>, <a href="/Develop/References/ContentTemplates/Timer.md">Timer</a>, <a href="/Develop/References/ContentTemplates/TimerList.md">TimerList</a> 템플릿 추가</li>
          <li><a href="/Develop/References/ContentTemplates/ImageText.md">ImageText</a> 템플릿의 일부 코드 예제 수정</li>
          <li><a href="/Develop/References/ContentTemplates/Popup.md">Popup 템플릿</a> 일부 필드 수정</li>
          <li><a href="/Develop/Guides/Interact_with_CIC.md#CreateCLOVAAccessToken">CLOVA access token 생성하기</a>와 <a href="/Develop/References/Client_Auth_API.md#RequestAuthorizationCode">Authorization code 요청</a>에 서비스 이용 약관에 대한 내용 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-25</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController API</a>에 음악 재생 제어용 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#NextCommandIssued">PlaybackController.NextCommandIssued</a> 이벤트 메시지와 <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PreviousCommandIssued">PlaybackController.PreviousCommandIssued</a> 이벤트 메시지 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a> 지시 메시지에 expectSpeechId 필드를 <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> 이벤트 메시지에 speechId와 explicit 필드를 각각 추가</li>
          <li><a href="/Develop/References/ContentTemplates/Popup.md">Popup 템플릿</a> 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-18</td>
      <td>
        <ul>
          <li>DeviceControl API에 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState">DeviceControl.ExpectReportState</a> 지시 메시지, <a href="/Develop/References/MessageInterfaces/DeviceControl.md#ReportState">DeviceControl.ReportState</a> 이벤트 메시지, <a href="/Develop/References/MessageInterfaces/DeviceControl.md#RequestStateSynchronization">DeviceControl.RequestStateSynchronization</a> 이벤트 메시지 추가 및 DeviceControl.UpdateDeviceState 지시 메시지를 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#SynchronizeState">DeviceControl.SynchronizeState</a>로 이름 변경</li>
          <li><a href="/Develop/References/ContentTemplates/Text.md">Text</a> 템플릿에 item3 필드 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play">AudioPlayer.Play</a> 지시 메시지에 출처 정보 관련 source 필드 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a>에 durationInMilliseconds 필드 추가 </li>
          <li>Notifier 네임스페이스 및 <a href="/Develop/References/MessageInterfaces/Notifier.md#ClearIndicator">ClearIndicator</a>, <a href="/Develop/References/MessageInterfaces/Notifier.md#SetIndicator">SetIndicator</a> 지시 메시지 추가</li>
          <li><a href="/Develop/References/ContentTemplates/Atmosphere.md">대기 정보(Atmosphere) 템플릿</a> 추가</li>
          <li>라이선스 이슈에 따른 날씨 템플릿의 bgClipURL 필드 사용 불가 문구 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-11</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a> 지시 메시지에 explicit 필드 추가</li>
          <li><a href="/Develop/References/Content_Templates.md">Content template</a>에 <a href="/Develop/References/ContentTemplates/Common_Fields.md">공통 필드</a> 스펙 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-04</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/Clova.md#Help">Clova.Help</a> 지시 메시지 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md#LaunchApp">DeviceControl.LaunchApp</a> 지시 메시지 추가</li>
          <li>TextRecognizer 네임스페이스 및 <a href="/Develop/References/MessageInterfaces/TextRecognizer.md">TextRecognizer.Recognize</a> 이벤트 메시지 추가</li>
          <li><a href="/Develop/References/CIC_API.md">CIC API</a>의 목차, 설명 재작성</li>
          <li>CIC API 내용 업데이트: 요청/응답 헤더의 Status code 추가, REST API reference 문서 포맷 적용</li>
          <li>일부 문서 오류 수정</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-28</td>
      <td>
        <ul>
          <li>셋톱박스용 TV 채널 정보 스펙과 전원 상태 정보 스펙을 <a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>와 <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl API</a>에 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl API</a>에서 target으로 사용되는 값 일부 추가 및 변경: power, energysave, screenbrightness</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl API</a>의 SetPoint를 <a href="/Develop/References/MessageInterfaces/DeviceControl.md#SetValue">SetValue</a>로 이름 변경</li>
          <li><a href="/Develop/References/Client_Auth_API.md">CLOVA 인증 API</a> 내용 업데이트 - 요청/응답 헤더와 Status code 추가, REST API reference 문서 포맷 적용</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-21</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Interact_with_CIC.md#ManageConnection">Access token 갱신</a>절 추가 및 /token API 내용 업데이트</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-14</td>
      <td>
        <ul>
          <li>대화 모델 설명 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> API 추가</li>
          <li><a href="/Develop/References/Context_Objects.md">Device.DeviceState</a> payload 필드 추가: airplane, battery, bluetooth, brightness, flashLight, gps, powerSavingMode, soundMode, volume, wifi</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-04</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/Clova.md#Hello">Clova.Hello</a> 지시 메시지 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play">AudioPlayer.Play</a> 지시 메시지의 AudioItem 객체에 type 필드 추가</li>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a> 객체에 urlPlayable 필드 추가</li>
          <li><a href="/Develop/References/CIC_API.md#Error">CIC 오류 메시지</a> 스펙 추가</li>
          <li><a href="/Develop/References/CIC_API.md#MultipartMessage">Multipart 메시지</a> 내용 재작성</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-28</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a>의 PlayNext, Stop 제거 (<a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a>에 병합)</li>
          <li> <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a>의 메시지 이름 변경(Mute, Next, Pause, Previous, Resume, Stop, Unmute, VolumeDown, VolumeUp </li>
          <li>길찾기 템플릿 추가: CarRoute, TransportationRoute</li>
          <li>날씨 템플릿 추가: <a href="/Develop/References/ContentTemplates/Humidity.md">Humidity</a>, <a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a>, <a href="/Develop/References/ContentTemplates/TomorrowWeather.md">TomorrowWeather</a>, <a href="/Develop/References/ContentTemplates/WeeklyWeather.md">WeeklyWeather</a>, <a href="/Develop/References/ContentTemplates/WindSpeed.md">WindSpeed</a></li>
          </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-14</td>
      <td>
        <ul>
          <li><a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a> 객체 beginAtInMilliseconds 필드 내용 추가</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-06-19</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Interact_with_CIC.md#ManageConnection">연결 관리하기</a> 업데이트 (HTTP Ping 프레임을 사용할 수 없을 때)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-06-08</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Interact_with_CIC.md">CIC 연동하기</a>에 <a href="/Develop/Guides/Interact_with_CIC.md#ManageConnection">연결 관리하기</a> 추가 (HTTP Ping)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-05-29</td>
      <td>
        <ul>
          <li>CIC 문서 파트 작성</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>
