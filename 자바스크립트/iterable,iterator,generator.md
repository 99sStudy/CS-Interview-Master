## ✨iterable, iterator, generator에 대해 설명해 주세요.

### 이터러블(iterable)

- `순회 가능한 객체`를 의미합니다.

- 이터러블은 `반복 가능한 객체를 의미`합니다. `배열`, `문자열`, `Map`, `Set` 등이 이터러블에 해당합니다.

### 특징:

- `Symbol.iterator 메서드`를 갖고 있습니다.
- `for...of` 루프를 사용하여 순회할 수 있습니다.
- `Array.from`이나 `전개 연산자(...)` 등을 사용하여 `이터러블 객체`를 `배열로 변환`할 수 있습니다.

```javascript
const myArray = [1, 2, 3];
for (const item of myArray) {
  console.log(item);
}

// 또는
const iterator = myArray[Symbol.iterator]();
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

### 이터레이터(iterator)

- `반복을 위해 설계된 특정 인터페이스가 있는 객체`이며, `{value, done}을 반환`하고 `next() 메서드`를 가집니다.

- 이터레이터는 이터러블 객체를 순회하는 데 사용되는 객체입니다.

- 이터레이터는 **next**() 메서드를 통해 다음 요소를 반환하며, 더 이상 반환할 요소가 없을 때 StopIteration 예외를 발생시킵니다.

### 특징:

- next() 메서드를 가지고 있으며, 호출 시 { value: 요소, done: boolean } 형태의 객체를 반환합니다.
- `done 속성이 true가 될 때까지 순회`할 수 있습니다.

```javascript
const myArray = [1, 2, 3];
const iterator = myArray[Symbol.iterator]();
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

### 제너레이터(generator)

- 제너레이터는 `이터레이터를 생성하는 함수`입니다.` function\* 키워드를 사용`하여 정의하며, `yield 키워드를 사용하여 값을 하나씩 반환`합니다.

- 제너레이터 함수는 호출될 때 즉시 실행되지 않고, Iterator 객체를 반환합니다.

- 이 Iterator 객체를 통해 next() 메소드를 호출할 때마다 함수 내부의 다음 yield 문까지 실행되고, 해당 위치의 값이 반환됩니다.

- 제너레이터를 사용하면 복잡한 비동기 코드를 동기 코드처럼 간결하게 표현할 수 있기 때문입니다.

- 제너레이터와 함께 사용되는 `co 라이브러리`나 `async/await` 구문은 `내부적으로 제너레이터를 사용하여 비동기 코드의 실행을 관리`합니다.

```javascript
function* myGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = myGenerator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```
