# ✨React에서 상태(state) 관리 중 ‘lifting state up’과 ‘prop drilling’을 설명하고, 복잡한 컴포넌트 계층 구조에서 이로 인해 발생할 수 있는 문제들을 설명하세요.

### Lifting State Up

- **Lifting State Up**은 React에서 자식 컴포넌트 간의 상태를 공유하기 위해 상태를 공통의 상위 컴포넌트로 끌어올리는 패턴입니다.

- 이 방법은 여러 자식 컴포넌트가 동일한 상태를 필요로 할 때 유용합니다.

- 상위 컴포넌트에서 상태를 관리하고, `해당 상태와 관련된 업데이트 함수를 자식 컴포넌트에 props로 전달`하여 자식들이 상태를 공유할 수 있도록 합니다.

**예시**:

```jsx
function ParentComponent() {
  const [value, setValue] = useState(0);

  return (
    <>
      <ChildComponentA value={value} />
      <ChildComponentB setValue={setValue} />
    </>
  );
}
```

### Prop Drilling

**Prop Drilling**은 컴포넌트 계층 구조에서 상태나 데이터를 하위 컴포넌트로 전달할 때, 중간 컴포넌트를 통해 props를 계속 전달해야 하는 상황을 설명합니다.

- 이 경우, 중간 컴포넌트는 필요한 데이터를 직접 사용하지 않더라도 props를 받아야 하므로 코드가 복잡해지고 관리하기 어려워질 수 있습니다.

**예시**:

```jsx
function GrandparentComponent() {
  const [value, setValue] = useState(0);
  return <ParentComponent value={value} setValue={setValue} />;
}

function ParentComponent({ value, setValue }) {
  return <ChildComponent value={value} setValue={setValue} />;
}

function ChildComponent({ value, setValue }) {
  // value와 setValue를 사용
}
```

### 복잡한 컴포넌트 계층 구조에서의 문제

1. **코드 가독성 저하**:

   - prop drilling이 발생하는 경우, 중간 컴포넌트가 필요한 데이터를 직접 사용하지 않더라도 props를 전달해야 하므로, 코드가 복잡해지고 가독성이 떨어질 수 있습니다.
   - 이는 특히 컴포넌트 계층이 깊어질수록 더욱 두드러집니다.

2. **유지보수 어려움**:

   - 상태를 끌어올리거나 props를 전달하는 과정에서 변경이 필요할 경우, 여러 컴포넌트를 수정해야 할 수 있습니다. 이는 코드 유지보수를 어렵게 만듭니다.

3. **성능 저하**:

   - prop drilling이 심한 경우, 불필요한 렌더링이 발생할 수 있습니다. 예를 들어, 상위 컴포넌트의 상태가 변경될 때, 모든 하위 컴포넌트가 다시 렌더링될 수 있습니다.
   - React의 메모이제이션 기능을 사용하지 않으면, 성능에 부정적인 영향을 줄 수 있습니다.

4. **상태 관리의 복잡성**:
   - 상태를 공통으로 공유하기 위해 lifting state up을 사용할 때, 상태 관리가 복잡해질 수 있습니다. 여러 자식 컴포넌트에서 동일한 상태를 업데이트할 경우, 어떻게 업데이트할지를 명확히 해야 하며, 이로 인해 코드가 더욱 복잡해질 수 있습니다.

### 해결 방법

1. **Context API 사용**:

   - React의 Context API를 사용하여 전역 상태를 관리함으로써 prop drilling을 피할 수 있습니다. 이를 통해 컴포넌트 간 상태를 효율적으로 공유할 수 있습니다.

2. **상태 관리 라이브러리**:

   - Redux, MobX 등의 상태 관리 라이브러리를 사용하여 전역 상태를 관리하고, 복잡한 상태 관리를 단순화할 수 있습니다.

3. **컴포넌트 구조 재설계**:
   - 컴포넌트 구조를 재설계하여 상태를 필요한 컴포넌트에만 전달할 수 있도록 합니다. 이로 인해 prop drilling을 줄이고, 코드의 가독성을 높일 수 있습니다.
