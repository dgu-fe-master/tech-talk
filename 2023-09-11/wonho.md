# Enum vs Union Type

## 배경
최근 참여한 프로젝트에서 재사용하는 모달을 타입에 따라 모달의 내용을 변화시키게 하였습니다.
```ts
export enum ModalType {
  CLOSE = "CLOSE",
  CHANGE_TRIM = "CHANGE_TRIM",
  CHANGE_ENGINE = "CHANGE_ENGINE",
  CONFIRM = "CONFIRM",
  CHANGE_SEARCH_TRIM = "CHANGE_SEARCH_TRIM",
  ALERT = "ALERT",
}
```
당시에는 몰랐는데, enum을 많이 사용하는지에 대해 궁금해졌고, 이에 대해 알아보게 되었습니다!

## 요약
### enum의 문제점

enum을 사용하면 컴파일 시 객체가 생성되고, 이는 런타임에 영향을 줍니다!

또한, js로 트랜스파일되는 과정에서 rollup 번들러에서 ``TreeShaking``을 못하는 문제가 있다고 하네요. 

(webpack에서는 되는걸로 보임!!)

### const enum

enum의 문제점을 해결하는 첫번째 방법은 const enum을 사용하는 것입니다! <br/>

const enum을 사용하면 트리 쉐이킹도 잘 되며 직접 매핑이 이루어지게 됩니다!

하지만 const enum은 Babel로 트랜스파일 될 수 없다고 하네요..

### union type

그래서 여러 자료를 찾아보았을 때, 가장 권장하는 것은 ``union type``이라고 합니다!

union type을 사용하면 위의 문제를 모두 해결할 수 있고,

enum에서의 number type 사용시 문제도 해결할 수 있다고 합니다!

또한, import를 해야하는 enum과 달리, import를 사용하지 않아도 되어 생산성도 뛰어나다고 합니다!

## 참고자료
https://fe-developers.kakaoent.com/2021/211012-typescript-tip/
https://velog.io/@sensecodevalue/Typescript-Enum-%EC%99%9C-%EC%93%B0%EC%A7%80-%EB%A7%90%EC%95%84%EC%95%BC%ED%95%98%EC%A3%A0
