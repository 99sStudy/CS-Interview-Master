## 1. index.html만들기

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

## 2. React / ReactDOM 설치하기

-> CDN으로 로드

```html
<script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
```

## 3. React 코드 작성해보기

```html
<script>
  const App = () => {
    return <h1>HELLO</h1>;
  };
  const root = ReactDom.createRoot(document.getElementById("root"));
  root.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>
  );
</script>
```

## 4. 에러 발생

```
Uncaught SyntaxError : Unexpected token '<'
```

- '<'모양의 예상치 못한 토큰이 발견되었다는 내용의 Syntax Error가 발생한다.
- 왜냐하면 아래와 같은 코드는 JSX라는 Javascript의 확장 문법이다.
  ```js
  <React.StrictMode>
    <App />
  </React.StrictMode>
  ```
- 즉 Javascript의 문법과는 다른 것이다.
- JSX없이도 `React.createElement("h1",null,"HELLO")` 와 같이 만들 수는 있지만 어플리케이션 복잡도가 올라갈 수록 이런 방식으로 컴포넌트를 정의하기에는 불편하다.

=> 결과적으로 JSX문법을 사용하기 위한 `트랜스파일러 Babel`이 필요하다.

## 5. Babel을 적용하여 JSX문법 사용하기
