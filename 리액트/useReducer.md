## ✨useReducer에 대해서 설명해주세요.

** useReducer는 복잡한 컴포넌트 상태 로직을 관리할 때 useState보다 더 선호되는 방법입니다. **

state값을 변경하는 시나리오를 제한적으로두고, 이에 대한 변경을 빠르게 확인할 수 있게끔 하는 것이 목적입니다.

상태(state)를 복잡한 형태로 관리하면서, 그 상태를 수정할 수 있는 방법을 미리 정의한 함수(dispatcher)를 통해서만 가능하게 합니다.

상태에 대한 접근은 컴포넌트 내에서만 허용하고, 상태를 업데이트하는 구체적인 방법은 컴포넌트 밖에서 정의합니다.

이렇게 하면 상태 업데이트는 오직 정의된 함수를 통해서만 이루어집니다.

### 🛠️사용법

```javascript
import { useReducer } from "react";

function reducer(state, action) {
  if (action.type === "incremented_age") {
    return {
      age: state.age + 1,
    };
  }
  throw Error("Unknown action.");
}

function MyComponent() {
  const [state, dispatch] = useReducer(reducer, { age: 42 });
  // ...
}
```

```javascript
// 화면에 표시되는 내용을 업데이트하려면 사용자가 수행한 작업을 나타내는 객체, 즉,액션을 사용하여 dispatch를 호출합니다:
function handleClick() {
  dispatch({ type: "incremented_age" });
}
```

[코드 출처](https://react-ko.dev/reference/react/useLayoutEffect)
