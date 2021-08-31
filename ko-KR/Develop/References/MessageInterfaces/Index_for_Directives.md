# 지시 메시지 색인

| 네임스페이스          | 메시지 이름       | 설명                                             |
|--------------------|----------------|-------------------------------------------------|
| Alerts             | [`DeleteAlert`](/Develop/References/MessageInterfaces/Alerts.md#DeleteAlert)             | CLOVA가 특정 알람을 삭제하도록 클라이언트에 지시합니다. |
| Alerts             | [`SetAlert`](/Develop/References/MessageInterfaces/Alerts.md#SetAlert)                   | CLOVA가 알람을 새로 추가하거나 특정 알람을 수정하도록 클라이언트에 지시합니다. |
| Alerts             | [`StopAlert`](/Develop/References/MessageInterfaces/Alerts.md#StopAlert)                 | CLOVA가 특정 알람을 중지하도록 클라이언트에 지시합니다.  |
| Alerts             | [`SynchronizeAlert`](/Develop/References/MessageInterfaces/Alerts.md#SynchronizeAlert)   | CLOVA가 `payload`에 있는 사용자의 알람 데이터를 동기화하도록 클라이언트에 지시합니다.  |
| AudioPlayer        | [`ClearQueue`](/Develop/References/MessageInterfaces/AudioPlayer.md#ClearQueue)          | CLOVA가 오디오 스트림 재생 대기열(queue)을 초기화하도록 클라이언트에 지시합니다.                              |
| AudioPlayer        | [`ExpectReportPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#ExpectReportPlaybackState) | CLOVA가 현재 재생 상태를 보고하도록 클라이언트에 지시합니다. |
| AudioPlayer        | [`Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play)                      | CLOVA가 특정 오디오 스트림을 재생하거나 재생 대기열에 추가하도록 클라이언트에 지시합니다.                          |
| AudioPlayer        | [`StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver)    | CLOVA가 오디오 스트림 정보를 교체하여 재생하도록 클라이언트에 지시합니다. |
| Clova              | [`ExpectLogin`](/Develop/References/MessageInterfaces/Clova.md#ExpectLogin)              | CLOVA가 사용자로부터 {{ book.ServiceEnv.OrientedService }} 계정 인증(login)을 받도록 클라이언트에 지시합니다.          |
| Clova              | [`FinishExtension`](/Develop/References/MessageInterfaces/Clova.md#FinishExtension)      | CLOVA가 특정 Extension을 종료하도록 클라이언트에 지시합니다.                                             |
| Clova              | [`HandleDelegatedEvent`](/Develop/References/MessageInterfaces/Clova.md#HandleDelegatedEvent) | CLOVA가 CLOVA 앱으로부터 [위임된 사용자의 요청을 처리](/Develop/Guides/Handle_Delegation.md)하도록 클라이언트에 지시합니다.   |
| Clova              | [`Hello`](/Develop/References/MessageInterfaces/Clova.md#Hello)                          | 클라이언트에 downchannel 연결 설정이 완료되었음을 알립니다.                                       |
| Clova              | [`Help`](/Develop/References/MessageInterfaces/Clova.md#Help)                            | CLOVA가 미리 준비해 둔 도움말 정보를 제공하도록 클라이언트에 지시합니다.                                       |
| Clova              | [`LaunchURI`](/Develop/References/MessageInterfaces/Clova.md#LaunchURI)     | CLOVA가 URI로 표현되는 사이트 혹은 앱을 열거나 실행하도록 클라이언트에 지시합니다.       |
| Clova              | [`RenderTemplate`](/Develop/References/MessageInterfaces/Clova.md#RenderTemplate)        | CLOVA가 템플릿을 표시하도록 클라이언트에 지시합니다.                                                     |
| Clova              | [`RenderText`](/Develop/References/MessageInterfaces/Clova.md#RenderText)                | CLOVA가 텍스트를 표시하도록 클라이언트에 지시합니다.                                                     |
| Clova              | [`StartExtension`](/Develop/References/MessageInterfaces/Clova.md#StartExtension)        | CLOVA가 특정 Extension을 시작하도록 클라이언트에 지시합니다.                                             |
| DeviceControl      | [`BtConnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtConnect)          | CLOVA가 특정 블루투스 기기와 연결을 설정하도록 클라이언트에 지시합니다.                                       |
| DeviceControl      | [`BtConnectByPINCode`](/Develop/References/MessageInterfaces/DeviceControl.md#BtConnectByPINCode) | CLOVA가 PIN 코드를 요청한 블루투스 기기와 연결하도록 클라이언트에 지시합니다.                      |
| DeviceControl      | [`BtDelete`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDelete)            | CLOVA가 블루투스 페어링 목록에서 특정 기기를 제거하도록 클라이언트에 지시합니다.                        |
| DeviceControl      | [`BtDisconnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDisconnect)    | CLOVA가 특정 블루투스 기기와 연결을 해제하도록 클라이언트에 지시합니다.                                       |
| DeviceControl      | [`BtPlay`](/Develop/References/MessageInterfaces/DeviceControl.md#BtPlay)                | CLOVA가 연결된 블루투스 기기를 통해 음악을 재생하도록 클라이언트에 지시합니다.                          |
| DeviceControl      | [`BtRescan`](/Develop/References/MessageInterfaces/DeviceControl.md#BtRescan)            | CLOVA가 블루투스 기기를 재탐지(rescan)하도록 클라이언트에 지시합니다.                               |
| DeviceControl      | [`BtStartPairing`](/Develop/References/MessageInterfaces/DeviceControl.md#BtStartPairing) | CLOVA가 블루투스 페어링을 시작하도록 클라이언트에 지시합니다.                                              |
| DeviceControl      | [`BtStopPairing`](/Develop/References/MessageInterfaces/DeviceControl.md#BtStopPairing)   | CLOVA가 블루투스 페어링을 중지하도록 클라이언트에 지시합니다.                                              |
| DeviceControl      | [`Decrease`](/Develop/References/MessageInterfaces/DeviceControl.md#Decrease)             | CLOVA가 스피커 볼륨 또는 화면 밝기를 기본 단위만큼 줄이도록 클라이언트에 지시합니다.                            |
| DeviceControl      | [`ExpectReportState`](/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState) | CLOVA가 기기의 현재 상태를 CIC로 보고하도록 클라이언트에 지시합니다.                                  |
| DeviceControl      | [`Increase`](/Develop/References/MessageInterfaces/DeviceControl.md#Increase)             | CLOVA가 스피커 볼륨 또는 화면 밝기를 기본 단위만큼 높이도록 클라이언트에 지시합니다.                            |
| DeviceControl      | [`LaunchApp`](/Develop/References/MessageInterfaces/DeviceControl.md#LaunchApp)           | **(Deprecated)** CLOVA가 특정 앱을 실행하도록 클라이언트에 지시합니다.                                                    |
| DeviceControl      | [`Open`](/Develop/References/MessageInterfaces/DeviceControl.md#Open)                     | CLOVA가 특정 화면을 표시하도록 클라이언트에 지시합니다.  |
| DeviceControl      | [`OpenScreen`](/Develop/References/MessageInterfaces/DeviceControl.md#OpenScreen)         | **(Deprecated)** CLOVA가 설정 화면을 열도록 클라이언트에 지시합니다.                                                     |
| DeviceControl      | [`RenderDeviceList`](/Develop/References/MessageInterfaces/DeviceControl.md#RenderDeviceList)       | CLOVA가 사용자 계정에 등록된 클라이언트 기기 목록을 표시하도록 클라이언트에 지시합니다.  |
| DeviceControl      | [`SetValue`](/Develop/References/MessageInterfaces/DeviceControl.md#SetValue)            | CLOVA가 스피커 볼륨 또는 화면 밝기를 지정한 값으로 설정하도록 클라이언트에 지시합니다.                           |
| DeviceControl      | [`SynchronizeState`](/Develop/References/MessageInterfaces/DeviceControl.md#SynchronizeState) | CLOVA가 사용자 계정에 등록된 또 다른 클라이언트 기기의 상태를 업데이트하도록 클라이언트에 지시합니다.           |
| DeviceControl      | [`TurnOff`](/Develop/References/MessageInterfaces/DeviceControl.md#TurnOff)               | CLOVA가 지정한 기능이나 모드를 끄거나 비활성화하도록 클라이언트에 지시합니다.                                  |
| DeviceControl      | [`TurnOn`](/Develop/References/MessageInterfaces/DeviceControl.md#TurnOn)                 | CLOVA가 지정한 기능을 켜거나 활성화하도록 클라이언트에 지시합니다.                                          |
| Notifier           | [`ClearIndicator`](/Develop/References/MessageInterfaces/Notifier.md#ClearIndicator)      | CLOVA가 알림을 나타내는 표시를 모두 끄도록 클라이언트에 지시합니다.                                         |
| Notifier           | [`Notify`](/Develop/References/MessageInterfaces/Notifier.md#Notify)                      | CLOVA가 알림 내용을 사용자에게 전달하도록 클라이언트에 지시합니다.                                          |
| Notifier           | [`SetIndicator`](/Develop/References/MessageInterfaces/Notifier.md#SetIndicator)          | CLOVA가 사용자가 읽지 않은 알림이 있음을 표시하도록 클라이언트에 지시합니다.                                  |
| PlaybackController | [`ExpectNextCommand`](/Develop/References/MessageInterfaces/PlaybackController.md#ExpectNextCommand)         | CLOVA가 [`PlaybackController.NextCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#NextCommandIssued) 이벤트 메시지를 CIC로 보내도록 클라이언트에 지시합니다.  |
| PlaybackController | [`ExpectPauseCommand`](/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPauseCommand)       | CLOVA가 [`PlaybackController.PauseCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued) 이벤트 메시지를 CIC로 보내도록 클라이언트에 지시합니다.  |
| PlaybackController | [`ExpectPlayCommand`](/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPlayCommand)         | CLOVA가 [`PlaybackController.PlayCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#PlayCommandIssued) 이벤트 메시지를 CIC로 보내도록 클라이언트에 지시합니다.  |
| PlaybackController | [`ExpectPreviousCommand`](/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPreviousCommand) | CLOVA가 [`PlaybackController.PreviousCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#PreviousCommandIssued) 이벤트 메시지를 CIC로 보내도록 클라이언트에 지시합니다.  |
| PlaybackController | [`ExpectResumeCommand`](/Develop/References/MessageInterfaces/PlaybackController.md#ExpectResumeCommand)     | CLOVA가 [`PlaybackController.ResumeCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#ResumeCommandIssued) 이벤트 메시지를 CIC로 보내도록 클라이언트에 지시합니다.  |
| PlaybackController | [`ExpectStopCommand`](/Develop/References/MessageInterfaces/PlaybackController.md#ExpectStopCommand)         | CLOVA가 [`PlaybackController.StopCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#StopCommandIssued) 이벤트 메시지를 CIC로 보내도록 클라이언트에 지시합니다.  |
| PlaybackController | [`Mute`](/Develop/References/MessageInterfaces/PlaybackController.md#Mute)               | CLOVA가 스피커 볼륨을 음소거하도록 클라이언트에 지시합니다.                                                |
| PlaybackController | [`Next`](/Develop/References/MessageInterfaces/PlaybackController.md#Next)               | CLOVA가 재생 대기열에 있는 다음 오디오 스트림을 재생하도록 클라이언트에 지시합니다.                               |
| PlaybackController | [`Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause)             | CLOVA가 재생 중인 오디오 스트림을 일시 정지하도록 클라이언트에 지시합니다.                                    |
| PlaybackController | [`Previous`](/Develop/References/MessageInterfaces/PlaybackController.md#Previous)       | CLOVA가 재생 대기열에 있는 이전 오디오 스트림을 재생하도록 클라이언트에 지시합니다.                              |
| PlaybackController | [`Replay`](/Develop/References/MessageInterfaces/PlaybackController.md#Replay)           | CLOVA가 오디오 스트림을 처음부터 다시 재생하도록 클라이언트에 지시합니다.                                     |
| PlaybackController | [`Resume`](/Develop/References/MessageInterfaces/PlaybackController.md#Resume)           | CLOVA가 오디오 스트림 재생을 재개하도록 클라이언트에 지시합니다.                                            |
| PlaybackController | [`SetRepeatMode`](/Develop/References/MessageInterfaces/PlaybackController.md#SetRepeatMode) | CLOVA가 지정된 반복 모드로 재생 상태를 변경하도록 클라이언트에 지시합니다.                                |
| PlaybackController | [`Stop`](/Develop/References/MessageInterfaces/PlaybackController.md#Stop)               | CLOVA가 오디오 스트림 재생을 중지하도록 클라이언트에 지시합니다.                                            |
| PlaybackController | [`TurnOffRepeatMode`](/Develop/References/MessageInterfaces/PlaybackController.md#TurnOffRepeatMode) | **(Deprecated)** CLOVA가 한 곡 반복 재생 모드를 끄도록 클라이언트에 지시합니다.                |
| PlaybackController | [`TurnOnRepeatMode`](/Develop/References/MessageInterfaces/PlaybackController.md#TurnOnRepeatMode) | **(Deprecated)** CLOVA가 한 곡 반복 재생 모드를 켜도록 클라이언트에 지시합니다.                  |
| PlaybackController | [`Unmute`](/Develop/References/MessageInterfaces/PlaybackController.md#Unmute)           | CLOVA가 스피커 볼륨의 음소거를 해제하도록 클라이언트에 지시합니다.                                           |
| PlaybackController | [`VolumeDown`](/Develop/References/MessageInterfaces/PlaybackController.md#VolumeDown)   | **(Deprecated)** CLOVA가 스피커 볼륨을 낮추도록 클라이언트에 지시합니다.                                                   |
| PlaybackController | [`VolumeUp`](/Develop/References/MessageInterfaces/PlaybackController.md#VolumeUp)       | **(Deprecated)** CLOVA가 스피커 볼륨을 높이도록 클라이언트에 지시합니다.                                                   |
| Settings           | [`ExpectReport`](/Develop/References/MessageInterfaces/Settings.md#ExpectReport)                                                 | CLOVA가 현재의 설정 정보를 보고하도록 클라이언트에 지시합니다. |
| Settings           | [`Update`](/Develop/References/MessageInterfaces/Settings.md#Update)                                                             | CLOVA가 `payload`에 포함된 내용을 설정에 반영하도록 클라이언트에 지시합니다.  |
| Settings           | [`UpdateVersionSpec`](/Develop/References/MessageInterfaces/Settings.md#UpdateVersionSpec)  | CLOVA가 `payload`에 포함된 설정 항목을 설정 메뉴에 반영하도록 클라이언트에 지시합니다. |
| SpeechRecognizer   | [`ExpectSpeech`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech) | CLOVA가 사용자의 음성 입력을 대기하도록 클라이언트에 지시합니다.                                            |
| SpeechRecognizer   | [`KeepRecording`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#KeepRecording) | CLOVA가 음성 입력을 계속 받도록 클라이언트에 지시합니다.                                                |
| SpeechRecognizer   | [`ShowRecognizedText`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#ShowRecognizedText) | CLOVA가 인식된 사용자 음성을 실시간으로 표시하도록 클라이언트에 지시합니다.                 |
| SpeechRecognizer   | [`StopCapture`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#StopCapture)   | CLOVA가 사용자의 음성 인식을 중지하도록 클라이언트에 지시합니다.                                            |
| SpeechSynthesizer  | [`Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak)                 | CLOVA가 합성된 TTS를 스피커로 출력하도록 클라이언트에 지시합니다.                                |
| System             | [`SynchronizeState`](/Develop/References/MessageInterfaces/System.md#SynchronizeState) | CLOVA가 `payload`에 있는 데이터를 동기화하도록 클라이언트에 지시합니다.                                   |
| TemplateRuntime    | [`ExpectRequestPlayerInfo`](/Develop/References/MessageInterfaces/TemplateRuntime.md#ExpectRequestPlayerInfo)  | CLOVA가 재생 메타 정보를 요청하도록 클라이언트에 지시합니다.   |
| TemplateRuntime    | [`RenderPlayerInfo`](/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlayerInfo)                | CLOVA가 미디어 플레이어에 표시할 재생 목록, 앨범 이미지, 가사와 같은 재생 메타 정보를 전달하고 이를 표시하도록 클라이언트에 지시합니다. |
| TemplateRuntime    | [`RenderPlaylist`](/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlaylist)                | CLOVA가 재생목록 화면을 표시하도록 클라이언트에 지시합니다. |
| TemplateRuntime    | [`UpdateLike`](/Develop/References/MessageInterfaces/TemplateRuntime.md#UpdateLike)                            | CLOVA가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대한 사용자의 좋아요(like) 표시 상태를 지정된 값으로 업데이트하도록 클라이언트에 지시합니다.  |
| TemplateRuntime    | [`UpdateSubscribe`](/Develop/References/MessageInterfaces/TemplateRuntime.md#UpdateSubscribe)                  | CLOVA가 클라이언트의 미디어 플레이어에서 특정 미디어 콘텐츠에 대한 사용자의 구독(subscribe) 표시 상태를 지정된 값으로 업데이트하도록 클라이언트에 지시합니다.  |