# Typescript satisfies 연산자

satisfies연산자는 유니온 타입에 사용한다.

```typescript
type objType = {
  name: number | string;
  age: number | string;
};

const obj = {
  namee: "dsdsd", // 오타가 있을 때 타입을 지정해주면 오타가 잡힘 -> obj: objType
  age: 20,
};
```

```typescript
const obj: objType = {
  name: "dsdsd",
  age: 20,
};

//하지만 타입 지정을 하면 union일 때 아래와 같이 메서드를 사용할 수 없음
obj.name.charAt(1);
obj.age.toFixed(1);
```

타입 지정을 지우면 메서드가 사용가능하지만

타입 지정을 하면 메서드가 사용이 안된다.

이는 `name | string` 이기 때문이다.

그래서 아래와 같이 satiesfies를 사용하면 가능하다.

```typescript
const obj = {
  name: "dsdsd",
  age: 20,
} satisfies objType;
```

타입스크립트의 타입 추론은 그대로 활용하면서

추가적으로 타입 검사를 한 번 더 하게 된다.

보통 타입이 유니온으로 타입이 넓을 때 사용한다.
