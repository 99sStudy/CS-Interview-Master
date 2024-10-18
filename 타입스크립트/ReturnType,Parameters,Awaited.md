타입스크립트에서 `ReturnType`, `Parameters`, `Awaited`는 유용한 내장 유틸리티 타입입니다. 각각의 기능을 설명하겠습니다.

### 1. ReturnType

`ReturnType<T>`는 함수 타입 `T`의 반환 타입을 추출하는 유틸리티 타입입니다.

#### 예제

```typescript
function exampleFunction(): string {
  return "Hello, TypeScript!";
}

// ReturnType을 사용하여 반환 타입을 추출
type ResultType = ReturnType<typeof exampleFunction>; // ResultType은 string
```

### 2. Parameters

`Parameters<T>`는 함수 타입 `T`의 매개변수 타입을 튜플 형태로 추출하는 유틸리티 타입입니다.

#### 예제

```typescript
function exampleFunctionWithParams(a: number, b: string): void {
  console.log(a, b);
}

// Parameters를 사용하여 매개변수 타입을 추출
type ParamsType = Parameters<typeof exampleFunctionWithParams>; // ParamsType은 [number, string]
```

### 3. Awaited

`Awaited<T>`는 `Promise`의 결과 타입을 추출하는 유틸리티 타입입니다. 일반적으로 비동기 함수의 반환 타입을 다룰 때 유용합니다.

#### 예제

```typescript
async function asyncFunction(): Promise<number> {
  return 42;
}

// Awaited를 사용하여 Promise의 결과 타입을 추출
type AwaitedType = Awaited<ReturnType<typeof asyncFunction>>; // AwaitedType은 number
```

### 요약

- **ReturnType**: 함수의 반환 타입을 추출.
- **Parameters**: 함수의 매개변수 타입을 튜플 형태로 추출.
- **Awaited**: `Promise`의 결과 타입을 추출.
