## ✨useState에 대해서 설명해주세요.

함수형 컴포넌트 내부에서 상태를 정의하고, 이 상태를 관리할 수 있게 해주는 훅이다.

## 🔁꼬리질문

### useState는 왜 필요할까요?
컴포넌트 내에서 어느 값을 일반 변수를 선언하여 사용하게 된다면, 컴포넌트가 리런데링 될 때마다 값이 초기화 된다는 문제점. 

### 🤔useState를 자바스크립트로 구현한다면 어떻게 구현하실 건가요?

```
let currentStateKey = 0 // useState가 실행 된 횟수
const states = [] // state를 보관할 배열
function useStateCustom(initState) {
  //클로저로 key를 참조
  const key = currentStateKey

  // initState로 초기값 설정
  if (!states[currentStateKey]) {
    states[currentStateKey] = initState
  }

  // state 할당
  const state = states[currentStateKey]

  const setState = newState => {
    // state를 직접 수정하는 것이 아닌, states 내부의 값을 수정
    states[key] = newState
    //리-렌더링 될 때 0으로 초기화해줘야 자신의 state를 지정함
    currentStateKey = 0
  }
  currentStateKey += 1
  return [state, setState]
}
1.여러 state를 관리하기 위한 index 값과 states 배열을 전역 스코프에 정의합니다..

2. useState 함수는 매개변수로 initialState를 받습니다.

3. useState함수 내부에서는 전역변수로 선언한 index값을 클로저로 가둡니다.

4. 해당 키값으로 값이 있는지 확인하고 없다면 initialState로 초기값을 설정합니다.

5. 현재 state도 클로저로 가둔 index 값으로 할당받습니다.

6. setState 함수는 매개변수로 값 하나를 받고, 클로저로 가둔 index값으로 states 배열 내부를 수정합니다.

7. 이때 리-렌더링을 하는데 여기서 전역 변수에 설정한 index 값을 0으로 초기화 해줍니다.

왜냐하면 리-렌더링이 수행될 때마다 useState함수가 또 호출되므로 전역 변수 index가 무한히 늘어나기 때문입니다.

8. 전역변수 index를 +1 증가시켜서 또 다른 useState를 사용할 수 있도록 합니다.

9. 내부에서 정의한 state와 setState를 사용합니다.
```

여기서 중요한 점은 리-렌더링하는 부분에서 currentStateKey를 0으로 해줘야합니다.

### 🤔말씀하신 코드에서 만약 setState를 수행하다가 변경된 값이 없을 경우에는 어떻게 할 것인가요?

```
// 값이 똑같은 경우
if (newState === state) return

// 배열/객체일 때는 JSON.stringify를 통해 간단하게 비교할 수 있다.
if (JSON.stringify(newState) === JSON.stringify(state)) return
```

setState에서 값이 똑같은 경우에는 early return을 해줍니다.

### 🤔말씀하신 코드에서 만약 setState가 여러개가 동시에 호출되면 여러번 렌더링되어버리지 않나요?

이때는 리렌더링을 하는 부분에 디바운스기법을 사용할 것 같습니다.

### 🤔useState의 게으른 초기화에 대해서 설명해주실 수 있나요?

일반적으로 useState에 기본값을 선언하기 위해 보통 인수로 원시값을 넣는데 함수도 인수로 넣어줄 수 있습니다.

useState에 변수대신 함수를 넘기는 것을 게으른 초기화라고 합니다.

리액트에서는 게으른 초기화를 초깃값이 복잡하거나 무거운 연산을 포함하고 있을 때만 사용하라고 되어있습니다.

이 게으른 초기화 함수는 오로지 state가 처음 만들어질 때만 사용됩니다.

에시로 localStorage에 접근하거나 map,filter, find 같은 배열에 대한 접근, 무거운 계산 등 비용이 많이 드는 경우에 게으른 초기화를 사용합니다.
