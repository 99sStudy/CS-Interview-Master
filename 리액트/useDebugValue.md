### ✨useDebugValue에 대해서 설명해주세요.

**디버깅할 때 유용하게 사용할 수 있는 훅입니다.**

### 🛠️사용법

리액트 애플리케이션을 개발하는 과정에서 디버깅 하고 싶은 정보에 해당 훅을 사용하면 리액트 개발자 도구에서 볼 수 있습니다.

```
import { useDebugValue } from 'react';

function useOnlineStatus() {
  // ...
  useDebugValue(isOnline ? 'Online' : 'Offline');
  // ...
}
```

[코드 출처](https://react-ko.dev/reference/react/useLayoutEffect)

![image](https://github.com/99sStudy/CS-Interview-Master/assets/90139306/cf8bc158-0951-4473-a225-c363956474c7)

주의 사항 : useDebugValue는 반드시 hooks내에서 사용해야 동작합니다.
component의 top level에서 사용하면 아무기능도 하지 않습니다.
