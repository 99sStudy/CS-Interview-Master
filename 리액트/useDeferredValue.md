# ✨useDeferredValue란 무엇인가요?

**useDeferredValue**는 React에서 제공하는 Hook으로, UI 업데이트 우선순위를 관리하는 데 유용합니다.

이 Hook은 특정 상태값이 변경될 때 **지연된 값(deferred value)**을 제공하여, 성능 문제를 완화하거나 사용자 경험을 개선합니다.

특히, `검색과 같은 고비용 연산을 포함한 작업에서 유용`합니다.

주요 목적: 사용자 입력과 UI 업데이트 간의 지연을 최소화하면서도, 고비용 작업을 백그라운드에서 처리하게 만듭니다.

예를 들어 a를 검색하고 ab를 검색하는 상황을 가정하고,

Suspense를 감싼 경우 a를 검색한 이후 ab를 검색하면 `로딩 fallback으로 돌아간 이후 ab의 결과`로 나타납니다.

하지만 `useDeferredValue`를 사용하면 a결과 이후 바로 ab의 결과를 나타냅니다.

```js
import { useState, useDeferredValue } from "react";

function SearchComponent() {
  const [searchQuery, setSearchQuery] = useState("");
  const deferredQuery = useDeferredValue(searchQuery);

  return (
    <div>
      <input
        value={searchQuery}
        onChange={(e) => setSearchQuery(e.target.value)}
        placeholder="Search..."
      />
      {/* 고비용 작업에 지연된 값을 사용 */}
      <SearchResults query={query} />
    </div>
  );
}

function SearchResults({ query }) {
  const results = expensiveSearchFunction(query); // 고비용 작업
  return (
    <ul>
      {results.map((result, index) => (
        <li key={index}>{result}</li>
      ))}
    </ul>
  );
}
```

1. useDeferredValue는 지정된 값(예: searchQuery)이 바뀔 때 `바로 업데이트되지 않고 지연된 값을 반환`합니다.
2. React는 `입력값 업데이트(사용자 입력)와 같은 높은 우선순위 작업을 먼저 처리`합니다.

### ⚡장점

1. 성능 최적화
   사용자 입력처럼 빠른 응답이 필요한 작업과 검색 결과 표시처럼 고비용 작업을 분리합니다.
   React의 Concurrent Mode와 함께 동작하여, 화면이 렉 없이 부드럽게 업데이트됩니다.
2. 사용자 경험 개선
   사용자가 입력을 빠르게 진행하더라도, UI가 즉시 반응하므로 더 직관적인 UX를 제공합니다.
   검색 결과 업데이트가 약간 느려도 사용자는 입력 인터페이스에만 집중할 수 있습니다.

### ⚡단점

1. 추가 복잡성
   - 간단한 컴포넌트에서는 불필요하게 코드가 복잡해질 수 있습니다.
   - 초보 개발자에게는 React의 비동기 업데이트 모델을 이해하기 어렵게 만들 수 있습니다.
2. 제어 어려움
   - 지연되는 값이 정확히 언제 업데이트될지 명시적이지 않아, 특정 상황에서 예상치 못한 동작을 초래할 수 있습니다.3. 그 후, UI가 덜 민감한 작업(예: 검색 결과 표시)을 처리할 때 지연된 값(deferredQuery)을 사용해 업데이트합니다.

### ⚡언제 사용해야 할까?

useDeferredValue는 `실시간 검색` 또는 `필터링`처럼 `입력과 결과 계산의 속도 차이가 큰 경우에 적합`합니다.

- 사용자가 검색창에 입력할 때 입력 속도와 상관없이 검색 결과가 부드럽게 로드되어야 할 때
- 고비용 데이터 연산(예: API 요청, 대규모 필터링)과 UI 업데이트를 분리하고 싶을 때

## ✨useTransition과 useDeferredValue의 차이점은?

### 1. 개념 차이

### useTransition

- "작업의 우선순위를 낮춘다."
- 특정 상태 업데이트를 비동기적이고 우선순위가 낮은 작업으로 처리하도록 지정합니다.
- 적용 범위: 컴포넌트 렌더링 자체의 우선순위를 조절하고 싶을 때.

### useDeferredValue

- "값의 업데이트를 지연시킨다."
- 전달받은 값의 업데이트를 지연시켜서, UI가 덜 민감한 작업(예: 고비용 계산)에 지연된 값을 사용하도록 만듭니다.
- 적용 범위: 값(state)의 우선순위를 낮춰야 할 때.

## ✨**1. 개념 차이**

### **`useTransition`**

- **"작업의 우선순위를 낮춘다."**
- 특정 상태 업데이트를 **비동기적이고 우선순위가 낮은 작업**으로 처리하도록 지정합니다.
- **적용 범위:** 컴포넌트 렌더링 자체의 우선순위를 조절하고 싶을 때.

### **`useDeferredValue`**

- **"값의 업데이트를 지연시킨다."**
- 전달받은 값의 업데이트를 지연시켜서, **UI가 덜 민감한 작업**(예: 고비용 계산)에 지연된 값을 사용하도록 만듭니다.
- **적용 범위:** 값(state)의 우선순위를 낮춰야 할 때.

---

## ✨**2. 사용 예시**

### **`useTransition`**

사용자가 버튼을 눌러 새로운 데이터를 로드하는 상황을 가정합니다. 렌더링 비용이 크다면, `useTransition`을 사용해 UI가 부드럽게 업데이트되도록 처리할 수 있습니다.

- **`startTransition`:** 상태 업데이트를 비동기적이고 우선순위가 낮은 작업으로 처리.
- **`isPending`:** 비동기 작업이 진행 중인지 여부를 나타내는 Boolean 값.

### **`useDeferredValue`**

사용자가 검색어를 입력하고, 검색 결과를 필터링하는 상황을 가정합니다. `useDeferredValue`를 사용해 입력 지연 없이 UI가 부드럽게 반응하도록 구현합니다.

## ✨**3. 주요 차이점**

| **특징**           | **`useTransition`**              | **`useDeferredValue`**                       |
| ------------------ | -------------------------------- | -------------------------------------------- |
| **적용 대상**      | 상태 업데이트 전체               | 특정 값(state)                               |
| **작동 방식**      | 상태 업데이트를 비동기 처리      | 값의 업데이트를 지연 처리                    |
| **사용 목적**      | UI 렌더링 자체의 우선순위 낮추기 | 값 업데이트와 고비용 작업의 분리             |
| **주요 상황**      | 데이터 로딩, 화면 전환           | 실시간 검색, 대규모 데이터 필터링            |
| **React API 연계** | Concurrent Mode 필수             | Concurrent Mode 필요 없음 (있으면 더 효과적) |
| **반환 값**        | `isPending`, `startTransition`   | 지연된 값 (deferred value)                   |

---

### 결론

- **useTransition**은 UI 렌더링 우선순위를 조정하는 데 유용합니다.
- **useDeferredValue**는 특정 값 업데이트를 지연하여 고비용 작업을 최적화합니다.
