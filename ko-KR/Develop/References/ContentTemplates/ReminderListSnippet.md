CLOVA는 사용자가 리마인더의 목록을 요청하면 사용자에게 등록된 리마인더의 목록을 ReminderList 템플릿 형태로 클라이언트에 전달합니다. 클라이언트는 이 템플릿을 사용하여 사용자가 등록한 리마인더 목록을 화면에 표시해야 합니다.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>ReminderList 템플릿은 현재 다음과 같은 제약 사항이 있습니다.</p>
  <ul>
    <li>사용자는 음성으로 리마인더 등록과 리마인더 목록 조회만 요청할 수 있습니다.</li>
    <li>사용자가 리마인더를 수정하거나 삭제하려면 CLOVA 앱을 이용해야 합니다.</li>
  </ul>
</div>

다음은 landscape 화면 형태에서 ReminderList 템플릿의 내용을 표현한 UI 예제입니다.

![Content_Template-ReminderList](/Develop/Assets/Images/Content_Template-ReminderList.png)