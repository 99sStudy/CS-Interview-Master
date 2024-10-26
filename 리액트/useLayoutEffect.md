## ✨useLayoutEffect에 대해서 설명해주세요.

useEffect와 동일하지만, `콜백 함수 실행이 모든 DOM의 변경 후에 동기적으로 발생`합니다.

아래와 같은 순서로 진행됩니다.

리액트가 DOM을 업데이트 -> useLayoutEffect를 실행 -> 브라우저에 변경 사항을 반영 ->useEffect를 실행

## 🔁꼬리질문

### 🤔그럼 useEffect나 useLayoutEffect 둘 중에 아무거나 사용해도 괜찮지 않을까요? 어떻게 생각하시나요?

useLayoutEffect는 `DOM 변경 후 동기적으로 발생`합니다.

동기적으로 발생한다는 것은 리액트가 useLayoutEffect의 실행이 종료될 때까지 기다린 다음에 화면을 그린다른 것을 의미합니다.

이 때문에 `컴포넌트가 잠시 동안 일시 중지되는 것과 같은 일이 발생`할 수 있습니다.

이는 웹 애플리케이션 성능에 문제를 일으킬 수 있습니다.

### 🤔그럼 useLayoutEffect는 보통 어떨 때 사용할까요?

DOM은 계산됐지만 이것이 화면에 반영되기전에 하고 싶은 작업이 있을 때 사용합니다.

`DOM 요소를 기반으로 하는 애니메이션`, `스크롤 위치를 제어`하는 등 `화면에 반영되기전 하고 싶은 작업`이 있을 때 사용합니다.

### 🛠️사용법

```
function Tooltip() {
  const ref = useRef(null);
  const [tooltipHeight, setTooltipHeight] = useState(0); // You don't know real height yet
                                                         // 아직 실제 height 값을 모릅니다.

  useLayoutEffect(() => {
    const { height } = ref.current.getBoundingClientRect();
    setTooltipHeight(height); // Re-render now that you know the real height
                              // 실제 높이를 알았으니 이제 리렌더링 합니다.
  }, []);

  // ...use tooltipHeight in the rendering logic below...
  // ...아래에 작성될 렌더링 로직에 tooltipHeight를 사용합니다...
}
```

[코드 출처](https://react-ko.dev/reference/react/useLayoutEffect)
