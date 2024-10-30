# ✨CSS의 BEM(Block, Element, Modifier) 방법론과 이를 사용하여 코드의 유지보수성과 확장성을 높이는 방법에 대해 설명해 주세요. 또한, 프로젝트에서 BEM을 적용한 경험이나 다른 CSS 네이밍 방법론을 사용한 경험이 있다면 말씀해주세요.

- `BEM(Block, Element, Modifier) 방법론`은 `CSS 클래스 이름을 구조화하여 코드의 가독성과 유지보수성을 높이는 데 도움을 주는 방법`입니다.

BEM은 다음과 같은 세 가지 구성 요소로 이루어져 있습니다.

- `Block`: 독립적인 구성 요소를 나타냅니다. 예를 들어, `header`, `menu`, `button` 등이 블록이 될 수 있습니다.
- `Element`: 블록의 구성 요소로, 블록에 종속됩니다. 예를 들어, `menu__item`, `button__icon`과 같이 작성합니다.
- `Modifier`: 블록이나 요소의 상태나 변형을 나타냅니다. 예를 들어, `button--primary`, `menu__item--active`와 같이 사용합니다.

```html
<div class="button button--primary">
  <span class="button__icon">✔</span>
  <span class="button__text">확인</span>
</div>
```

### BEM 장점

- 명확한 구조: 클래스 이름이 블록, 요소, 수정자 형태로 명확하게 구분되므로 코드를 이해하기 쉽습니다.
- 유지보수성: 코드가 구조화되어 있어 수정이 필요할 때 어떤 부분을 수정해야 하는지 쉽게 파악할 수 있습니다.
- 확장성: 새로운 요소나 수정자를 추가할 때 기존의 구조를 해치지 않고 추가할 수 있습니다.
- 재사용성: 블록 단위로 독립적인 CSS를 작성할 수 있어 다른 프로젝트에서도 재사용이 용이합니다.

## 🔁꼬리질문

### 🤔 OOCSS(Object-Oriented CSS) 방법론에 대해서 설명해주실 수 있나요?

OOCSS는 `객체 지향 프로그래밍의 개념을 CSS에 적용`한 것입니다.

주로 재사용성과 구조화에 중점을 둡니다.

OOCSS는 두 가지 `주요 원칙`을 따릅니다.

- 구조와 스타일 분리: 스타일과 레이아웃을 분리하여 재사용성을 높입니다. 예를 들어, `버튼의 스타일`과 `버튼이 배치되는 위치를 별도로 정의`합니다.
- 상태와 스타일 분리: `기본 스타일과 상태(pseudo-class) 스타일을 분리하여 관리`합니다. 이를 통해 같은 객체의 다양한 상태를 쉽게 처리할 수 있습니다.

### 장점:

- 코드의 재사용성이 높아집니다.
- 모듈화된 구조 덕분에 유지보수성이 향상됩니다.
- 다양한 컴포넌트 간에 일관성을 유지할 수 있습니다.

```css
/* 구조 */
.button {
  padding: 10px;
  border: none;
  border-radius: 5px;
}

/* 스타일 */
.button--primary {
  background-color: blue;
  color: white;
}

/* 상태 */
.button--disabled {
  background-color: grey;
  cursor: not-allowed;
}
```

### 🤔 SMACSS (Scalable and Modular Architecture for CSS) 방법론에 대해서 설명해주실 수 있나요?

SMACSS는 CSS의 `구조를 더 세분화`하고, `스타일을 모듈화`하여 관리하는 방법론입니다.

SMACSS는 `5가지 카테고리`로 스타일을 나누어 관리합니다.

- `기본(Base)`: 기본적인 HTML 요소에 대한 스타일을 정의합니다.
- `레이아웃(Layout)`: 페이지의 구조를 정의하는 스타일입니다. 전체 레이아웃에 영향을 미치는 스타일을 포함합니다.
- `모듈(Module)`: 재사용 가능한 UI 컴포넌트에 대한 스타일입니다.
- `상태(State)`: 특정 상태(활성, 비활성 등)에 대한 스타일입니다.
- `테마(Theme)`: 사이트의 전반적인 스타일을 정의합니다.

### 장점:

- 코드의 구조가 명확해져서 유지보수가 용이합니다.
- 팀원 간의 협업이 쉬워집니다.
- 스타일이 모듈화되어 있어 새로운 컴포넌트 추가가 용이합니다.

```css
/* Base */
h1 {
  font-size: 2em;
}

/* Layout */
.header {
  display: flex;
  justify-content: space-between;
}

/* Module */
.card {
  border: 1px solid #ccc;
  border-radius: 5px;
}

/* State */
.card--active {
  border-color: blue;
}

/* Theme */
.theme-dark .card {
  background-color: #333;
  color: white;
}
```
