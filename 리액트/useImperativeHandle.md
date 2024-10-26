## ✨useImperativeHandle에 대해서 설명해주세요.

`부모에게서 넘겨받은 ref를 원하는대로 수정할 수 있는 훅`입니다.

부모컴포넌트에서 내려준 ref의 값에 원하는 값이나 액션을 정의할 수 있습니다.

### 🛠️사용법

```
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  // 예를 들어, 전체 <input> DOM 노드를 노출하지 않고 focus와 scrollIntoView의 두 메서드만을 노출하고 싶다고 가정해 봅시다. 그러기 위해서는 실제 브라우저 DOM을 별도의 ref에 유지해야 합니다. 그리고 useImperativeHandle을 사용하여 부모 컴포넌트에서 호출할 메서드만 있는 핸들을 노출합니다:
  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);
  // 이제 부모 컴포넌트가 MyInput에 대한 ref를 가져오면 focus 및 scrollIntoView 메서드를 호출할 수 있습니다. 그러나 기본 <input> DOM 노드의 전체 액세스 권한은 없습니다.

  return <input {...props} ref={inputRef} />;
});
```

[코드 출처](https://react-ko.dev/reference/react/useLayoutEffect)
