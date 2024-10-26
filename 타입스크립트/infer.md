# infer

TypeScript의 infer 키워드는 `조건부 타입에 사용`되며, `조건식이 참으로 평가될 때 사용할 수 있는 타입을 추론하는 데 필요한 도구`이다

```typescript
const arr = [1, true, "hello"]; // 추론된 타입 (string | number | boolean)[]
```

위 데이터에서 추론된 타입을 가져오고 싶을 때 사용할 수 있다.

```typescript
type El<T> = T extends (infer E)[] ? E : never; // T가 배열 타입((infer E)[])일 경우, E를 추론하여 그 타입을 반환
//(infer E)[]는 "타입 E를 요소로 가지는 배열"을 의미

const element: El<typeof arr> = "hi"; // 추론된 타입 string | number | boolean
```

타입스크립트에서 추론하는 타입을 그대로 가져 온 것이다.
