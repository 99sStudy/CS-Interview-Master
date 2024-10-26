## ✨useReducer에 대해서 설명해주세요.

**useReducer는 복잡한 컴포넌트 상태 로직을 관리할 때 useState보다 더 선호되는 방법입니다.**

state값을 변경하는 시나리오를 제한적으로두고, 이에 대한 변경을 빠르게 확인할 수 있게끔 하는 것이 목적입니다.

`상태(state)를 복잡한 형태로 관리`하면서, 그 상태를 수정할 수 있는 방법을 `미리 정의한 함수(dispatcher)를 통해서만 가능하게` 합니다.

상태에 대한 접근은 컴포넌트 내에서만 허용하고, 상태를 업데이트하는 구체적인 방법은 컴포넌트 밖에서 정의합니다.

이렇게 하면 상태 업데이트는 오직 정의된 함수를 통해서만 이루어집니다.

### 🛠️사용법

```javascript
import React, { useReducer } from "react";

// 초기 상태
const initialState = { count: 0 };

// 리듀서 함수
function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    case "reset":
      return initialState;
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>현재 카운트: {state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>증가</button>
      <button onClick={() => dispatch({ type: "decrement" })}>감소</button>
      <button onClick={() => dispatch({ type: "reset" })}>리셋</button>
    </div>
  );
}

export default Counter;
```
