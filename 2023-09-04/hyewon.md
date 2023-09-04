# 자바스크립트 Object를 해시 맵처럼 이용하려면 Map을 사용하자

## 배경

커리어리에 올라온 포스트들 가운데 관심이 갔던 주제에 대해 다루어보았어요. 그동안 JS로 많은 프로젝트를 진행해보았지만 한 번도 Map을 사용해본 적이 없는 것 같아 Object와의 비교를 통해 두 가지 가운데 근거 있게 선택할 수 있도록 학습해보려고 해요.

## 요약

### Map을 언제 사용해야 할까?

모던 자바스크립트 Deep Dive의 정의를 인용하면,
*자바스크립트 Object는 프로퍼티와 메서드로 구성된 집합체로, **이처럼 객체는 상태와 동작을 하나의 단위로 구조화할 수 있어 유용**하다*로 정의되어요.

Object는 위 설명대로 상태와 동작을 하나로 저장할 수 있는 데이터 타입이에요. 다음 코드는 여러 상태를 갖고 있지만, 상태들이 하나의 단위로 이용되지는 않아 보여요. 이러한 경우 Object 대신 `Map`을 이용해볼 수 있어요.

```tsx
const ResultIconByStatus: Record<Status, ResultIconInfo> = {
    info: { name: 'info-rounded', color: '#005FFC' },
    success: { name: 'check-circle-rounded', color: '#00C021' },
    warning: { name: 'warning', color: '#FFA825' },
    error: { name: 'close-circle', color: '#FF464A' },
};
```

### Map을 사용해야 하는 이유

#### 1. 더 다양한 key types를 제공한다.

Object는 key로 Symbol과 String 타입만 이용할 수 있으나, Map은 key로 어떤 타입이든 사용 가능해요.

```tsx
const map = new Map();
const myFunction = () => console.log('I am a useful function.');
const myNumber = 666;
const myObject = {
    name: 'plainObjectValue',
    otherKey: 'otherValue',
};
map.set(myFunction, 'function as a key');
map.set(myNumber, 'number as a key');
map.set(myObject, 'object as a key');

console.log(map.get(myFunction)); // function as a key
console.log(map.get(myNumber)); // number as a key
console.log(map.get(myObject)); // object as a key
```

#### 2. 크기를 빠르게 구할 수 있다.

Map은 크기를 O(1)의 시간 복잡도로 구할 수 있으나 Object는 O(n)의 시간이 소요돼요. 또한 크기를 구하는 방식도 간단해요.

```tsx
const map = new Map();
map.set('someKey1', 1);
map.set('someKey2', 1);
...
map.set('someKey100', 1);

console.log(map.size) // 100, Runtime: O(1)

const plainObjMap = {};
plainObjMap['someKey1'] = 1;
plainObjMap['someKey2'] = 1;
...
plainObjMap['someKey100'] = 1;

console.log(Object.keys(plainObjMap).length) // 100, Runtime: O(n)
```

#### 3. 더 나은 성능

Map은 설계단계부터 데이터의 추가와 제거에 최적화 되어 있기 때문에 성능에 있어서 매우 유리해요. Object는 key-value 쌍의 빈번한 추가 및 제거에 최적화되어 있지 않아요.
원문에 따르면 맥북프로에서 천만개의 데이터 셋을 가지고 테스트 했을 때 Object는 1.6초의 처리시간이 필요했고 Map은 1ms 이하의 처리시간을 보였다고 해요.

#### 4. 반복문 사용

Object는 key들을 먼저 찾아낸 다음 그것들을 토대로 순회하지만, Map은 그 자체가 iterable 하기 때문에 직접 반복할 수 있어요.

```tsx
const map = new Map();
map.set('someKey1', 1);
map.set('someKey2', 2);
map.set('someKey3', 3);

for (let [key, value] of map) {
    console.log(`${key} = ${value}`);
}
// someKey1 = 1
// someKey2 = 2
// someKey3 = 3

const plainObjMap = {};
plainObjMap['someKey1'] = 1;
plainObjMap['someKey2'] = 2;
plainObjMap['someKey3'] = 3;

for (let key of Object.keys(plainObjMap)) {
    const value = plainObjMap[key];
    console.log(`${key} = ${value}`);
}
// someKey1 = 1
// someKey2 = 2
// someKey3 = 3
```

#### 5. 순서의 보장

Map은 추가한 순서에 따른 순번을 보장해요. ECMAScript 2015 이전 버전에서는 Object에서 키의 순서를 보장하지 않았지만 현재는 지원되어요.

#### 6. Key overriding 금지

Object에는 미리 정의된 prototype이 있기 때문에 key가 충돌할 위험이 있어요(toString, constructor, valueOf 등). 하지만 Map을 사용할 때는 그런 걱정을 할 필요가 없어요. Map은 기본적으로 키를 포함하지 않고, 명시적으로 입력된 내용만 포함되어요.

```tsx
const map = new Map();
map.set('키1', 1);
map.set('키2', 2);
map.set('toString', 3); // Map에서는 문제 없습니다.

const plainObjMap = {};
plainObjMap['키1'] = 1;
plainObjMap['키2'] = 2;
plainObjMap['toString'] = 3; // toString은 이미 선점되어 있습니다. toString()을 사용할 수 없게됩니다.
```

## 결론

## 참고 자료

-   https://betterprogramming.pub/stop-using-objects-as-hash-maps-in-javascript-9a272e85f6a8
-   https://shanepark.tistory.com/m/220
-   https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map
