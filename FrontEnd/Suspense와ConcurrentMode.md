# React의 Suspense와 Concurrent Mode의 개념을 설명하고, 이를 사용하여 애플리케이션 성능을 최적화할 수 있는 방법을 설명해 주세요.

### Suspense란?

Suspense는 React 컴포넌트가 비동기적으로 데이터를 로드할 수 있도록 도와주는 기능입니다.

예를 들어, API 호출이나 코드 분할 등에서 데이터가 로드되는 동안 로딩 상태를 관리할 수 있습니다. Suspense를 사용하면 다음과 같은 장점이 있습니다.

1. 로딩 상태 관리: 데이터가 준비되지 않았을 때 로딩 스피너나 대체 UI를 표시할 수 있습니다.
2. 코드 분할: React.lazy와 함께 사용하여 코드 분할을 쉽게 구현할 수 있습니다. 필요한 순간에만 컴포넌트를 로드하여 초기 로딩 시간을 줄입니다.

```javascript
const LazyComponent = React.lazy(() => import("./LazyComponent"));

<Suspense fallback={<div>로딩 중...</div>}>
  <LazyComponent />
</Suspense>;
```

### Concurrent Mode란?

리액트가 돌아가는 환경인 자바스크립트는 싱글 스레드로 동작하기 때문에 동기적으로 동작하는 어떤 작업이 오래걸린다면 그 동안 아무것도 할 수 없다.

그것은 화면을 보여주는 렌더링도 마찬가지이다.

리액트 16버전 이전에는 이런 렌더링 하는 과정이 동기적으로 이루어졌기 때문에 렌더링이 끝날때까지 사용자의 입력도 에니메이션도 처리하지 못하는 경우가 생기게 되었다.

리액트팀은 이를 해결하기 위해서 16버전에서 렌더링 엔진을 변경하게 되었고 스케쥴러를 통해 우선순위에 따라 급한작업이 있다면 먼저 처리할 수 있도록 해결하였다.

`동시성 모드는 이러한 리액트팀의 노력으로 만들어진 동시성을 지원하는 환경을 의미`한다.

리액트 18 버전이 업데이트 되면서 이전 버전에 대한 호환성을 유지하고 점진적이고 안전한 업그레이드를 지원하기 위해서 기존 ReactDOM.render를 통한 lagacy mode와 createRoot를 통한 concurrent mode를 선택할 수 있게 하였다.

### lagacy mode

```javascript
// index.js

import ReactDOM from "react-dom";
import App from "./App";

const container = document.getElementById("app");

ReactDOM.render(<App />, container);
```

### concurrent mode

```javascript
// index.js
import ReactDOM from "react-dom";
import App from "./App";

const container = document.getElementById("app");

const root = ReactDOM.createRoot(container); // createRoot를 사용한다.

root.render(<App />);
```

`기본적으로 CRA(create-react-app) 으로 생성하게 되면 createRoot를 사용하게된다.`

### 동시성 모드에서는 어떤걸 지원할까?

동시성 모드를 사용하면 리액트 18버전에서 동시성 렌더링을 제공하는 새로운 기능을 사용할 수 있다.
대표적으로는 `useTransition`과 `useDeferredValue` 훅이 있다.

### Concurrent Mode의 특징

1. 중단 가능한 렌더링 (Interruptible Rendering):

- 긴 렌더링 작업을 중단하고, 더 중요한 작업(예: 사용자 입력)을 먼저 처리할 수 있습니다. 이로 인해 UI가 항상 반응할 수 있게 됩니다.

2. 우선 순위 조정 (Prioritized Updates):

- React는 여러 업데이트의 우선 순위를 설정하여, 사용자 인터랙션과 같은 중요한 작업을 먼저 처리합니다. 덜 중요한 업데이트는 나중에 수행될 수 있습니다.

3. 비동기 렌더링:

- 렌더링 작업을 비동기로 처리하여, UI가 차단되지 않고 사용자와의 상호작용이 계속 가능하게 합니다.

4. Suspense와의 통합:

- Concurrent Mode는 Suspense와 통합되어, 비동기 데이터 로딩을 관리하는 데 도움을 줍니다.
- 이를 통해 로딩 상태를 쉽게 관리하고, 사용자에게 더 나은 경험을 제공합니다.

### Concurrent Mode가 해결하려는 문제

1. UI 반응성 저하:

- 사용자 입력에 대한 즉각적인 반응을 보장하여, 사용자가 삽입한 데이터나 클릭한 버튼에 대해 즉시 반응할 수 있도록 합니다.

2. 긴 렌더링 작업으로 인한 차단:

- 긴 렌더링 작업이 UI를 차단하지 않도록 하여, 사용자가 앱을 사용하는 동안 원활한 경험을 유지할 수 있습니다.

3. 데이터 로딩 및 비동기 작업 관리:

- 데이터를 비동기적으로 로드할 때, 로딩 상태를 적절히 관리하여 사용자에게 피드백을 제공하고, UI가 멈추지 않도록 합니다.

### 동시성 이슈?

리액트 18에서는 useTransition이나 useDeferredValue의 훅처럼 `렌더링을 일시 중지하거나 뒤로 미루는 등의 최적화가 가능해지면서 동시성 이슈가 발생할 수 있게 되었습니다.`

과거 리액트에서는 중간에 데이터 업데이트가 일어나는 것과 상관없이 동기적으로 렌더링이 한번에 발생해서 이러한 문제가 없었습니다.

하지만 리액트 18에서부터는 리액트가 렌더링을 앞선 훅처럼 중지했다가 다시 실행하는 등 '양보'하는 것이 가능해졌기 때문에 이러한 문제가 발생할 가능성이 존재하는 것입니다.

물론 리액트에서 관리하는 state라면 useTransition이나 useDeferredValue와 같이 내부적으로 이러한 문제를 해결하기 위한 처리를 했지만

리액트에서 관리할 수 없는 `외부 데이터 소스`에서라면 문제가 달라집니다.

여기서 말하는 외부 데이터 소스란 리액트의 클로저 범위 밖에 있는, 관리 밖에 있는 값들을 말합니다.

1. 글로벌 변수
2. document.body
3. window.innerWidth
4. DOM
5. 리액트 외부에 상태를 저장하는 외부 상태 관리 라이브러리

이 `외부 데이터 소스에 리액트가 추구하는 동시성 처리가 되어있지 않다면 테어링 현상이 발생`할 수 있습니다.

그리고 이 문제를 해결하기 위한 훅이 바로 `useSyncExternalStore`입니다.
