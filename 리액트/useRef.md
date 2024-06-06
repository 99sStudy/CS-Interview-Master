## ✨useRef에 대해서 설명해주세요

useRef 는 .current 프로퍼티에 전달된 인자(initialValue)로 초기화된 변경 가능한 ref 객체를 반환합니다.

반환된 객체는 컴포넌트의 전 생애주기 동안 유지됩니다.

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
