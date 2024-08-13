### Symbol 타입은 무엇인가요?

JavaScript에서 Symbol은 ES6(ECMAScript 2015)에서 도입된 `새로운 원시 데이터 타입`입니다.

주로 `객체의 고유한 속성을 만들기 위해 사용`됩니다.

Symbol은 `항상 고유한 값`이기 때문에, 같은 이름의 심볼이라도 서로 다른 객체에서 사용될 때 충돌하지 않습니다.

### 주요 특징

1. **고유성**: `Symbol()` 함수를 호출할 때마다 새로운 고유한 심볼이 생성됩니다.

   ```javascript
   const sym1 = Symbol("description");
   const sym2 = Symbol("description");
   console.log(sym1 === sym2); // false
   ```

2. **심볼 설명**: 심볼을 생성할 때 문자열 설명을 추가할 수 있지만, 이는 심볼의 고유성에 영향을 미치지 않습니다. 설명은 디버깅 시에만 도움을 줍니다.

3. **객체 속성 키로 사용**: `심볼은 객체의 속성 키로 사용할 수 있으며, 일반적인 문자열 키와는 달리 충돌을 방지할 수 있습니다.`

   ```javascript
   const sym = Symbol("mySymbol");
   const obj = {
     [sym]: "value",
   };
   console.log(obj[sym]); // 'value'
   ```

4. **내장 심볼**: JavaScript에는 몇 가지 내장 심볼이 있습니다. 예를 들어, `Symbol.iterator`는 반복 가능한 객체를 정의하는 데 사용됩니다.

   ```javascript
   const iterable = {
     [Symbol.iterator]() {
       let step = 0;
       return {
         next() {
           step++;
           if (step <= 3) {
             return { value: step, done: false };
           } else {
             return { done: true };
           }
         },
       };
     },
   };

   for (const value of iterable) {
     console.log(value); // 1, 2, 3
   }
   ```

5. **속성 열거에서 제외**: 심볼로 정의된 속성은 `for...in`이나 `Object.keys()`와 같은 메서드로 열거되지 않습니다. 이로 인해 객체의 비공식적인 속성을 정의할 때 유용합니다.

### 사용 예시

```javascript
const USER_ROLE = Symbol("user role");

const user = {
  name: "Alice",
  [USER_ROLE]: "admin",
};

console.log(user[USER_ROLE]); // 'admin'
console.log(Object.keys(user)); // ['name']
```

### 결론

`Symbol`은 고유한 속성을 정의하고 객체의 속성을 분리하는 데 매우 유용합니다. 이로 인해 코드의 안전성과 유지보수성을 높일 수 있습니다.

이런 자료를 참고했어요.
[1] MDN Web Docs - Symbol - JavaScript - MDN Web Docs (https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
[2] GitHub - JavaScript의 Symbol에 대해서. (https://pozafly.github.io/javascript/symbol/)
[3] IT 엘도라도 - [JavaScript] 심볼 (Symbol) 타입 이해하기 - IT 엘도라도 - 티스토리 (https://it-eldorado.tistory.com/149)
[4] 모던 JavaScript 튜토리얼 - 심볼형 (https://ko.javascript.info/symbol)

뤼튼 사용하러 가기 > https://agent.wrtn.ai/5xb91l
