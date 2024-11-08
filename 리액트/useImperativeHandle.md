## ✨useImperativeHandle에 대해서 설명해주세요.

`부모에게서 넘겨받은 ref를 원하는대로 수정할 수 있는 훅`입니다.

부모컴포넌트에서 내려준 ref의 값에 원하는 값이나 액션을 정의할 수 있습니다.

### 🛠️사용법

```js
import { forwardRef, useRef, useImperativeHandle } from "react";

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  // 예를 들어, 전체 <input> DOM 노드를 노출하지 않고 focus와 scrollIntoView의 두 메서드만을 노출하고 싶다고 가정해 봅시다.
  // 그러기 위해서는 실제 브라우저 DOM을 별도의 ref에 유지해야 합니다.
  // 그리고 useImperativeHandle을 사용하여 부모 컴포넌트에서 호출할 메서드만 있는 핸들을 노출합니다:
  useImperativeHandle(
    ref,
    () => {
      return {
        focus() {
          inputRef.current.focus();
        },
        scrollIntoView() {
          inputRef.current.scrollIntoView();
        },
      };
    },
    []
  );
  // 이제 부모 컴포넌트가 MyInput에 대한 ref를 가져오면 focus 및 scrollIntoView 메서드를 호출할 수 있습니다. 그러나 기본 <input> DOM 노드의 전체 액세스 권한은 없습니다.

  return <input {...props} ref={inputRef} />;
});
```

### 언제 사용해야 하는가

컴포넌트의 특정 기능을 외부에서 호출해야 할 때:

- 예를 들어, 자식 컴포넌트의 `포커스 메서드`나 `애니메이션 제어`와 같은 `특정 기능을 부모 컴포넌트에서 호출해야 할 때` 유용합니다.
- 폼 입력 또는 애니메이션 제어: 입력 필드의 포커스나 애니메이션 시작/중지와 같은 기능을 외부에서 간편하게 제어하고 싶을 때 사용합니다.

### 남용했을 때 문제점

1. React의 선언적 패러다임 훼손
2. 컴포넌트의 캡슐화 손상: useImperativeHandle을 남용하면 컴포넌트의 내부 구현이 외부에 노출되어, 컴포넌트 간의 `의존성이 증가`하고 `캡슐화가 손상`될 수 있습니다.
3. 복잡한 코드: 외부에서 자식 컴포넌트의 메서드를 많이 호출하게 되면, 코드가 복잡해지고 유지보수가 어려워질 수 있습니다.
4. 테스트 어려움: 컴포넌트의 내부 로직이 외부에 노출되면, 테스트가 복잡해지고 의도하지 않은 사이드 이펙트가 발생할 수 있습니다.

[코드 출처](https://react-ko.dev/reference/react/useLayoutEffect)
