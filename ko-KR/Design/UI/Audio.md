<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# 오디오

클라이언트 기기에서 오디오 콘텐츠, 효과음과 같은 소리를 출력할 때 어떤 사항을 지켜야 하는지 설명합니다.

* [기본 오디오 재생 규칙](#AudioInterruptionRule)
* [사용자 발화 시 오디오 재생 규칙](#AudioInterruptionRuleForUserUtterance)
* [효과음](#SoundEffect)
* [플랫폼 지원 오디오 포맷](#SupportedAudioFormat)

## 기본 오디오 재생 규칙 {#AudioInterruptionRule}

클라이언트는 오디오 콘텐츠를 재생하는 중에 다른 오디오 콘텐츠를 재생해야 할 수 있습니다. 이때 클라이언트는 오디오 재생 규칙에 따라 오디오 콘텐츠를 재생해야 합니다. 오디오 재생 규칙은 오디오 콘텐츠 타입을 기준으로 작성되었습니다. 따라서 재생 규칙에 대해 알기 전에 우선 오디오 콘텐츠 타입에 대해 알아야 합니다. 오디오 콘텐츠 타입은 다음과 같이 구분되며, 클라이언트는 각 오디오 콘텐츠 타입과 관련된 CIC API 네임스페이스에따라 CLOVA로부터 전달되는 오디오 콘텐츠를 구분해야 합니다.

| 오디오 콘텐츠 타입 | 설명                                   | CIC API 네임스페이스             |
|---------------|---------------------------------------------|----------------------------------|
| Alert         | 알람 소리, 타이머 소리, 리마인더 소리, 리마인더 발화, 긴급 경보음과 같은 오디오 콘텐츠             | [`Alerts`](/Develop/References/MessageInterfaces/Alerts.md) |
| Content       | 사용자 요청에 대한 음악, 동화, 뉴스, Podcast과 같은 오디오 콘텐츠                           | [`AudioPlayer`](/Develop/References/MessageInterfaces/AudioPlayer.md) |
| Dialog        | 사용자 요청에 대한 TTS 오디오 콘텐츠                                                  | [`SpeechRecognizer`](/Develop/References/MessageInterfaces/SpeechRecognizer.md), [`SpeechSynthesizer`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md) |
| Feedback      | 초기화음, 벨소리(ring tone), 통화 연결음(ringback tone)                              | 없음 (클라이언트 자체 판단) |
| Notification  | 비프음, 시스템 상태 발화(배터리 부족 알림, 블루투스 연결 해제 알림과 같은 소리), 알림음, 알림 발화         | [`Notifier`](/Develop/References/MessageInterfaces/Notifier.md) |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Alert와 notification 타입은 효과음과 발화를 묶어 하나의 오디오 콘텐츠로 취급해야 합니다. 예를 들면 리마인더이면 리마인더 소리와 리마인더 발화를 하나의 alert 오디오 콘텐츠로 봐야하며, 배터리 부족 알림이면 비프음과 "배터리가 부족합니다."와 같은 시스템 상태 발화를 하나의 notification 오디오 콘텐츠로 취급해야 됩니다.</p>
</div>

다음과 같은 오디오 재생 규칙이 있습니다.

* 물리 버튼의 효과음은 즉시 재생되어야 하며 이를 위해 mixing 방식으로 효과음을 출력해야 합니다.
* 오디오 콘텐츠는 즉시 재생되어야 합니다. 만약, 이미 재생 중인 오디오 콘텐츠가 있다면 이를 배경음(background)으로 처리하고 새로운 오디오 콘텐츠를 재생해야 합니다.
* 다만, 이미 재생 중인 오디오 콘텐츠와 새로 재생해야 할 오디오 [콘텐츠의 타입](#AudioInterruptionRule)이 서로 같다면 다음과 같이 처리합니다.
  - **Alert, Content, Dialog, Feedback 타입**: 재생 중인 오디오 콘텐츠의 재생을 중지(cancel)하고 새로운 오디오 콘텐츠를 재생합니다.
  - **Notification 타입**: 현재 재생 중인 오디오 콘텐츠를 계속 재생하고 새로운 오디오 콘텐츠를 재생 대기열(queue)에 보관합니다. 이미 재생 중인 오디오 콘텐츠를 재생한 후 재생 대기열에 있는 순서대로 오디오 콘텐츠를 재생합니다.
* 오디오 재생을 중지하려면 현재 재생 중인 오디오 콘텐츠부터 재생을 중지해야 합니다.

다음은 위 규칙을 토대로 오디오 콘텐츠 타입에 따라 이미 재생 중인 오디오 콘텐츠를 어떻게 처리해야 하는지 나타냅니다.

<style>
.audio-rule th {
  text-align: center !important;
}
.notion-content .column-list {
  display: flex;
  justify-content: space-between;
}
.notion-content .column {
  padding: 0.5em !important;
}
.notion-content .column:first-child {
  padding-left: 0;
  align-self:flex-start;
}
.notion-content .column:last-child {
  padding-right: 0;
  /* align-self:center; */
  align-self: flex-start;
}
</style>
<table class="audio-rule" style="text-align:center !important">
  <thead>
    <tr>
      <th rowspan="2">재생 중인 타입</th><th colspan="5">재생해야 할 타입</th><th rowspan="2">물리 버튼 효과음</th>
    </tr>
    <tr>
      <th>Alert</th><th>Content</th><th>Dialog</th><th>Feedback</th><th>Notification</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alert</th>
      <td><span class="audioInterruptionRule cancelPlayback">재생 중지</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td rowspan="5"><span class="audioInterruptionRule">Mixing 처리</span></td>
    </tr>
    <tr>
      <th>Content</th>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule cancelPlayback">재생 중지</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
    </tr>
    <tr>
      <th>Dialog</th>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule cancelPlayback">재생 중지</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
    </tr>
    <tr>
      <th>Feedback</th>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule cancelPlayback">재생 중지</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
    </tr>
    <tr>
      <th>Notification</th>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">배경음 처리</span></td>
      <td><span class="audioInterruptionRule continuePlayback">계속 재생</span></td>
    </tr>
  </tbody>
</table>

## 사용자 발화 시 오디오 재생 규칙 {#AudioInterruptionRuleForUserUtterance}

클라이언트가 오디오 콘텐츠를 재생하는 중에 사용자가 음성 입력을 시도하면 다음과 같은 규칙을 따릅니다.

* 이미 재생 중인 오디오 콘텐츠가 있다면 attending 상태부터 processing & reporting 상태까지 이를 배경음(background)으로 처리해야 합니다.
* 음성 입력 대기 시간을 초과하거나 사용자 요청을 처리하는데 실패했다면 배경음으로 처리했던 오디오 콘텐츠를 원래대로 재생해야 합니다.
* 요청 처리 결과에 따라 다른 오디오 콘텐츠를 재생해야 하면 [기본 오디오 재생 규칙](#AudioInterruptionRule)에 따라 오디오 콘텐츠를 재생해야 합니다.
* Multi-turn 대화를 시도할 때 추가로 갖게되는 listening, processing & reporting 상태에도 위와 같은 규칙을 따릅니다.

사용자 음성 입력을 수신하는 attending, listening 상태에서 새로운 오디오 콘텐츠 재생 요청이 들어오면 다음과 같이 처리해야 합니다.

* **Alert/dialog/content** 타입의 오디오 콘텐츠를 재생해야 하면 사용자 음성 입력 수신을 취소하고 해당 오디오 콘텐츠를 재생해야 합니다.
* **Notification** 타입이나 **Feedback** 타입의 오디오 콘텐츠를 재생해야 하면 해당 오디오 콘텐츠를 배경음으로 재생해야 합니다.

## 효과음 {#SoundEffect}

클라이언트는 기기의 상태나 사용자 요청의 피드백과 같은 것을 표현하기 위해 [조명](/Design/UI/Light.md)뿐만 아니라 효과음을 함께 제공해야 합니다. 클라이언트가 사용자에게 어떤 상황에 어떤 효과음을 제공해야 하는지 설명합니다.

* [효과음 종류](#SoundEffects)
* [효과음 가이드라인](#SoundEffectGuideline)

### 효과음 종류 {#SoundEffects}

클라이언트의 [상태 및 이벤트](/Design/UI/Client_State_And_Event.md) 표현을 위해 다음과 같은 효과음을 제공해야 합니다.

<div class="notion-content">
  <div class="column-list">
    <div style="width:50%" class="column">
      <ul class="bulleted-list">
        <li><strong>Attending 상태 진입</strong><br /><audio title="Attending" controls><source src="./Audio/Clova-Client-Soundeffect-Attending.wav" type="audio/wav" /></audio></li>
      </ul>
      <p class="">
      </p>
      <ul class="bulleted-list">
        <li><strong>Error 상태 진입</strong><br /><audio title="Error" controls><source src="./Audio/Clova-Client-SoundEffect-Error.wav" type="audio/wav" /></audio></li>
      </ul>
      <p class="">
      </p>
      <ul class="bulleted-list">
        <li><strong>마이크 꺼짐(Mic. off) 상태 진입</strong><br /><audio title="Turn microphone off" controls><source src="./Audio/Clova-Client-SoundEffect-Turn_Mic_Off.wav" type="audio/wav" /></audio></li>
      </ul>
      <p class="">
      </p>
      <ul class="bulleted-list">
        <li><strong>마이크 꺼짐(Mic. off) 상태 해제</strong><br /><audio title="Turn microphone on" controls><source src="./Audio/Clova-Client-SoundEffect-Turn_Mic_On.wav" type="audio/wav" /></audio></li>
      </ul>
    </div>
    <div style="width:50%" class="column">
      <ul class="bulleted-list">
        <li><strong>알람(이벤트 발생 시, 효과음 반복 재생)</strong><br /><audio title="Alarm" controls><source src="./Audio/Clova-Client-SoundEffect-Alarm.wav" type="audio/wav" /></audio></li>
      </ul>
      <p class="">
      </p>
      <ul class="bulleted-list">
        <li><strong>리마인더(이벤트 발생 시, 효과음, TTS 순서로 반복 재생)</strong><br /><audio title="Reminder" controls><source src="./Audio/Clova-Client-SoundEffect-Reminder.wav" type="audio/wav" /></audio></li>
      </ul>
      <p class="">
      </p>
      <ul class="bulleted-list">
        <li><strong>타이머(이벤트 발생 시, 효과음 반복 재생)</strong><br /><audio title="Timer" controls><source src="./Audio/Clova-Client-SoundEffect-Timer.wav" type="audio/wav" /></audio></li>
      </ul>
      <p class="">
      </p>
    </div>
  </div>
</div>

### 효과음 가이드라인 {#SoundEffectGuideline}

효과음을 제공할 때 다음과 같은 사항을 따라야 합니다.

* 제공하는 효과음 음원을 그대로 사용할 것을 권고합니다.
* 제공하는 효과음 외에도 상황에 적절하거나 제조사의 UX 정책에 따라 효과음을 추가할 수 있습니다.
* 사용자는 각 상황을 소리로 인지할 수 있어야 합니다.
* 조명 효과나 화면의 상황에 맞게 일관성있는 효과음을 제공해야 합니다.
* 버튼 피드백에 대한 효과음을 제공하는 상황이면 버튼의 물성과 촉감에 어울리는 효과음을 제공해야 합니다.

## 플랫폼 지원 오디오 포맷 {#SupportedAudioFormat}

클라이언트는 CLOVA가 전달하는 음원을 재생해야 하므로 반드시 CLOVA가 지원하는 오디오 압축 포맷을 재생할 수 있어야 합니다.

<!-- Start of the shared content: SupportedAudioFormat -->

CLOVA가 지원하는 오디오 압축 포맷(codec)은 다음과 같습니다.

| 오디오 압축 포맷                 | 라이선스 비용 |
|----------------------------------|---------------|
| MPEG-1 or MPEG-2 Audio Layer III | 무료          |

CLOVA가 지원하는 파일 확장자와 MIME 타입은 다음과 같습니다.

| 파일 확장자     | MIME 타입                      | 비고                            |
|-------------|-------------------------------|--------------------------------|
| AAC         | audio/aac                     | <!-- -->                       |
| M3U8        | application/vnd.apple.mpegurl | HTTP Live Streaming 사용        |
| MP3         | audio/mpeg                    | <!-- -->                       |

<!-- End of the shared content -->

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p><a href="/Develop/References/Context_Objects.md">맥락 정보</a> 중 <a href="/Develop/References/Context_Objects.md#Audio">Device.Audio</a> 객체를 이용하면 클라이언트 기기가 지원하는 미디어 포맷과 함께 선호하는 포맷의 순서를 전달할 수 있습니다.</p>
</div>
