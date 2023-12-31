# 유닛 테스트

## 배경

그동안 프로젝트를 하면서 테스트 코드를 작성해보지 않았고, 그 이유는 다음과 같습니다.

- 기획이 바뀔 가능성이 많은 프로젝트라 코드의 생명주기가 짧음
- 서비스의 안정성 보다는 일단 빠른 MVP 개발이 우선
- 코드의 복잡도가 높지 않음

테스트 코드를 작성하는 방법에 대해 공부를 해보고싶었던 내용이었는데 그동안의 프로젝트에서는 필요성을 느끼지 못하여 테스트 코드를 작성하지 않았습니다.
이번에 회사에서 일을 하면서 테스트 코드를 작성해야겠다는 생각이 들었고, 자세한 내용은 아래에서 설명하겠습니다.

## 요약

### 유닛 테스트란?

유닛 테스트는 애플리케이션을 구성하는 하나의 기능이 올바르게 동작하는지를 독립적으로 테스트 하는것으로 "어떤 기능이 실행되면 어떤 결과가 나온다" 정도로 테스트를 진행한다.

### 왜 필요한가?

- 간단한 기능인데도 기능을 수정하거나 추가하는 것이 어려움
- 리팩터링을 하고 싶은데 기존 기능을 훼손할까봐 리팩터링이 두려움
- 어떻게 동작하는지 기능을 파악하기가 시간이 오래 걸림
- 테스트를 할 때 계속 테스트 공고에 지원해보면서 테스트하는것이 비효율적이라고 생각함
- 공고마다 입력받는 정보나 특징이 다 달라서 특정 공고에만 들어가는 기능이 불가피하게 생겨 여러가지의 경우를 테스트 코드없이 테스트하기가 어려움

그래서 지원부분만이라도 유닛테스트를 도입해야겠다고 생각했습니다.

### 어떻게 작성하는가?

1. 기존의 동작하는 코드를 기반으로 사용자 시나리오를 작성한다.
2. 사용자 시나리오를 기반으로 테스트코드를 작성한다.
3. 리팩터링

### 기대효과

- 작업자가 자주 바뀌기 때문에 테스트 시나리오만으로도 어떻게 동작하는지 어떠한 케이스들이 있는지 파악하기 좋을것 같다. (실제 내가 인수인계 받을 때 힘들었던 부분 중 하나)
- 리팩터링, 기능 수정 추가에 수월
- 테스트 시간 감소 (계속 지원해보면서 테스트를 하는데 단위 테스트를 하고 테스트 코드가 통과되면 QA식으로 테스트 해보는 방식으로 될 것 같다.)

## 결론

이전에는 실제 현업에서도 테스트코드가 필요할까? 라는 생각을 했었습니다.
사이드 프로젝트 수준이 아니라 생각보다 코드가 복잡하고, 유지보수하는게 어렵다고 생각이 들어서 테스트코드가 정말 필요하다고 생각이 들었습니다.
테스트코드를 점진적으로 작성해볼 생각이고, 작성해보고 장단점과 느낀점에 대해 공유해보겠습니다 !

## 참고 자료

- https://techblog.woowahan.com/8942/
