# flushSync란 무엇일까?

## 배경

React 공식 문서를 찾아보던 도중, flushSync라는 API를 발견했습니다. 이게 뭐지? 하며 바로 들어갔는데

`Using flushSync is uncommon and can hurt the performance of your app.` 

라고 쓰여있어서 놀랐는데요..

react-dom의 `flushSync`를 사용하면 Auto Batching을 무시하고 보류중인 작업을 강제로 flush하고 DOM을 동기적으로 업데이트할 수 있습니다!

```tsx
import { useState, useEffect } from 'react';
import { flushSync } from 'react-dom';

export default function PrintApp() {
  const [isPrinting, setIsPrinting] = useState(false);

  useEffect(() => {
    function handleBeforePrint() {
      flushSync(() => {
        setIsPrinting(true);
      })
    }

    function handleAfterPrint() {
      setIsPrinting(false);
    }

    window.addEventListener('beforeprint', handleBeforePrint);
    window.addEventListener('afterprint', handleAfterPrint);
    return () => {
      window.removeEventListener('beforeprint', handleBeforePrint);
      window.removeEventListener('afterprint', handleAfterPrint);
    }
  }, []);

  return (
    <>
      <h1>isPrinting: {isPrinting ? 'yes' : 'no'}</h1>
      <button onClick={() => window.print()}>
        Print
      </button>
    </>
  );
}
```
해당 코드에서 flushSync를 사용하면 인쇄 창이 열리기 전에 페이지 변경이 가능하여

isPrinting을 yes로 변경할 수 있습니다!

## 요약

### Auto Batching을 무시하고 렌더링한다!

- createRoot를 기반으로 렌더링을 진행하면 모든 state update 작업은 Auto Bathcing 됩니다!

### 강제로 Flush 하는 것이기에 공식문서에서 필요할 때만 쓰라고 강조한다!!

## 참고 자료
- https://react.dev/reference/react-dom/flushSync
- https://velog.io/@rookieand/React-18%EC%97%90%EC%84%9C-%EC%B6%94%EA%B0%80%EB%90%9C-Auto-Batching-%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80#:~:text=%EB%90%98%EC%97%88%EB%8B%A4%EB%8A%94%20%EC%A0%90%EC%9D%B4%EB%8B%A4.-,%E2%9C%8F%EF%B8%8F%20ReactDOM.flushSync()%20%EB%9E%80%3F,%EC%A6%89%EC%8B%9C%20DOM%EC%9D%84%20%EB%A0%8C%EB%8D%94%EB%A7%81%ED%95%B4%EC%A4%80%EB%8B%A4.
