## ✨useMemo에 대해서 설명해주세요.

- useMemo는 `비용이 큰 연산에 대한 결과를 저장해 두고, 이 저장된 값을 반환하는 훅`입니다.

- 첫 번째 인수로는 어떠한 값을 반환하는 생성 함수를, 두 번째 인수로는 해당 함수가 의존하는 값의 배열을 전달합니다.

- useMemo는 렌더링 발생 시 의존성 배열의 값이 변경되지 않았으면 함수를 재실행하지않고, 이전에 기억해 둔 해당 값을 반환하고, 의존성 배열의 값이 변경됐다면 첫 번째 인수의 함수를 실행항 후의 값을 반환하고 그 값을 다시 기억해둡니다.

- 단순히 값 뿐만 아니라 컴포넌트도 가능합니다.

- 물론 React.memo를 쓰는 것이 더 현명합니다.

### 🛠️사용법

```
import { useMemo } from 'react';

function TodoList({ todos, tab, theme }) {
  const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
  // ...
}

// 대부분의 계산은 매우 빠르기 때문에 이것은 문제가 되지 않습니다.  그러나 큰 배열을 필터링하거나, 변환하거나, 고비용의 계산을 수행할 때, 데이터가 변경되지 않았다면 다시 계산하는 것을 건너뛰고 싶을 수 있습니다. todos와 tab이 이전 렌더링 때와 동일하다면, 이전처럼 계산을 useMemo로 감싸서 이전에 이미 계산해놓은 visibleTodos를 재사용할 수 있습니다.
```

[코드 출처](https://react-ko.dev/reference/react/useLayoutEffect)
