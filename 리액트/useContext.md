## ✨useContext에 대해서 설명해주세요

React의 Context API를 함수형 컴포넌트 내에서 사용할 수 있게 해주는 Hook입니다

Context API는 주로 애플리케이션 전역에서 사용되어야 하는 데이터를 관리하기 위해 사용되며, useContext Hook을 사용하면 컴포넌트 트리의 깊은 곳에 있는 컴포넌트들이 props 드릴링 없이도 이러한 데이터에 접근할 수 있습니다.

Props 드릴링이란, 특정 데이터를 상위 컴포넌트에서 하위 컴포넌트로 일일이 전달하는 과정을 말합니다.

이 과정은 관리해야 할 props가 많아질수록 복잡해지고 유지보수가 어려워집니다.

useContext를 사용하면 이러한 복잡성을 줄일 수 있습니다.

## 🔁꼬리질문

### 🤔useContext를 사용할 때 주의할 점에 대해서 아는대로 설명해주세요.

1. useContext를 함수형 컴포넌트 내부에서 사용할 떄는 항상 컴포넌트 재활용이 어려워진다는 점을 염두에 둬야합니다.

   Provider에 의존성을 가지고 있는 셈이 되므로 아무데서나 재활용하기에는 어려운 컴포넌트가 됩니다.

2. useContext는 상태관리를 위한 리액트의 API가 합니다.

   상태관리 라이브러리가 되기위해서는 최소한의 두가지 조건을 만족해야합니다.

첫 번째로 어떠한 상태를 기반으로 다른 상태를 만들어 낼수 있어야 합니다.

두 번째로 필요에 따라 이러한 상태 변화를 최적화할 수 있어야 합니다.

### 🤔그럼 모든 콘텍스트를 최상위 루트 컴포넌트에 넣으면 되지 않나요?

리액트 애플리케이션 관점에서는 그다지 현명한 접근법이 아닙니다. 콘텍스트가 많아질 수 록 루트 컴포넌트는 더 많은 콘텍스트로 둘러싸일 것이고, 해당 props를 다수의 컴포넌트에서 사용할 수 있게끔 해야하므로 불필요하게 리소스가 낭비됩니다.

Context의 범위는 필요한 환경에서 최대한 좁게 만들어야 합니다.

### 🤔그럼 Context API를 사용하면서 렌더링 최적화를 만들어 낼려면 어떻게 해야할까요?

React.memo를 사용하면 됩니다.

### 🛠️사용법

```
function MyPage() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  );
}

function Form() {
    const theme = useContext(ThemeContext);
}

```

```
// 시간이 지남에 따라 context가 변경되기를 원하는 경우가 종종 있습니다. context를 업데이트하려면 state와 결합해야 합니다. 부모 컴포넌트에 state 변수를 선언하고 현재 state를 context 값으로 provider에 전달합니다.
function MyPage() {
  const [theme, setTheme] = useState('dark');
  return (
    <ThemeContext.Provider value={theme}>
      <Form />
      <Button onClick={() => {
        setTheme('light');
      }}>
        Switch to light theme
      </Button>
    </ThemeContext.Provider>
  );
}

```

[코드 출처](https://react-ko.dev/reference/react/useLayoutEffect)
