# 맥락 정보(Context)

맥락 정보(Context)는 클라이언트의 다양한 상태 정보를 의미합니다. 맥락 정보는 context objects를 통해 표현되며, CIC의 API인 [이벤트 메시지](/Develop/References/CIC_API.md#Event)를 보낼 때 포함됩니다. 맥락 정보는 사용자가 발화한 시점의 클라이언트 상태 정보를 담아야 하며, **작성 가능한 클라이언트의 모든 상태 정보를 맥락 정보로 나타내야 합니다.** 다음과 같은 context objects가 있습니다.

* [`Alerts.AlertsState`](#AlertsState)
* [`AudioPlayer.PlaybackState`](#PlaybackState)
* [`Clova.Location`](#Location)
* [`Clova.SavedPlace`](#SavedPlace)
* [`Device.Audio`](#Audio)
* [`Device.DeviceState`](#DeviceState)
* [`Device.Display`](#Display)
* [`Speaker.VolumeState`](#VolumeState)
* [`SpeechSynthesizer.SpeechState`](#SpeechState)

{% include "/Develop/References/ContextObjects/AlertsState.md" %}

{% include "/Develop/References/ContextObjects/PlaybackState.md" %}

{% include "/Develop/References/ContextObjects/Location.md" %}

{% include "/Develop/References/ContextObjects/SavedPlace.md" %}

{% include "/Develop/References/ContextObjects/Audio.md" %}

{% include "/Develop/References/ContextObjects/DeviceState.md" %}

{% include "/Develop/References/ContextObjects/Display.md" %}

{% include "/Develop/References/ContextObjects/VolumeState.md" %}

{% include "/Develop/References/ContextObjects/SpeechState.md" %}
