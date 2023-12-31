# Service Worker

## 배경

프로젝트를 진행하면서 MSW의 필요성에 대해 느꼈습니다. 그러면서 Service Worker란 무엇인지에 대해 궁금증이 생기게 되었고
자료를 찾아보게 되었습니다.


## 요약

### Service Worker?

특정 사이트의 하나 혹은 그 이상의 페이지를 제어하는 스크립트이며, 이벤트 기반 워커로 JavaScript로 작성된 파일.<br/>
자신이 제어하는 페이지에서 발생하는 이벤트를 수신할 수 있음.<br/>
네트워크 요청과 같은 이벤트를 가로채 수정할 수 있고, 다시 페이지로 돌려보낼 수 있음. <br/>
또한, https에서만 실행된다는 특징이 있음!

### 사용 이유
- 오프라인 지원
- 캐싱을 통한 빠른 로딩 속도
- 백그라운드 작업
- PWA

### 생명 주기

1. Installing
  - navigator.serviceWorker.register를 통해 서비스 워커 등록
  - JS 파일이 다운되고 파싱되고나면 서비스 워커는 Installing 상태
2. Installed / Waiting
  - 서비스 워커가 성공적으로 설치되면 Installed 상태
  - 활성화된 서비스 워커가 없다면 즉시 Activating 상태
  - 다른 서비스 워커가 앱을 제어중이라면 Waiting 상태
3. Activating
  - 서비스 워커가 활성화되기 직전의 상태
4. Activated
  - 서비스 워커가 활성화된 상태, 앱을 제어할 수 있고 이벤트를 받아서 처리 가능
5. Redundant
  - 서비스 워커가 등록, 설치중에 실패하거나 새로운 버전으로 교체되면 Redundant 상태
  - 앱에 아무런 영향을 미칠 수 없음.

## 결론

MSW를 통해 백엔드에 의존적으로 개발하는 것이 아니라 능동적으로 개발할 수 있는 환경을 구성해본 적은 있지만, <br/>
이를 넘어서 다양한 Service Worker 사용 예시에 대해 알아보고 프로젝트에 적용해볼 예정입니다!<br/>
(다음은 PWA..?)
