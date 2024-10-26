# ReactNode와 ReactElement, JSX.Element의 차이점은 무엇인가요?

## 🎈ReactNode

ReactElement의 superset입니다.

ReactNode는 ReactElement일 수 있고 ReactFragment, string, number, ReactNode의 Array, null, undefined, boolean 등의 좀 더 유연한 타입 정의라고 할 수 있습니다.

그래서 보통 어떤 props을 받을 건데, 구체적으로 어떤 타입이 올지 알 수 없거나, 어떠한 타입도 모두 받고 싶다면 ReactNode로 지정해주는 것이 좋습니다.

![이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbM3qPt%2FbtsJjT3ywe5%2Fl9kRb2yF51PuO7IwRbQyQK%2Fimg.png)

## 🎈ReactElement

ReactElement는 `type과 props를 가진 객체`다.

React.createElement를 호출하면 이런 타입의 객체가 리턴됩니다.

```
interface ReactElement<P = any, T extends string | JSXElementConstructor<any> = string | JSXElementConstructor<any>>{ type: T; props: P; key: Key | null; }
```

단순하게 `리액트 컴포넌트를 JSON 형태로 표현`해놨다고 생각하면 됩니다.

## 🎈JSX.Element

JSX.Element는 `ReactElement의 특정 타입`이라고 생각하면 된다.

JSX.Element는 `props와 type이 any`인 제네릭 타입을 가진 ReactElement다.

JSX는 다양한 라이브러리에서 각자의 방식으로 JSX를 구현할 수 있기 때문에 존재한다.

즉 JSX.Element는 React.Element의 type, props 타입을 any로 두어 React.Element보다 `타입을 느슨하게 검사하는 타입`이다.

### 🤔ReactElement 와 JSX.Element의 차이?

현재까지는 JSX.Element은 `props와 type`이 `any`여서 타입 추론이 불가능하지만

`ReactElement`는 `제네릭으로 타입을 지정`할 수 있어 prosp와 type에 대한 자동완성이 지원된다. 라는 점만 이해했다.

또 다른 곳에서는 아래와 같이 설명하기도 한다.

차이가 거의 없다.

`JSX.Element`는 ReactElement 인터페이스를 상속받은 인터페이스이다.

내부 구조나 제약 타입이 별도로 존재하지 않아 완전히 동일하다고 봐도 무방하다.
