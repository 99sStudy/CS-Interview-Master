## ✨useRef에 대해서 설명해주세요

useRef 는 .current 프로퍼티에 전달된 인자(initialValue)로 초기화된 변경 가능한 ref 객체를 반환합니다.

반환된 객체는 컴포넌트의 전 생애주기 동안 유지됩니다.

```
useRef는 저장공간 또는 DOM요소에 접근하기 위해 사용되는 React Hook이다.

useRef는 크게 2가지의 역할을 수행합니다.
먼저, 저장공간의 역할인데, state와 비슷한 역할을 하지만 state와의 차이점은 state는 변화가 일어나면 리-렌더링이 일어나고 내부 변수들은 초기화되는 반면에, ref에 저장한 값은 렌더링이 일어나지 않습니다.
즉, ref의 값 변화가 일어나도 렌더링으로 인해 내부변수들이 초기화되는 것을 막을 수 있습니다.

두 번째로 돔 요소에 접근할 수 있는 역할입니다.
바닐라js에서 특정 돔을 선택하기 위해 getElementById 또는 querySelector를 이용했는데 리액트에서도 특정 돔을 선택해야 하는 경우가 있습니다.
예를 들어 화면이 렌더링 된 직후 특정 input 태그가 포커싱이 되어야 하는 경우 useRef를 사용합니다.
```

## 🔁꼬리질문

### 🤔useRef를 사용하지 않고, 그냥 함수 외부에 값을 선언해서 관리하면 되지 않나요? 왜 안될까요?

함수 외부에 값을 선언해서 관리하게 되면 몇가지 문제점이 있습니다.

- 먼저 컴포넌트가 실행되어 렌더링 되지 않았음에도 외부값이 기본 적으로 존재하게 됩니다.
  이는 메모리에 불필요한 값을 갖게 하는 악영향을 미칩니다.

- 그리고 컴포넌트가 여러개 생성된다해도 각 컴포넌트가 가리키는 값이 모두 동일하게 됩니다.
  하나의 값을 봐야하는 경우라면 유효하지만 대부분의 경우 컴포넌트 인스턴스 하나당 하나의 값을 필요로 하는 것이 일반적입니다.

### 🛠️사용법

```
import { useRef } from 'react';

function MyComponent() {
  const intervalRef = useRef(0);
  const inputRef = useRef(null);
  // ...
```

[코드 출처](https://react-ko.dev/reference/react/useLayoutEffect)

### forwardRef란?

forwardRef는 React에서 제공하는 고차 컴포넌트(Higher-Order Component, HOC)로, 부모 컴포넌트가 자식 컴포넌트의 DOM 요소나 클래스 인스턴스에 직접 접근할 수 있도록 합니다.
주로 useRef와 함께 사용되어 컴포넌트 간에 참조를 전달하는 데 유용합니다.
