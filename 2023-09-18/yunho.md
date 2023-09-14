# 실행컨텍스트 예시 (React 컴포넌트 + 컴포넌트 아래에 변수 선언)

## 배경

지영님이 학습 대리 맡김.. [관련 PR 댓글](https://github.com/depromeet/www.depromeet.com/pull/255#discussion_r1320738616)

호이스팅 때문이라고 생각했는데, 이전에 자스 스터디할 때도 잘못 이해한 걸 업데이트하지 않은 이슈

- `const는 호이스팅 되어도 초기화가 되지않아서 에러`

```tsx
// Component.tsx
export function Component() {
  console.log(b);

  return <div></div>;
}

console.log(b);
const b = 3;
```

아래 react 코드에서 Component안에 b는 정상적으로 출력되는데 Component 밖의 b는 "Block-scoped variable 'b' used before its declaration.ts(2448)"에러를 반환해요.

component안의 b도 같은 에러를 반환해야할 것 같은데 왜 정상적으로 될까요?

## 요약

### 자바스크립트의 실행 컨텍스트와 라이프사이클에 관련된 문제!

- 함수 밖에서 정의된 console.log(b);는 스크립트가 로드되면 즉시 실행됩니다. 이 때 b는 정의되지 않았으므로 ReferenceError가 발생
- 함수 정의는 실행 전에 함수의 내부 스코프에 접근하지 않는다!
  - 따라서 함수 내부에서 정의되지 않은 변수를 가지고 있어도 함수 정의 시에는 문제가 없음!
- 함수는 호출되어야 실행 컨텍스트가 생성되며 변수 초기화가 진행되는데, 함수가 실행되기 전에 const b = 3;가 초기화되어 b에 값이 할당
- 리액트 컴포넌트 함수들은 렌더링 타이밍에 실행되므로, 일반적으로 함수가 정의되는 시점보다 먼저 실행되지 않아서 에러가 발생하지 않는다

### 흐름 정리

> “function Component()”정의 -> “console.log(b)” 외부 실행(에러) -> “const b = 3” 초기화 및 값 할당 -> 렌더링 시점에서 “Component()” 함수 실행(에러x)

우리가 파일을 볼 때는 위에서부터 아래로 읽으니까 그 순서대로 함수나 변수를 선언하고 사용될 것 같은데, 자바스크립트의 실행 컨텍스트와 함수 실행 시점에서 봐야해요!

그러니까 React 컴포넌트 위,아래 어디 변수를 선언해도 접근 가능했더 것이다~

그런데 가끔 아래 선언하면 에러 났던 것 같은데 원인이 뭘까요👀

## 참고 자료

- https://careerly.co.kr/qnas/4616?fa=notification
- https://stackoverflow.com/questions/75891792/how-variable-hoisting-works-in-react
- https://stackoverflow.com/questions/71792930/hoisting-in-react-components-how-does-it-work
