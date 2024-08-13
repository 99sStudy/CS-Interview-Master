## ✨iterable, iterator, generator에 대해 설명해 주세요.

### 이터러블(iterable)

- 순회 가능한 객체를 의미합니다.

- 이터러블은 반복 가능한 객체를 의미합니다. 배열, 문자열, Map, Set 등이 이터러블에 해당합니다.

### 특징:

- Symbol.iterator 메서드를 갖고 있습니다.
- for...of 루프를 사용하여 순회할 수 있습니다.
- Array.from이나 전개 연산자(...) 등을 사용하여 이터러블 객체를 배열로 변환할 수 있습니다.

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

- 반복을 위해 설계된 특정 인터페이스가 있는 객체이며, {value, done}을 반환하고 next() 메서드를 가집니다.

- 이터레이터는 이터러블 객체를 순회하는 데 사용되는 객체입니다.

- 이터레이터는 **next**() 메서드를 통해 다음 요소를 반환하며, 더 이상 반환할 요소가 없을 때 StopIteration 예외를 발생시킵니다.

### 특징:

- next() 메서드를 가지고 있으며, 호출 시 { value: 요소, done: boolean } 형태의 객체를 반환합니다.
- done 속성이 true가 될 때까지 순회할 수 있습니다.

```javascript
const myArray = [1, 2, 3];
const iterator = myArray[Symbol.iterator]();
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

### 제너레이터(generator)

- iterator 객체를 반환하며, iterable을 생성하는 함수입니다.

- generator를 통해서 iterator를 만들 수 있고 함수 안에서 값을 순회할 수 있습니다.

- 제너레이터 선언은 함수 선언문에 \*(Asterisk)를 붙여 선언합니다.

- 제너레이터는 이터레이터를 생성하는 함수입니다. function\* 키워드를 사용하여 정의하며, yield 키워드를 사용하여 값을 하나씩 반환합니다.

### 특징:

- 제너레이터 함수는 호출될 때 제너레이터 객체를 반환합니다.
- 제너레이터 객체는 이터레이터 인터페이스를 따릅니다.
- yield 키워드를 사용하여 값을 생성하며, 일시 중지된 상태를 유지합니다.
- next 메서드를 사용하여 제너레이터의 실행을 재개할 수 있습니다.

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
