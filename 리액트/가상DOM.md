# ✨가상DOM에 대해서 설명해주세요.

`VirtualDOM은 웹 성능을 최적화하기 위해 사용되는 DOM 관리 방법으로, 웹 어플리케이션의 상태 변경 시, 객체 형태의 가상 DOM을 통해 변경된 부분만 찾아내어 이를 실제 DOM에 적용하는 기능`을 합니다.

Virtual DOM의 `동작 순서`는 Diffing과 Reconiliation, 크게 두 가지로 구분할 수 있는데,

`Diffing`이란, Virtual DOM에서 변경점을 찾아내는 과정을 의미하며,

`Reconciliation`이란, 찾아낸 변경점을 실제 DOM에 적용하는 과정을 의미합니다.

실제 DOM을 추상화한 개념으로, 변화가 있을 때마다 전체 UI를 실제 DOM에 직접 반영하는 대신, 이 변화를 메모리에 있는 가상 DOM에 먼저 적용합니다.
그런 다음, 실제 DOM과 가상 DOM을 비교하여 실제로 변경된 부분만 실제 DOM에 반영합니다

> Virtual DOM이란, DOM과 유사한 객체를 메모리에 올려놓고,
> 변경 사항이 생기면 변경된 부분만 업데이트를 해서 반응성이 빠른 웹을 구현할 수 있게 해주는 추상화된 DOM을 뜻합니다.
> 결론적으로 DOM을 반복적으로 직접 조작하게 되면 그만큼 브라우저가 렌더링을 자주 하게 되고 자원을 많이 소모하게 되는 문제를 해결하기 위한 기술입니다.

## 🔁꼬리질문

### 🤔가상 DOM이 동작하는 예시를 간단하게 설명해주세요.

1. 먼저, 어플리케이션이 제일 처음 rendering 될 때, ``어플리케이션의 초기 상태를 담은 Virtual DOM을 메모리 상에 하나 생성`합니다.

2. 이후, 어플리케이션이 실행되면서 state나 props가 변경된 부분이 있는 경우, `새로운 버전의 Virtual DOM을 메모리 상에 하나 더 생성`합니다.

3. 새로운 버전의 Virtual DOM이 생성된 후, 이전 버전의 Virtual DOM과 비교하는 과정인 Diffing에 돌입하고, 변경점을 찾아냅니다.

4. 이 과정에서 `두 Virtual DOM 트리의 각 노드를 비교하여 어떤 부분이 변경되었는지 확인`합니다.

5. 변경점을 찾아낸 이후에는, `실제 DOM에 적용하는 과정인 Reconciliation에 돌입`합니다. 이 과정에서 `변경된 부분만 실제 DOM에 업데이트`하기 때문에, `브라우저 성능이 향상`될 수 있는 것입니다.

6. Reconciliation이 완료된 이후, 또 다른 변경점이 생기면, `구 버전의 Virtual DOM이 폐기`되고, `새로운 변경 사항을 반영한 최신 버전의 Virtual DOM이 다시 생성`됩니다.

### 🤔그럼 state나 props가 변경될 때마다 Diffing과 Reconciliation이 수행되는건가요?

React를 비롯하여 Virtual DOM을 사용하는 대부분의 프레임워크에서는 `Batch 업데이트를 지원`하고 있습니다.

따라서, 짧은 시간 안에 여러 개의 state와 props가 동시에 변경되면, 이를 각각 처리하는 것이 아니라, `한꺼번에 모아서 처리`합니다.

### 🤔Virtual DOM을 사용하는 것이 그렇지 않은 것보다 좋은가요?

항상 그런 것은 아닙니다. 간단한 어플리케이션의 경우에는 `Virtual DOM을 사용하는 것이 오히려 오버헤드를 초래`할 수 있습니다.

왜냐하면 Virtual DOM 자체도 `메모리 공간을 차지`하고, `Diffing하는 과정 역시 CPU를 활용`하기 때문입니다.

다만, DOM 트리가 복잡하고, 상태 변경도 빈번하게 일어나는 대규모 어플리케이션에서 사람의 인지 능력으로는 정확히 어떤 DOM을 업데이트해야 하는지 식별하기 어렵기 때문에, Virtual DOM을 사용하는 것입니다.

따라서, 어플리케이션의 `복잡도`와 `요구 사항`에 맞게 `Virtual DOM 적용 여부를 결정`하는 것이 좋습니다.

## ✍️추가 정리

### 🤔React 18에서 추가된 Automatic Batching 이란?

- React 18 버전 이하에서는 오직 React 의 이벤트 핸들러 내부의 state update 작업에 대해서만 Batching 이 가능했다.`하지만 Promise나 setTimeout, Native Event Handler 내부의 작업은 불가능`했다.
- 왜냐하면 이전에는 브라우저의 이벤트가 실행되는 중에만 Batching 작업을 수행했기 때문이다. 따라서 이벤트가 종료된 후에 실행되는 경우는 Batching 작업이 불가능했다.
- 하단의 코드의 경우 state update 작업이 비동기적으로 처리되어 Event가 종료된 후에 실행되기 때문에, React의 Batching 작업에 걸리지 않아 두 차례 리렌더링을 유발시켰다.

```javascript
function App() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);

  function handleClick() {
    fetchSomething().then(() => {
      // React 17 이전의 버전에서는 해당 작업을 Batching 처리하지 않는다.
      // 왜냐하면 해당 작업은 이벤트가 종료된 이후 (100ms 뒤) 에 실행되기 때문이다.
      setCount((c) => c + 1); // 리렌더링 유발
      setFlag((f) => !f); // 리렌더링 유발
    });
  }

  return (
    <div>
      <button onClick={handleClick}>Next</button>
      <h1 style={{ color: flag ? "blue" : "black" }}>{count}</h1>
    </div>
  );
}

