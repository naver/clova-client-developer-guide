CLOVA는 사용자가 타이머의 목록을 요청하면 사용자에게 등록된 타이머의 목록을 TimerList 템플릿 형태로 클라이언트에 전달합니다. 클라이언트는 이 템플릿을 사용하여 사용자가 등록한 타이머 목록을 화면에 표시해야 합니다.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>TimerList 템플릿은 현재 다음과 같은 제약 사항이 있습니다.</p>
  <ul>
    <li>사용자는 음성으로 타이머 등록과 타이머 목록 조회만 요청할 수 있습니다.</li>
    <li>사용자가 타이머를 수정하거나 삭제하려면 CLOVA 앱을 이용해야 합니다.</li>
  </ul>
</div>

다음은 landscape 화면 형태에서 TimerList 템플릿의 내용을 표현한 UI 예제입니다.

![Content_Template-TimerList](/Develop/Assets/Images/Content_Template-TimerList.png)