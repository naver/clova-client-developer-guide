CLOVA는 사용자가 알람을 생성하면 생성한 알람의 정보를 Alarm 템플릿 형태로 클라이언트에 전달합니다. 클라이언트는 이 템플릿을 사용하여 사용자가 생성한 알람 정보를 화면에 표시해야 합니다.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>Alarm 템플릿은 현재 다음과 같은 제약 사항이 있습니다.</p>
  <ul>
    <li>사용자는 음성으로 알람 등록과 알람 목록 조회만 요청할 수 있습니다.</li>
    <li>사용자가 알람를 수정하거나 삭제하려면 CLOVA 앱을 이용해야 합니다.</li>
  </ul>
</div>

다음은 landscape 화면 형태에서 Alarm 템플릿의 내용을 표현한 UI 예제입니다.

![Content_Template-Alarm](/Develop/Assets/Images/Content_Template-Alarm.png)

참고로 알람을 울려야 할 때 화면 UI 예제는 다음과 같습니다.

* 예제 1

  ![Content_Template-Alarm_Goes_Off-Example1](/Develop/Assets/Images/Content_Template-Alarm_Goes_Off-Example1.png)

* 예제 2

  ![Content_Template-Alarm_Goes_Off-Example2](/Develop/Assets/Images/Content_Template-Alarm_Goes_Off-Example2.png)