React의 `useReducer`와 `Context API`를 결합하여 전역 상태 관리를 구현하는 방법은 비교적 간단하지만 강력한 접근 방식입니다. 이 조합은 상태가 복잡한 애플리케이션에서 상태 관리 라이브러리(예: Redux) 없이도 효과적으로 동작합니다. 아래에 주요 내용을 단계별로 설명하고 Redux와의 비교도 덧붙입니다.

---

### 1️⃣ `useReducer`와 `Context API`의 역할

- **`useReducer`**: 복잡한 상태 로직을 관리하기 위한 React 훅으로, 액션(action)과 리듀서(reducer)를 사용해 상태 업데이트를 처리합니다.
- **`Context API`**: React 컴포넌트 트리 전체에 데이터를 공급하고 관리할 수 있는 메커니즘으로, `useContext` 훅과 함께 사용하여 하위 컴포넌트에서 데이터를 쉽게 접근할 수 있습니다.

이 둘을 결합하면 다음과 같은 이점이 있습니다:

1. 상태 관리의 로직을 단일 장소(`reducer`)에 집중화.
2. `Context API`를 통해 하위 트리 전체에 상태와 디스패치(dispatch) 함수를 전달.

---

### 2️⃣ 구현 단계

#### Step 1. 초기 상태와 리듀서 함수 정의

```javascript
const initialState = {
  count: 0,
  user: null,
};

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + 1 };
    case "SET_USER":
      return { ...state, user: action.payload };
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
}
```

#### Step 2. Context 생성

```javascript
import { createContext, useReducer, useContext } from "react";

const StateContext = createContext();
const DispatchContext = createContext();
```

#### Step 3. Provider 컴포넌트 작성

```javascript
export function AppProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <StateContext.Provider value={state}>
      <DispatchContext.Provider value={dispatch}>
        {children}
      </DispatchContext.Provider>
    </StateContext.Provider>
  );
}
```

#### Step 4. `useContext`를 사용해 데이터 접근

```javascript
export function useAppState() {
  const context = useContext(StateContext);
  if (!context) {
    throw new Error("useAppState must be used within an AppProvider");
  }
  return context;
}

export function useAppDispatch() {
  const context = useContext(DispatchContext);
  if (!context) {
    throw new Error("useAppDispatch must be used within an AppProvider");
  }
  return context;
}
```

#### Step 5. 컴포넌트에서 활용

```javascript
import { useAppState, useAppDispatch } from "./AppContext";

function Counter() {
  const state = useAppState();
  const dispatch = useAppDispatch();

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Increment</button>
    </div>
  );
}
```

#### Step 6. `AppProvider`로 전체 앱 감싸기

```javascript
import { AppProvider } from "./AppContext";

function App() {
  return (
    <AppProvider>
      <Counter />
    </AppProvider>
  );
}
```

---

### 3️⃣ Redux와의 비교

| **특징**        | **useReducer + Context API**                              | **Redux**                                  |
| --------------- | --------------------------------------------------------- | ------------------------------------------ |
| **복잡도**      | 간단하고 설정이 적음                                      | 설정 및 boilerplate 코드가 많음            |
| **사용성**      | 소규모 애플리케이션에 적합                                | 대규모 애플리케이션에서 적합               |
| **성능**        | Context 값이 변경될 때 모든 하위 컴포넌트가 리렌더링 가능 | 효율적인 상태 관리 및 선택적 리렌더링 지원 |
| **확장성**      | 확장이 제한적                                             | 미들웨어 및 DevTools를 통한 확장성         |
| **데이터 흐름** | 단일 Context로 제한될 수 있음                             | 상태의 명확한 흐름 및 구조화된 관리 가능   |

#### 사용 시 고려 사항

- `useReducer + Context API`는 작은 규모의 애플리케이션이나 상태가 단순한 경우 적합합니다.
- Redux는 복잡한 상태 관리가 필요하고 여러 미들웨어와의 통합이 중요한 대규모 애플리케이션에 적합합니다.

---

### 4️⃣ 성능 최적화 팁

1. **Context 분리**: 상태와 디스패치를 별도의 Context로 분리해 불필요한 리렌더링을 줄일 수 있습니다.
2. **메모이제이션**: `useMemo` 또는 `React.memo`를 사용해 컴포넌트 리렌더링을 최적화합니다.

---