function fetchSomething() {
  return new Promise((resolve) => setTimeout(resolve, 100));
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

- 하지만 React 18 버전 이후부터는 `단순히 이벤트 핸들러 내부 뿐만이 아니라 Promise나 setTimeout, Native Event Handler 같은 작업에 대해서도 Batching 작업을 자동으로 수행`하게 해주었다.
- React 18 에서 제공하는 ReactDOM.createRoot 메서드를 기반으로 렌더링을 진행할 경우 모든 state update 작업은 자동으로 Batching 처리된다. 이 기능을 Automatic Batching 이라고 한다.

### 🤔가상 DOM과 리얼 DOM의 차이를 설명해주세요.

“먼저, `DOM이란 브라우저를 구성하는 수많은 페이지(document)를 이루는 element를 tree 형태로 표현한 것`을 의미합니다

그리고 그 tree의 요소 하나하나를 '노드' 라고 하는데 각각의 노드는 접근과 제어를 할 수 있는 API를 제공합니다.

그래서 Real DOM의 트리형태를 그대로 가져온 복사본 형태가 가상 DOM입니다.”

리얼 돔은 돔 요소를 조작하는 반면에, 가상 돔은 돔 요소를 객체 형태로 메모리에 저장하기 때문에 실제 돔을 조작하는 것보다 훨씬 빠르게 조작을 수행할 수 있습니다.

즉, 실제 돔을 조작하는 것보다 메모리상에 올라와 있는 자바스크립트 객체를 변경하는 작업이 훨씬 더 가벼우므로 가상돔을 사용합니다.

그래서 리액트는 가상 돔을 이용해서 리얼 돔을 변경하는 작업을 상당히 효율적으로 수행합니다.

### 🤔가상 DOM의 장점이 무엇인가요? 왜 쓰는 건가요?

가상 DOM은 웹페이지가 표시해야할 DOM을 일단 메모리에 저장하고, 실제 변경에 대한 준비가 완료됐을 때 실제 브라우저의 DOM에 반영합니다.

이렇게 DOM계산을 브라우저가 아닌 메모리에서 계산하는 과정을 한 번 거치게 된다면 `실제로는 여러번 발생했을 렌더링 과정을 최소화`할 수 있고, `브라우저와 개발자의 부담`을 덜 수 있습니다.

### 🤔 DOM은 무엇인가요?

`HTML이나 XML 같은 마크업 언어로 작성된 문서를 자바스크립트와 같은 프로그래밍 언어가 조작할 수 있도록 하는 인터페이스를 의미`합니다.

DOM은 계층적 구조를 가진 노드 트리로 구성됩니다.

### 🤔 가상DOM이 객체라고 하셨는데 왜 객체인걸까요?

리액트는 `값으로 UI를 표현하는 것이 가상 DOM의 핵심`이라고 피력했습니다.

화면에 표시되는 UI를 자바스크립트의 문자열, 배열 등과 같은 값으로 관리하고 이러한 `흐름을 효율적으로 관리하기 위한 메커니즘이 리액트의 핵심`입니다.
