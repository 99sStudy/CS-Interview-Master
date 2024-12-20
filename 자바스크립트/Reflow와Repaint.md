# ✨리플로우란 무엇인지 설명해주세요

리플로우란, 웹 페이지 내에서 `요소의 위치 또는 크기에 변화`가 있을 때, 변화된 레이아웃을 다시 계산하여 렌더 트리에 적용하는 과정을 의미합니다.

`width, height, padding, margin, border-width`와 같은 크기 관련 속성

`position, top, left`와 같은 위치 관련 속성

`display, flex` 속성과 같은 레이아웃 관련 속성

`font-size, font-weight`와 같은 폰트 크기 관련 속성이 `리플로우`를 유발하는 속성입니다.

# ✨리페인트란 무엇인지 설명해주세요

리페인트란, 웹 페이지 내에서 요소의 `시각적인 표현에 변화`가 있을 때, 변화된 시각적 표현을 다시 계산하여 렌더 트리에 적용하는 과정을 의미합니다.

`color, background-color` 같은 색상 관련 속성

`border-color, border-radius` 와 같은 테두리 관련 속성이 `리페인트`를 유발하는 속성입니다.

## 🔁꼬리질문

### 🤔리플로우와 리페인트의 성능상 차이점을 설명해주세요

`부모 노드의 레이아웃 변화는 자식 노드의 레이아웃까지 영향을 미치기 때문`에, 리페인트와는 달리, 리플로우가 발생하면 하위 렌더 트리를 다시 계산하고 재구성하는 과정이 필요합니다.

따라서, `리플로우는 그 자체만으로도 부하가 큰 작업`입니다.

또한, `리플로우가 발생하면 일반적으로 리페인트도 다시 발생`하기 때문에, 성능에 큰 영향을 끼친다고 할 수 있으며, 렌더링 성능을 최적화하기 위해선 리플로우를 최소화해야 합니다.

또한,`리플로우는 주로 CPU를 활용`하여 연산하는 반면, `리페인트는 GPU를 활용`한다는 차이도 있습니다.

### 🤔리플로우를 최소화하기 위한 방법은 무엇이 있을까요?\*

리플로우를 최소화하기 위해, DOM 업데이트를 하나로 묶어 `Batch Update하는 방법`을 생각해볼 수 있습니다.

```javascript
//비효율적인 방법
for (let i = 0; i < 100; i++) {
  let item = document.createElement("div");
  item.textContent = i;
  document.body.appendChild(item);
}

//Batch Update 방법
let fragment = document.createDocumentFragment();
for (let i = 0; i < 100; i++) {
  let item = document.createElement("div");
  item.textContent = i;
  fragment.appendChild(item);
}
document.body.appendChild(fragment);
```

또한, offsetHeight, offsetWidth와 같은 자바스크립트의 `레이아웃 속성에 여러 번 접근하면 리플로우가 발생`할 수 있기 때문에, 이러한 속성들은 변수에 저장해 두고 재사용해야 합니다.

```javascript
let height = element.offsetHeight; //이처럼 변수에 저장해서 사용
for(let i =0; i<100; i++){
    //아래와 같이 여러번 접근시 reflow 발생
    if(element.offsetHeight > 100)
}
```

마지막으로, `가급적 레이아웃 변경이 적은 요소를 사용`해야 합니다.

position 속성을 예로 들면 `fixed`나 `absolute` 같은 값들을 사용할 수 있습니다.

```css
.fixed-element {
  position: fixed;
  top: 0;
  left: 0;
}
```

### 🤔Composite 단계에 대해서 아시나요?

`Paint 단계에서 생성된 각각의 레이어들을 합성하여 실제 브라우저 화면 위에 표시하는 단계`이다.(포토샵의 레이어 개념과 유사)

composite는 메인 스레드와 별개로 `컴포지터 스레드` 에서 작동한다.

Layout 과 Paint 둘 다 거치지 않는 속성입니다.

- transform, opacity, cursor, orphans, perspective 등이 해당
