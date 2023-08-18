# 자바스크립트 Closure와 React Hooks 사이의 관계

## 배경

황준일 님의 개발 블로그에 작성된 포스팅 가운데 [Vanilla Javascript로 React UseState Hook 만들기](https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Make-useSate-hook/#_1-%E1%84%8B%E1%85%B4%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%B3%E1%86%AF-%E1%84%80%E1%85%A1%E1%86%BD%E1%84%80%E1%85%B5)를 보게 되었어요. 그동안 React 함수형 컴포넌트를 이용한 개발은 많이 해봤지만 React Hook이 어떤 식으로 동작하는지는 알아본 적이 없는 것 같았어요. 그래서 React Hook은 실제로 어떻게 동작하는지 궁금해졌고, 찾아보다보니 클로저와의 관계성을 발견하여 클로저 개념을 중심으로 동작 방식을 학습해보았어요.

## 요약

### 함수형 컴포넌트의 상태 관리

함수형 컴포넌트는 렌더링이 발생하면 함수 자체가 다시 호출됩니다. 그렇기 때문에 상태를 관리하려면 함수가 실행됐을 때 이전 상태에 대한 정보를 가지고 있어야 하고, React는 이 과정에서 클로저 개념을 이용하고 있어요.

### 클로저 개념

> 클로저는 함수와 그 함수가 선언됐을 때의 렉시컬 환경(Lexical Environment)의 조합이다. - MDN

클로저의 정의를 다르게 표현하면,
클로저란 함수가 속한 렉시컬 스코프를 기억하여 렉시컬 스코프 밖에서 실행될 때에도 이 스코프에 접근할 수 있게 하는 기능으로 정의할 수 있어요.

```js
const add = (function getAdd() {
    let foo = 1;

    return function () {
        foo += 1;

        return foo;
    };
})();

console.log(add()); // 2
console.log(add()); // 3
console.log(add()); // 4
```

### 실제 useState는 어떻게 구현되어 있는가?

실제 React의 useState 함수는 다음과 같이 구현되어 있어요. (`node_modules/react/cjs/react.development.js` or [ReactHooks.js](https://github.com/facebook/react/blob/main/packages/react/src/ReactHooks.js))

<img src="https://velog.velcdn.com/images/hyewon_kkang/post/b5885545-30d2-47ca-b6f8-b33e40ce8d16/image.png" width="400" />

`useState`는 초기값을 넘겨받아 `dispatcher` 객체의 `useState` 함수에 넘겨줍니다.
`dispatcher`를 반환하는 `resolveDispatcher()` 함수는 아래와 같아요.

![](https://velog.velcdn.com/images/hyewon_kkang/post/4c765d20-afe1-4065-956a-e09927845f31/image.png)

`resolveDispatcher()` 함수는 `ReactCurrentDispatcher`라는 객체의 current 프로퍼티를 반환하고 있어요. (중간의 if 조건문은 우리가 Hook을 컴포넌트 외부에서 사용했을 때 에러를 발생시키는 부분이에요.)
`ReactCurrentDispatcher`는 다음과 같이 구현되어 있어요.

<img src="https://velog.velcdn.com/images/hyewon_kkang/post/54702d5b-e28c-46fb-872e-4f98e75af3fa/image.png" width="400" />

즉, `ReactCurrentDispatcher`는 단지 **전역**에 선언된 current라는 속성을 가지는 변수일 뿐이에요.

지금까지의 과정을 요약하면 다음과 같이 정리할 수 있어요.

> 1. useState(를 포함한 모든 Hook)는 React 모듈에 선언되어 있는 함수로,
> 2. useState가 실행될 때마다 dispatcher를 선언하고 useState 메서드를 실행해 그 값을 반환받는다.
> 3. dispatcher는 전역 변수 ReactCurrentDispatcher로부터 가져온다.

결론적으로, **함수형 컴포넌트가 `클로저`를 통해 선언되는 시점에 접근 가능했던 외부 상태 값에 계속 접근할 수 있는 것**이에요.

(useState 내부에 대한 더 자세한 설명은 [여기](https://kyoung-jnn.com/posts/react-useState)서 확인할 수 있어요.)

### useState 간단히 구현해보기

```jsx
let value_;

const React = (function () {
    function useState(initialState) {
        const state = value_ || initialState;
        const setState = (newState) => {
            value_ = newState;
        };
        return [state, setState];
    }

    function render(Component) {
        const C = Component();
        C.render();
        return C;
    }

    return { render, useState };
})();

function Component() {
    const [count, setCount] = React.useState(0);

    return {
        render: () => console.log(count),
        increase: () => setCount(count + 1),
    };
}

var App = React.render(Component); // 0
App.increase();
var App = React.render(Component); // 1
```

여러 개의 Hook을 가진다면 어떻게 수정할 수 있을까요?
React는 state를 useState 외부에 배열 형식으로 관리하고 있어요. 따라서 배열과 인덱스를 통해 위 코드를 변경하면 다음과 같이 수정할 수 있어요.

```jsx
const React = (function () {
    let hooks = [];
    let idx = 0;

    function useState(initialState) {
        const state = hooks[idx] || initialState;
        const _idx = idx;
        const setState = (newState) => {
            hooks[_idx] = newState;
        };

        idx++;
        return [state, setState];
    }

    function render(Component) {
        idx = 0;
        const C = Component();
        C.render();
        return C;
    }

    return { render, useState };
})();

function Component() {
    const [count, setCount] = React.useState(0);
    const [text, setText] = React.useState('apple');

    return {
        render: () => console.log({ count, text }),
        increase: () => setCount(count + 1),
        type: (word) => setText(word),
    };
}

var App = React.render(Component); // { count: 0, text: 'apple' }
App.increase();
var App = React.render(Component); // { count: 1, text: 'apple' }
App.type('orange');
var App = React.render(Component); // { count: 1, text: 'orange' }
```

### Hook의 규칙

![](https://velog.velcdn.com/images/hyewon_kkang/post/78c256f7-30b5-412a-bee8-22ae3cdfa190/image.png)

위 구현에서 확인한 것과 같이, hook은 배열 내에 순서대로 저장되고, React는 hook이 호출되는 순서에 의존하기 때문에 항상 최상위 레벨에서 사용해야만 올바른 값을 보장받을 수 있다는 것을 알 수 있어요.

## 결론

-   컴포넌트 함수가 다시 실행(렌더링) 되더라도 상태 값이 초기화되지 않는다. 어떻게 가능할까? → 클로저
-   React에서 함수형 컴포넌트의 상태 관리를 위해서는 컴포넌트 외부에 저장된 값을 사용하며, 클로저를 통해 해당 값에 접근해 비교하고 변경한다.
-   useState는 컴포넌트 내부에서 값을 변경시키는 것이 아니라, 컴포넌트 외부의 값을 변경시키기 때문에 상태가 변경된 직후 컴포넌트가 가진 값은 이전의 값을 그대로 참조한다.
-   각 컴포넌트의 상태 정보는 순서대로 배열에 저장되기 때문에 Hook을 조건문이나 반복문 안에서 사용하면 잘못된 순서의 값을 참조하게 될 수 있다.

더 구체적인 내용은 [블로그](https://velog.io/@hyewon_kkang/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Closure%EC%99%80-React-Hooks-%EC%82%AC%EC%9D%B4%EC%9D%98-%EA%B4%80%EA%B3%84-aixsw5xf)에 정리해두었습니다 🐣

## 참고 자료

https://www.netlify.com/blog/2019/03/11/deep-dive-how-do-react-hooks-really-work/

[https://medium.com/humanscape-tech/자바스크립트-클로저로-hooks구현하기-3ba74e11fda7](https://medium.com/humanscape-tech/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%81%B4%EB%A1%9C%EC%A0%80%EB%A1%9C-hooks%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-3ba74e11fda7)

https://ko.legacy.reactjs.org/docs/hooks-rules.html

[https://velog.io/@ggong/useState-Hook과-클로저](https://velog.io/@ggong/useState-Hook%EA%B3%BC-%ED%81%B4%EB%A1%9C%EC%A0%80)

[https://seokzin.tistory.com/entry/React-useState의-동작-원리와-클로저](https://seokzin.tistory.com/entry/React-useState%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80)

https://www.rinae.dev/posts/getting-closure-on-react-hooks-summary

https://ingg.dev/hook-work/
