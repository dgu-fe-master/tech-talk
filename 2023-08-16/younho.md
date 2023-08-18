# if - else 클린 코드

## 배경

커리어리 주간 인기 TOP 10에 "'if-else' 보다는 'if-if'. 코드 가독성의 중요성"이라는 글이 예전에 올라왔었어요.

저도 if-else 문은 가독성이 좋지 않아 최대한 덜 사용하려고 했고, if-if 문과 같은 early return 문을 자주 사용했어요!

구체적인 이유는

- else 문이 어떤 조건에서 실행되지는 파악하기 어려울 때가 있다
- 로직이 중첩됐을 때 if-else보다 if-if가 로직을 한눈에 파악하기 용이하다.

그런데 댓글에 `<신입 또는 주니어 레벨에서 무조건 안 쓰려는 생각을 가지시는 분들이 많아지는 것 같아 주의가 필요해 보여요! 그저 방법의 하나일 뿐입니다~!>`라는 의견이 있어서 이번 주제로 고민해 봤어요

## 요약

### if-if가 좋은지 if-else가 좋은지

해당 글의 댓글의 의견을 정리해 보자면 무조건은 없고, `상황에 따라 원리를 이해하고 사용하는 것이 중요하다~` 정도 같아요

- 일반적일 때에서 if else를 if if로 무조건 바꿔서 쓰자는 것이 아니다. "예외 case에 대해서 독립된 if 문으로 분리해서 처리하자"가 맞는 것 같다
- 무조건 안 쓰는 것이 좋다기보다기 보다 원리를 알아야 한다
- nested if 문을 최소화하기 위한 early return의 중요성과, else 대신 Guard 문을 우선시하는 접근법을 지향하자
- 상황에 따라 if else가 가독성 면에서도 좋을 때가 있다.

### early return

그래도,,! 저는 대부분의 경우 아래와 같이 else 문을 생략하면 해결됐는데, 다른 분들 의견도 궁금해요.

```js
// 이거보단
if (조건) {
  // A
} else {
  // B
}
// 아래를 선호
if (조건) {
  // A
  return;
}
// B
```

### 객체로 리팩토링

분기가 많은 if 문의 경우 switch-case를 사용할 수 있는데요, 저는 Object를 활용해서 리팩토링하는 걸 선호해요.

### 중첩 조건문 처리

if 문은 최대한 중첩되지 않게 처리하려고 노력해요. 앞에서 말한 `early return`, `객체  매핑` 등의 방법으로 If 문을 줄이고 선언적으로 처리하려고 해요!

## 참고 자료

> 구체적인 예시는 아래 블로그랑 정재남님 유튜브 참고해서 보시면 좋을 것 같아요

- https://careerly.co.kr/comments/88380
- https://velog.io/@jangws/9.-Else-if와-Else를-피하고-Early-Return하자
- [FE재남 - [Javascript 미세팁] if else 리팩토링 #1 (feat. 객체 + 함수)](https://www.youtube.com/watch?v=p0YEviesgRM)
- [FE재남 - [Javascript 미세팁] if else 리팩토링 #2 중첩 조건문 처리하기](https://www.youtube.com/watch?v=4I-MiMmZdcQ)
