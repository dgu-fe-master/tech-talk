
# Promise allSettled

## 배경

그동안 Promise all만 사실 알았는데요.
회사 코드에서 Promise allSettled를 보게 되었고, all과 차이점은 뭔지 왜 allSettled를 작성했는지 궁금해서 찾아보다가 Promise를 병렬처리할 때 allSettled의 필요성에 대해 알게되었습니다.

아시는 분들도 계실거라고 생각하지만 all 작성했을 때의 문제점에 대해 아셔야할 것 같아서 이 주제를 선정했습니다.

## 요약

### Promise all과 Promise allSettled 차이점

Promise all과 allSettled는 여러 프로미스 객체를 병렬적으로 진행합니다.

단 Promise.all의 경우, 하나의 프로미스라도 실패하면 에러로 처리되고,
Promise.allSettled는 여러 프로미스를 병렬적으로 처리하되, 하나의 프로미스가 실패해도 무조건 이행한다.

<img width="1074" alt="스크린샷 2023-08-18 오후 10 49 14" src="https://github.com/dgu-fe-master/tech-talk/assets/62633444/9a9a150f-4705-4ab8-840e-aca239c452a9">

### Promise all의 문제점

다음과 같이 비동기 api로 서버에 요청을 보낸다고 가정해보자.
나머지 요청은 문제없이 잘 되었는데, req3 요청만 에러가 생겨 reject되면 다시 요청을 보내야 되는데, 또 req 1 ~ 5 개의 요청을 한꺼번에 보내야 된다. 실패한 req3 요청만 다시 보내서 응답 받으면 되는데 비효율적으로 작업 처리를 할 수도 있다.

```
const req1 = axios.post('서버주소', { obj1 });
const req2 = axios.post('서버주소', { obj2 });
const req3 = axios.post('서버주소', { obj3 });
const req4 = axios.post('서버주소', { obj4 });
const req5 = axios.post('서버주소', { obj5 });
Promise.all([req1, req2, req3, req4, req5])
   .then((result) => console.log(result))
   .catch((err) => console.log(err));
```

## 결론

Promise.all과 Promise.allSettled를 구분하여 잘 사용하자.
Promise.all은 별개의 작업들이 하나의 작업으로 묶여서 전체가 성공해야 다 성공이고, 하나라도 실패하면 실패인 경우이고, Promise.allSettled은 하나가 실패하더라도 나머지의 값들은 받아야하는 경우, 또한 Promise.all을 사용했을 때 비효율적인 구조라면 사용하는게 좋다.

## 참고 자료

- https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%8D%94%EC%9D%B4%EC%83%81-Promiseall-%EC%93%B0%EC%A7%80%EB%A7%90%EA%B3%A0-PromiseallSettled-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90
