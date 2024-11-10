# NextJS에서 기능이 추가된 fetch에 대해서 설명해주세요.

- Next.js에서 `fetch` 함수는 서버 사이드 렌더링(SSR) 및 정적 사이트 생성(SSG) 시 데이터를 가져오는 데 매우 중요한 역할을 합니다.

- Next.js의 `fetch`는 기본적으로 브라우저 환경과 비슷하게 동작하지만, Next.js에 최적화된 몇 가지 고유의 확장 기능을 추가하여 성능과 효율성을 높였습니다. 여기서 확장된 주요 기능들을 알아보겠습니다.

---

### Next.js의 `fetch` 확장 기능

#### 1. **자동 캐싱(Auto Caching)**

Next.js의 `fetch`는 서버 컴포넌트에서 호출할 때 `기본적으로 캐싱이 활성화`됩니다. 이 캐싱 기능은 성능 최적화와 API 호출 비용 절감을 위해 활용되며, 기본 설정을 통해 캐싱을 제어할 수 있습니다.

- `cache: 'force-cache'`: 기본 설정입니다. 페이지가 새로고침될 때까지 이전에 가져온 데이터를 캐시에 유지합니다.
- `cache: 'no-store'`: 캐싱하지 않고 항상 최신 데이터를 가져옵니다. 이는 실시간 데이터를 필요로 할 때 유용합니다.
- `revalidate: <초 단위>`: 정적 캐시로 저장하지만, 설정한 시간 간격으로 캐시를 새로 고칩니다. 예를 들어, `revalidate: 60`으로 설정하면 60초마다 캐시된 데이터가 갱신됩니다.

```javascript
// 60초마다 데이터를 캐시에서 재검증
const data = await fetch("https://api.example.com/data", {
  next: { revalidate: 60 },
});
```

- `revalidateTag`
  NextJs에는 경로 전반에 걸쳐 요청을 무효화하기 위한 캐시 태깅 시스템이 있다.

fetchAPI를 사용할때 fetch 하나 이상의 태그로 캐시항목에 태그를 지정할 수 있는 옵션이 있다.

그런다음 호출하여 revalidateTag 해당 태그와 관련된 모든 항목의 유효성을 다시 검사할 수 있다.

예를들어 fetch 요청에 collection 태그를 추가한다.

```js
// app/page.tsx
export default async function Page() {
  const res = await fetch("https://...", { next: { tags: ["collection"] } });
  const data = await res.json();
  // ...
}
```

```js
// app/action.ts
"use server";

import { revalidateTag } from "next/cache";

export default async function action() {
  revalidateTag("collection");
}
```

- `오류 처리 및 재검증`

  - 데이터 재검증을 시도하는 동안 `오류가 발생하면 마지막으로 성공적으로 생성된 데이터가 캐시되어 계속 제공`된다.

  - 다음 후속 요청에서 NextJs는 데이터 재검증을 다시 시도한다.

#### 2. **직접적 응답 스트리밍 지원(Edge and Streaming Responses)**

Next.js의 `fetch`는 엣지 서버에서 스트리밍 응답을 직접 지원합니다. 대용량 데이터를 점진적으로 처리하고, 로딩 속도를 향상할 수 있습니다.

- **엣지와 연동**: Next.js는 엣지 서버에서 실행할 수 있으므로, `fetch` 함수로 엣지 네트워크에서 빠르게 데이터를 스트리밍할 수 있습니다. 이렇게 하면 데이터가 준비되는 대로 점진적으로 사용자에게 보여줄 수 있어 페이지 초기 로드 속도를 크게 개선할 수 있습니다.

#### 3. **데이터 직렬화 및 자동 처리**

Next.js는 `fetch`를 통해 가져온 데이터를 자동으로 직렬화하여 서버에서 클라이언트로 전달할 수 있도록 최적화합니다. 이를 통해 서버 컴포넌트에서 `fetch`로 가져온 데이터가 클라이언트로 무리 없이 전달됩니다.

- **데이터 직렬화 지원**: 기본적으로 Next.js는 JSON 데이터를 직렬화하여 클라이언트로 보냅니다. 하지만 JSON이 아닌 Blob이나 FormData와 같은 복잡한 객체는 직렬화되지 않으므로 주의가 필요합니다.

```js
// 서버 컴포넌트 내 데이터 패칭
async function ServerComponent() {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json(); // Next.js가 자동 직렬화하여 클라이언트에 전달 가능

  return (
    <div>
      <h1>{data.title}</h1>
      <p>{data.description}</p>
    </div>
  );
}
```

#### 4. **Prefetching 및 페이지 빌드시 데이터 페칭**

Next.js의 `fetch`는 정적 페이지 생성 시 필요한 데이터를 미리 가져와 페이지 빌드에 포함할 수 있습니다. 이렇게 하면 SSG나 ISR을 통한 최적화된 데이터 렌더링이 가능합니다.

- **prefetch**: `fetch`가 `빌드 타임에 데이터를 가져와 페이지에 포함하도록 설정`할 수 있습니다. 특히 SEO가 중요한 페이지에서 사용 시 효과적입니다.
- **페이지 빌드시 데이터 통합**: `getStaticProps`나 `getServerSideProps` 대신 `fetch`를 서버 컴포넌트에서 사용하면 데이터를 가져와 페이지 렌더링에 사용할 수 있습니다.

#### 5. **CSR 및 SSR 모두에서 동일한 코드 사용 가능**

Next.js는 `fetch`를 클라이언트와 서버 모두에서 사용할 수 있게 하여 개발자가 CSR과 SSR 코드를 일관되게 작성할 수 있게 합니다.

- **동일한 API 사용**: 서버 컴포넌트에서 서버 사이드에서 동작하는 API를 호출하고, 클라이언트 컴포넌트에서는 클라이언트 API를 호출하는 방식으로 일관성 있게 데이터 패칭이 가능하도록 도와줍니다.

#### 6. **자동 에러 핸들링(Auto Error Handling)**

Next.js의 `fetch`는 자동으로 서버 에러를 핸들링하여 예외 상황에서도 안정적인 동작을 보장합니다.

- **Error Boundary**: `fetch`로 호출한 API에서 오류가 발생하면 Next.js는 Error Boundary 컴포넌트를 통해 오류를 잡아 사용자에게 안정적인 화면을 제공합니다.
- **정교한 에러 캐치**: 개발자가 원한다면 특정 `fetch` 호출에 try-catch 구문을 추가하여 직접 에러를 핸들링할 수도 있습니다.

---

### SSG

```js
fetch("https://jsonplaceholder.typicode.com/posts", {
  cache: "force-cache",
});
```

### SSR

```js
fetch("https://jsonplaceholder.typicode.com/posts", {
  cache: "no-cache",
});
```

### ISR

```js
fetch("https://jsonplaceholder.typicode.com/posts", {
  next: { revalidate: 60 }, // ISR을 통한 캐시 재검증 설정 (60초마다 새로 고침)
});
```
