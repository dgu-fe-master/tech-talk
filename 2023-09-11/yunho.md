# package.json - dependencies, devDependencies, peerDependencies

## 배경

최근 오픈 소스 라이브러리를 구경하고 있는데요, package에 잘 알지 못했던 속성이 많더라구요. 여러 속성이 많은데 모두 자세히 알 필요는 없는 것 같고, 핵심 속성인 dependencies에 대해서 다시 찾아봤어요.

## 요약

### dependencies

- 앱에 종속된 가장 일반적인 종속성이예요.
- **런타임과 빌드타임, 개발중** 모두에서 이 dependencies 패키지들이 필요하기 때문에, `앱이 빌드 될 때 dependencies 패키지들이 번들에 포함되어 배포돼요!`

### devDependencies

- `런타임에서는 필요하지 않고 빌드타임 & 개발중에만 필요한 패키지`이예요.
- 빌드타임에서 devDependencies 패키지들은 빌드에 도움을 주거나 참조가 되지만, **번들에는 포함되지 않는** 종속성 패키지들입니다.

=> devDependencies에 포함된 라이브러리는 실제 배포할 때 포함되지 않기 때문에 빌드 시간을 줄일 수 있어요. 앞으로 개발하면서 애플리케이션에 필요한 라이브러리가 아니라면, devDependencies에 설치하도록 하죠!

### peerDependencies

- 실제로 패키지에서 **`require`**나 **`import`** 하지는 않지만, 특정 라이브러리나 툴에 호환성을 필요로 할 경우에 명시하는 dependencies이예요.

예시로 보면 좋을 것 같은데, [yceffort: package.json에 쌓여있는 개발 부채 - 2. peerDependencies 도 자세히 확인해보자](https://yceffort.kr/2021/10/debt-of-package-json#2-peerdependencies-도-자세히-확인해보자)여기서 가져온 예제를 보면,

```json
{
  "name": "playground",
  "dependencies": {
    "react": "16.8.6"
  },
  "devDependencies": {
    "@testing-library/react-hooks": "^7.0.2"
  }
}
```

위의 packjson에서 react 버전은 16.8.6으로 고정되어 있고, @testing-library/react-hooks를 devDependencies로 가지려고 해요. 그런데 @testing-library/react-hooks는 peerDependencies로 react@>=16.9를 요구하기 때문에 아래와 같이 npm@7 환경에서는 설치가 되지 않습니다..!

(참고: npm3 부터 6까지는 peerDependencies가 자동으로 설치되지 않았고, 만약 버전이 맞지 않는 경우 에러가 아닌 warning(경고 문구)만 보였다고 해요. npm@7 부터는 peerDependencies가 기본으로 설치되고, 이 버전이 맞지 않으면 에러가 발생한다고 하네요.)

```json
{
  "name": "@testing-library/react-hooks",
  "peerDependencies": {
    "react": ">=16.9.0",
    "react-dom": ">=16.9.0",
    "react-test-renderer": ">=16.9.0"
  }
}
// https://github.com/testing-library/react-hooks-testing-library/blob/565c9f80ff969c3b9f20d8b2efdc033996d9ec27/package.json#L78
```

### peerDependencies가 왜 필요할까요?

이 부분이 핵심인데, 아직까지 잘 모르겠어요😓

peerDependencies를 정확히 정의되지 않은 라이브러리를 사용할 때, 어떤 문제가 생기는지 `node_modules`랑 `종속성` 관련해서 학습이 더 필요해요.

## 참고 자료

- https://yceffort.kr/2021/10/debt-of-package-json
