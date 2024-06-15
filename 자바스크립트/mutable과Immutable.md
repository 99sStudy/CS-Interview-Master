# ✨mutable과 immutable에 대해 설명해 주세요.

### mutable

- 변경 가능한 변수의 유형

- 참조 타입

- 해당 데이터 주소를 찾아서 값을 변경함

mutable methods : pop, push, unshift, shift, splice, fill, reverse, sort 등등

### immutable

- 변경이 불가한 변수의 유형

- 원시 타입

- 해당 데이터 주소와 별개의 새로운 주소에 값이 할당됨

immutable methods : concat, filter, find, forEach, includes, indexOf, map, join 등등

(slice, replace, split 등등 문자열 메서드의 경우 전부 immutable methods)

## 🔁꼬리질문

### 🤔불변성을 유지하려면 어떻게 해야하나요?

자바스크립트에서 불변성이란 객체가 생성된 이후 그 상태를 변경할 수 없는 것을 의미합니다.

1. `Object.assign()` 메소드와 `spread 연산자` 이용하기

   - 하지만 객체 내부의 객체가 있다면 얕은복사가 된다..

2. 객체를 불변하게 만들기

   - 객체를 복사할 때 깊은 복사를 해서 새로운 객체를 만들 수 있지만, 아예 객체 자체를 불변하게 만드는 방법이 있습니다.

   - 총 3가지 메소드 `Object.preventExtensions()`, `Object.seal()`, `Object.freeze()`를 통해서 객체의 변경을 막을 수 있습니다.

   1. `Object.preventExtensions()` 메소드

      - `Object.preventExtensions()` 메소드는 이름으로 유추할 수 있듯이, 객체의 확장을 막는 메소드입니다. 새로운 속성을 추가하려고 시도한다면, TypeError가 발생하거나 조용히 실패하게 됩니다.

   2. Object.seal() 메소드

      - `Object.seal()` 메소드는 속성의 추가와 속성의 삭제를 둘 다 막는 메소드입니다. 단, 기존의 속성 값을 변경하는 것은 가능합니다.

   3. Object.freeze() 메소드

      - `Object.freeze()`를 메소드는 속성의 추가, 속성의 삭제, 속성 값 변경을 모두 막는 강력한 메소드입니다

3. `JSON.parse(JSON.stringify(obj))` 사용(깊은복사 가능)

   - JSON 객체를 `stringify` 메서드를 통해 직렬화(serialize)한 뒤, parse 메서드를 통해 이를 역직렬화(deserialize)하는 방식이다.

   - JSON.`stringify()` 는 객체를 json 문자열로 변환하는데, 이 과정에서 원본 객체와의 참조가 모두 끊어지기 때문에 깊은 복사가 가능해진다.

   - 이후 `JSON.parse()`를 통해 다시 자바스크립트 객체로 만들어주면 복사가 완료된다.

   - 간단한 방법이지만, 객체 내에 함수가 있을 경우 함수를 처리하지 못한다는 단점이 있다.

   - 즉, 간단한 형태의 객체일 때에만 동작한다.

4. 외부 라이브러리 사용 lodash의 `cloneDeep`

   - 가장 안전하고 좋은 방법이다.
   - 외부 라이브러리의 깊은 복사로 불변성을 유지할 수 있다.

### 🤔깊은 복사와 얕은 복사에 대해서 설명해주세요.

`깊은 복사`의 경우 새로운 메모리 공간을 확보하여 완전히 복사하는 것을 의미합니다.

반면, `얕은 복사`의 경우 참조 타입 데이터가 저장한 메모리 주소값을 복사하는 것을 의미합니다.

따라서 얕은 복사 후 원시값과 복사된 값 모두 똑같은 참조를 가리키고 있기 때문에 복사된 값을 수정할 시 원시값에 영향을 끼치게 되므로 주의해야 합니다.
