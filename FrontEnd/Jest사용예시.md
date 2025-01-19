### **Jest란 무엇인가요?**

**Jest**는 Facebook에서 개발한 **JavaScript 테스트 프레임워크**로, 특히 **React** 애플리케이션을 테스트하는 데 널리 사용됩니다.  
단순하고 강력하며 다양한 기능을 제공하여 프론트엔드와 백엔드에서 모두 활용할 수 있습니다.

---

### **1. Jest의 주요 특징**

1. **간단한 설정**

   - Jest는 기본 설정이 간단하고 별도의 추가 구성 없이 바로 사용할 수 있습니다.

2. **스냅샷 테스트(Snapshot Testing)**

   - 컴포넌트 출력 결과를 저장하고, 이후 변경 사항을 감지하여 예상치 못한 UI 변화 방지.

3. **비동기 코드 테스트 지원**

   - 프로미스, `async/await` 등 비동기 로직을 쉽게 테스트 가능.

4. **모의(Mock) 함수 및 모듈**

   - 외부 의존성을 가짜로 구현하여 테스트 범위를 제한하고 독립성 보장.

5. **React와의 호환성**
   - React Testing Library와 함께 사용하면 컴포넌트 렌더링 및 이벤트 테스트에 적합.

---

### **2. Jest 기본 사용 예시**

#### **설치**

```bash
npm install --save-dev jest
```

#### **Jest 테스트 파일 작성**

파일 이름은 보통 `*.test.js` 또는 `*.spec.js`로 작성합니다.

#### **예제 1: 간단한 함수 테스트**

```javascript
// math.js
function add(a, b) {
  return a + b;
}

module.exports = add;
```

```javascript
// math.test.js
const add = require("./math");

test("add(2, 3)는 5를 반환해야 합니다.", () => {
  expect(add(2, 3)).toBe(5);
});
```

#### **실행**

```bash
npx jest
```

---

### **3. React에서 Jest 사용 예시**

React 컴포넌트와 로직을 테스트하려면 Jest와 함께 **React Testing Library**를 사용하는 것이 일반적입니다.

#### **설치**

```bash
npm install --save-dev jest @testing-library/react @testing-library/jest-dom
```

#### **React 컴포넌트 테스트**

##### **Counter 컴포넌트**

```javascript
// Counter.js
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p data-testid="count">Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

##### **Counter 테스트 코드**

```javascript
// Counter.test.js
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import "@testing-library/jest-dom"; // jest-dom의 매처 확장
import Counter from "./Counter";

test("Counter 컴포넌트 렌더링 테스트", () => {
  render(<Counter />);

  // 초기 카운트 값이 0인지 확인
  const countElement = screen.getByTestId("count");
  expect(countElement).toHaveTextContent("Count: 0");
});

test("Increment 버튼 클릭 시 카운트 증가", () => {
  render(<Counter />);

  // Increment 버튼 찾기
  const buttonElement = screen.getByText("Increment");

  // 클릭 이벤트 발생
  fireEvent.click(buttonElement);

  // 카운트가 1로 증가했는지 확인
  const countElement = screen.getByTestId("count");
  expect(countElement).toHaveTextContent("Count: 1");
});
```

#### **실행**

```bash
npx jest
```

---

### **4. Jest와 React Testing Library 통합의 장점**

1. **React 전용 매처**: `@testing-library/jest-dom`으로 `toHaveTextContent`, `toBeVisible` 등 매처 사용 가능.
2. **실제 DOM 시뮬레이션**: 컴포넌트를 렌더링하고 사용자 이벤트를 시뮬레이션.
3. **상태와 동작 테스트**: 컴포넌트 상태나 이벤트의 결과를 검증 가능.

---

### **5. 스냅샷 테스트 예시**

#### **스냅샷 테스트 코드**

```javascript
import React from "react";
import renderer from "react-test-renderer";
import Counter from "./Counter";

test("Counter 컴포넌트의 스냅샷 생성", () => {
  const tree = renderer.create(<Counter />).toJSON();
  expect(tree).toMatchSnapshot();
});
```

#### **실행 결과**

- 스냅샷 파일이 자동 생성되어 현재 컴포넌트 출력 결과를 저장.
- 이후 코드 변경 시 스냅샷과 비교하여 변화 감지.

---

### **6. Jest를 활용한 비동기 코드 테스트**

```javascript
// fetchData.js
export const fetchData = () =>
  new Promise((resolve) => setTimeout(() => resolve("Success!"), 1000));
```

```javascript
// fetchData.test.js
import { fetchData } from "./fetchData";

test("fetchData 비동기 테스트", async () => {
  const data = await fetchData();
  expect(data).toBe("Success!");
});
```

---

### **7. Jest의 Mock 기능 사용**

#### **모듈 Mock**

```javascript
// fetchData.js
export const fetchData = () => fetch("https://api.example.com/data");
```

```javascript
// fetchData.test.js
import { fetchData } from "./fetchData";

jest.mock("./fetchData", () => ({
  fetchData: jest.fn(() => Promise.resolve("Mocked Data")),
}));

test("Mocked fetchData 테스트", async () => {
  const data = await fetchData();
  expect(data).toBe("Mocked Data");
});
```

---

### **8. Jest 설정 추가**

#### **`package.json`에 Jest 설정**

```json
"scripts": {
  "test": "jest"
},
"jest": {
  "setupFilesAfterEnv": ["@testing-library/jest-dom"]
}
```
