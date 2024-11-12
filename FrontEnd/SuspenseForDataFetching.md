# React의 Suspense for Data Fetching 기능을 설명하고, 이를 통해 데이터 로딩을 비동기적으로 처리하는 방법을 설명해주세요.

React의 **Suspense for Data Fetching** 기능은 비동기 데이터 로딩을 손쉽게 처리할 수 있게 하여, 컴포넌트가 필요로 하는 데이터가 로드될 때까지 기다렸다가 화면을 렌더링하는 방식을 제공합니다.

이를 통해 **React 컴포넌트의 로딩 상태 관리**를 간소화하고, `fallback` 컴포넌트를 통해 로딩 중에 사용자에게 시각적 피드백을 제공할 수 있습니다.

## 1. Suspense for Data Fetching의 개념과 동작 방식

- **Suspense**는 `원래 코드 스플리팅을 위해 도입`되었지만, 이제 **비동기 데이터 로딩**에서도 활용할 수 있습니다.

- React는 **데이터 로딩이 완료될 때까지 Suspense 컴포넌트 내에서 다른 하위 컴포넌트를 렌더링하지 않고**, 대신 `fallback`으로 지정한 로딩 컴포넌트를 렌더링합니다.

- 데이터가 로딩되면, Suspense 내부의 컴포넌트들이 한 번에 화면에 나타나게 됩니다.

#### 동작 방식 예시

React에서 Suspense를 사용해 데이터 로딩을 처리하는 기본적인 구조는 다음과 같습니다:

```javascript
import React, { Suspense } from "react";

const MyComponent = React.lazy(() => import("./MyComponent"));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <MyComponent />
    </Suspense>
  );
}
```

- 위 예시는 코드 스플리팅 예시이지만, Data Fetching에서도 비슷한 방식으로 Suspense를 사용할 수 있습니다.

- 특히 Relay, React Query 같은 데이터 패칭 라이브러리에서 Suspense 기능을 지원하여 컴포넌트 내에서 데이터가 로드될 때까지 `fallback` 컴포넌트를 보여줄 수 있습니다.

## 2. Suspense for Data Fetching의 예시

React Suspense를 활용해 비동기 데이터 패칭을 처리할 때 다음과 같은 방식으로 로딩 상태를 처리할 수 있습니다. **React Query**와 같은 라이브러리와 함께 사용하면 쉽게 구현할 수 있습니다.

```javascript
import React, { Suspense } from "react";
import { useQuery } from "react-query";

const fetchUserData = async () => {
  const response = await fetch("https://api.example.com/user");
  return response.json();
};

function UserProfile() {
  const { data } = useQuery("userData", fetchUserData, { suspense: true });
  return <div>Welcome, {data.name}!</div>;
}

function App() {
  return (
    <Suspense fallback={<div>Loading user data...</div>}>
      <UserProfile />
    </Suspense>
  );
}
```

이 예시에서 `useQuery`의 `suspense` 옵션을 사용해 데이터를 로딩하는 동안 `fallback`에 지정한 컴포넌트를 렌더링할 수 있습니다.

# Suspense를 사용할 때 발생할 수 있는 성능 및 사용자 경험과 관련된 문제점과 이를 최적화하기 위한 전략을 설명해보세요.

- Suspense를 사용할 때 **성능 및 사용자 경험과 관련된 몇 가지 문제점**이 발생할 수 있습니다.

- 이를 최적화하는 다양한 전략도 함께 살펴보겠습니다.

### 문제점 1: 긴 로딩 시간으로 인한 사용자 경험 저하

- `fallback` 컴포넌트가 길게 나타나면 사용자에게 불편을 줄 수 있습니다.

- 데이터 로딩 시간이 길어질수록 `Loading...`과 같은 화면이 지속되며 사용자 경험이 저하될 수 있습니다.

### **해결 전략: Skeleton UI 사용**

- `fallback`에 단순한 "Loading..." 문구 대신, **Skeleton UI**를 사용해 콘텐츠의 윤곽을 미리 보여줌으로써 사용자에게 더 나은 경험을 제공합니다.

- Skeleton UI는 로딩 중에도 페이지 레이아웃을 유지하면서 콘텐츠가 채워질 것을 예측할 수 있어 로딩에 대한 불편함을 줄입니다.

- **예시**
  ```javascript
  <Suspense fallback={<UserSkeleton />}>
    <UserProfile />
  </Suspense>
  ```

### 문제점 2: 여러 데이터 소스의 의존성으로 인한 대기 시간 증가

Suspense는 하나의 Suspense 컴포넌트 내에서 다수의 비동기 작업을 처리할 때, **모든 비동기 작업이 완료될 때까지 기다리기 때문에 로딩 시간이 길어질 수 있습니다**.

### **해결 전략: 여러 개의 Suspense 컴포넌트 분리**

- `개별 컴포넌트를 각기 다른 Suspense로 감싸서 데이터를 독립적으로 로딩`하도록 하면, 특정 컴포넌트의 데이터가 준비되는 대로 화면에 표시할 수 있습니다.
- 이를 통해 로딩 지연을 최소화할 수 있습니다.

- **예시**
  ```javascript
  function App() {
    return (
      <>
        <Suspense fallback={<UserSkeleton />}>
          <UserProfile />
        </Suspense>
        <Suspense fallback={<PostsSkeleton />}>
          <UserPosts />
        </Suspense>
      </>
    );
  }
  ```

### 문제점 3: 초기 페이지 로딩 속도 저하

Suspense는 데이터가 로드될 때까지 화면 렌더링을 지연시킬 수 있어, 초기 페이지 로딩이 느려질 수 있습니다.

### **해결 전략: 데이터 프리패칭 (Pre-fetching)**

- 데이터가 필요한 시점보다 **미리 데이터를 불러오는 방식**으로, 이를 통해 초기 로딩 속도를 개선할 수 있습니다.
- `React Query`의 `prefetchQuery`나 `Relay`의 프리패칭 기능 등을 사용하면 컴포넌트가 마운트되기 전에 데이터를 미리 불러올 수 있습니다.

- **예시**

  ```javascript
  import { useQueryClient } from "react-query";

  const queryClient = useQueryClient();
  queryClient.prefetchQuery("userData", fetchUserData);
  ```

#### 문제점 4: 동적 데이터 로딩으로 인한 잦은 리렌더링

사용자 상호작용이나 화면 전환에 따라 로딩이 빈번히 발생할 수 있으며, Suspense의 `fallback` 화면이 자주 노출될 수 있습니다.

### **해결 전략: 캐싱 활용 및 로딩 최소화**

- React Query와 같은 데이터 패칭 라이브러리는 기본적으로 데이터 캐싱을 제공하므로, **자주 변경되지 않는 데이터에 대해서는 캐시를 활용**해 `fallback` 상태로 전환되는 빈도를 줄일 수 있습니다.
- 또한, `staleTime` 옵션을 설정해 데이터를 다시 로드하는 간격을 조절할 수 있습니다.

- **예시**
  ```javascript
  const { data } = useQuery("userData", fetchUserData, {
    suspense: true,
    staleTime: 5 * 60 * 1000, // 5분 동안 캐시된 데이터를 사용
  });
  ```
