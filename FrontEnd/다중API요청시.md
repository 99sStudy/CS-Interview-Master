# React에서 다중 비동기 데이터 요청이 필요한 컴포넌트의 성능을 최적화하기 위해 useEffect 훅과 Promise.all을 사용하는 방법을 설명하세요.

```js
import React, { useState, useEffect } from "react";

function MultiRequestComponent() {
  const [data, setData] = useState({}); // API 결과 저장
  const [loading, setLoading] = useState(true); // 로딩 상태
  const [error, setError] = useState(null); // 에러 상태

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      setError(null);
      try {
        // 여러 API를 병렬로 요청
        const [data1, data2, data3] = await Promise.all([
          fetch("https://api.example.com/data1").then((res) => res.json()),
          fetch("https://api.example.com/data2").then((res) => res.json()),
          fetch("https://api.example.com/data3").then((res) => res.json()),
        ]);
        setData({ data1, data2, data3 });
      } catch (err) {
        setError(err.message || "Something went wrong!");
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []); // 빈 의존성 배열로 컴포넌트 첫 렌더링 시 한 번 실행

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h1>Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

export default MultiRequestComponent;
```

# 동시 요청시 문제점과 해결 방법을 설명해주세요.

1. 요청 간의 의존성 문제
   - 요청 결과 중 일부가 다른 요청에 의존하는 경우, 요청이 병렬로 실행되면 `의존 관계를 보장할 수 없어 문제`가 발생할 수 있습니다.
   - 예: 사용자 데이터를 가져오기 전에 인증 토큰이 필요할 때.
2. 요청 실패 시 전체 실패
   - `Promise.all을 사용할 경우, 하나의 요청이 실패하면 전체 요청이 중단`됩니다.
     결과적으로 `성공한 요청의 데이터조차 사용할 수 없게 됩니다.`
3. 네트워크 병목 현상
   - 동시에 너무 많은 요청을 보낼 경우, `네트워크 대역폭이 부족해지고 서버가 과부하 상태`가 될 수 있습니다.
4. 상태 관리 및 UI 동기화 어려움
   - 다수의 요청 상태를 관리하고, 이를 UI에 적절히 반영하는 로직이 복잡해질 수 있습니다.
5. 중복 요청
   - 동일한 데이터를 중복해서 요청하거나, 동일한 컴포넌트에서 `같은 요청이 반복`될 수 있습니다.
6. 메모리 누수
   - 컴포넌트가 `언마운트된 이후에도 요청이 완료`되면, `상태를 업데이트하려고 하면서 경고 메시지`가 나타나거나, `불필요한 리소스가 사용`될 수 있습니다.

## 해결 방법

### 1. Promise.allSettled을 사용하면, 모든 요청의 성공 및 실패 상태를 개별적으로 확인할 수 있습니다.

```js
const fetchMultipleData = async () => {
  const results = await Promise.allSettled([
    fetch("/api/users").then((res) => res.json()),
    fetch("/api/posts").then((res) => res.json()),
  ]);

  const successfulData = results
    .filter((result) => result.status === "fulfilled")
    .map((result) => result.value);

  const errors = results
    .filter((result) => result.status === "rejected")
    .map((result) => result.reason);

  return { successfulData, errors };
};
```

### 2. 요청 상태를 개별적으로 관리하여, 각 요청의 성공, 실패, 로딩 상태를 독립적으로 표시합니다.

```js
function IndividualRequestComponent() {
  const [data1, setData1] = useState(null);
  const [data2, setData2] = useState(null);
  const [loading1, setLoading1] = useState(true);
  const [loading2, setLoading2] = useState(true);

  useEffect(() => {
    fetch("https://api.example.com/data1")
      .then((res) => res.json())
      .then((data) => setData1(data))
      .catch((err) => console.error(err))
      .finally(() => setLoading1(false));
  }, []);

  useEffect(() => {
    fetch("https://api.example.com/data2")
      .then((res) => res.json())
      .then((data) => setData2(data))
      .catch((err) => console.error(err))
      .finally(() => setLoading2(false));
  }, []);

  if (loading1 || loading2) return <p>Loading...</p>;
  return (
    <div>
      {data1 && data2 && <pre>{JSON.stringify({ data1, data2 }, null, 2)}</pre>}
    </div>
  );
}
```

### 3. 이미 요청된 데이터를 캐싱하거나, 상태가 업데이트 중인 경우 추가 요청을 차단합니다.

```js
const cache = new Map();

const fetchWithCache = async (url) => {
  if (cache.has(url)) {
    return cache.get(url);
  }
  const data = await fetch(url).then((res) => res.json());
  cache.set(url, data);
  return data;
};
```

### 4. 컴포넌트 언마운트 시에도 요청이 완료되지 않도록 **AbortController**를 활용합니다.

```js
useEffect(() => {
  const controller = new AbortController();
  const signal = controller.signal;

  const fetchData = async () => {
    try {
      const response = await fetch("/api/data", { signal });
      const data = await response.json();
      console.log(data);
    } catch (err) {
      if (err.name === "AbortError") {
        console.log("Request cancelled");
      } else {
        console.error(err);
      }
    }
  };

  fetchData();

  return () => {
    controller.abort(); // 요청 취소
  };
}, []);
```

### 결론

1. 요청 간 의존성 문제는 순차 요청으로 해결.
2. 요청 실패는 Promise.allSettled을 사용해 부분 성공을 허용.
3. 네트워크 병목 현상은 요청 수 제한 및 배치 처리로 완화.
4. 중복 요청은 캐싱을 통해 방지.
5. 메모리 누수는 AbortController를 활용해 해결.
