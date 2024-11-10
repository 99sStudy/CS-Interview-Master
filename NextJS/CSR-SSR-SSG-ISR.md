# ✨Next.js에서 제공하는 렌더링 기법에는 어떤 것들이 있나요? 각각의 특징과 구현방법을 간단히 설명해주세요.

- CSR(Client-Side Rendering) : 클라이언트 사이드 렌더링은 서버에서 빈 HTML을 보내고, 브라우저가 자바스크립트를 통해 필요한 데이터를 가져와 화면을 렌더링하는 방식입니다. 초기 로딩 속도가 느리지만, 이후 페이지 전환이 빠릅니다.
- SSR(Server-Side Rendering) : 사용자의 요청 시마다 서버에서 HTML을 동적으로 생성하여 전송하는 방식. 동적 콘텐츠에 적합
- SSG(Static Site Generation) : 빌드 타임에 미리 HTML 파일을 생성하여 요청 시 정적 파일을 제공하는 방식. 정적 컨텐츠에 적합
- ISR(Incremental Static Regeneration) : 빌드 시점에 페이지를 생성한 후, 설정된 주기에 따라 주기적으로 페이지를 재생성하여 갱신된 데이터를 반영

| 렌더링 방식 | 특징 및 장점                                                | 구현 방법                                 |
| ----------- | ----------------------------------------------------------- | ----------------------------------------- |
| **CSR**     | 클라이언트에서 렌더링, 초기 로딩 속도 느림, 상호작용성 높음 | 클라이언트 컴포넌트 사용                  |
| **SSR**     | 서버에서 요청마다 HTML 생성, 최신 데이터 반영, SEO에 유리   | `getServerSideProps` 사용                 |
| **SSG**     | 빌드 타임에 정적 페이지 생성, 로딩 속도 빠름, SEO에 최적    | `getStaticProps` 사용                     |
| **ISR**     | 정적 페이지 생성 후 설정 주기에 따라 페이지 재생성          | `getStaticProps`의 `revalidate` 옵션 설정 |

Next.js 14 버전에서의 렌더링 기법을 살펴보려면, 먼저 최신 Next.js가 13 버전에서 크게 변화한 구조와 패턴을 도입했음을 이해하는 것이 중요합니다. 특히, `app` 디렉토리를 사용하는 **App Router** 방식이 도입되면서, 파일 기반 라우팅과 서버 컴포넌트, 클라이언트 컴포넌트의 개념이 새롭게 정립되었습니다. 이러한 변화는 Next.js 14에서도 지속되며, 다음과 같은 렌더링 방식을 지원합니다.

---

## Next.js 14에서의 렌더링 방식

| 렌더링 방식 | 특징 및 장점                                                            | 구현 방법                                                        |
| ----------- | ----------------------------------------------------------------------- | ---------------------------------------------------------------- |
| **CSR**     | 브라우저에서 렌더링, 초기 로딩이 느리지만 상호작용이 많은 페이지에 적합 | `"use client";` 지시어 사용                                      |
| **SSR**     | 서버에서 요청마다 HTML 생성, 동적 데이터에 적합, SEO 유리               | `dynamic` 옵션을 `force-dynamic`으로 설정하여 서버 컴포넌트 사용 |
| **SSG**     | 빌드 타임에 정적 페이지 생성, 빠른 로딩과 SEO 최적화                    | 서버 컴포넌트에서 `cache: 'force-cache'` 설정                    |
| **ISR**     | 정적 페이지 주기적 재생성, 최신 데이터 유지                             | `revalidate` 옵션을 사용하여 특정 시간 간격마다 재생성           |

Next.js 14에서는 기존의 `getStaticProps`, `getServerSideProps` 없이도 서버 컴포넌트를 통해 다양한 데이터 패칭과 렌더링 방식이 가능합니다.

이를 통해 코드의 일관성과 간결성을 유지하면서 최적화된 웹 애플리케이션을 개발할 수 있습니다.

### + dynamic 사용방법

페이지에서 dynamic 옵션을 설정해 전체 페이지의 렌더링 방식을 지정할 수 있습니다.

예를 들어, 아래 코드처럼 페이지 최상단에 dynamic = "force-dynamic"을 설정하면 요청마다 새로운 데이터를 패칭해 페이지를 SSR로 렌더링합니다.

```javascript
// app/ssr/page.tsx - dynamic 옵션으로 SSR 구현
export const dynamic = "force-dynamic"; // 매 요청마다 데이터를 새로 패칭하여 SSR로 렌더링

async function SSRPage() {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();

  return (
    <div>
      <h1>Server-Side Rendering with force-dynamic</h1>
      <p>{data.message}</p>
    </div>
  );
}

export default SSRPage;
```

### dynamic 옵션의 종류와 사용법

- "force-dynamic": 요청 시마다 서버에서 데이터를 다시 패칭하여 SSR로 페이지를 렌더링합니다.
- "force-static": 페이지를 정적으로 생성하여 SSG로 처리합니다. (빌드 시점에 HTML 생성)
- "auto" (기본값): Next.js가 자동으로 동적 또는 정적 방식을 선택합니다.
