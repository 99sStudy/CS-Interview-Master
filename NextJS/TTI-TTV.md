# TTV, TTI, hydration의 관계를 설명해주세요.

- TTV : Time To View, 유저가 최초로 페이지를 보게 되는 데에까지 걸리는 시간
- TTI : Time To Interaction, 유저가 최초로 페이지와 상호작용 할 때 까지 걸리는 시간

### pre-rendering과 hydration의 관계

- pre-rendering : 사용자와 상호작용하는 부분을 제외한 껍데기만을 먼저 브라우저에게 제공합니다. TTV가 엄청나게 빠르겠네요.
- hydration :
  - 이 과정이 일어나기 전까지는 껍데기만 있는 html 파일이기 때문에 사용자가 아무리 버튼을 click 해도 아무 동작이 일어나지 않아요.
  - 인터렉션에 필요한 모든 파일을 다운로드 받는 과정 즉, hydration 과정이 끝나야 그제서야 인터렉션이 가능합니다. 이 간극! TTI를 줄이는 것이 관건이라 할 수 있겠습니다.

Next.js에서 프리렌더링은 두 가지 방식으로 나뉩니다.

- Static Generation (정적 생성): 빌드 시점에 HTML을 미리 생성해 두는 방식입니다.
- Server-side Rendering (서버 사이드 렌더링): 사용자의 요청이 있을 때마다 서버에서 HTML을 생성해 응답합니다.

### Next.js 13의 Pre-rendering 방식 요약

| 프리렌더링 방식       | 설명                                                       | 구현 방법                 |
| --------------------- | ---------------------------------------------------------- | ------------------------- |
| Static Generation     | 빌드 시 정적 HTML 생성, 모든 사용자에게 동일한 콘텐츠 제공 | `getStaticProps` 사용     |
| Server-side Rendering | 요청 시마다 HTML을 서버에서 생성하여 최신 데이터 반영      | `getServerSideProps` 사용 |

### Next.js 14의 Pre-rendering 방식 요약

| 렌더링 방식                         | 구현 방법                      | 예시 옵션 설정                                                      |
| ----------------------------------- | ------------------------------ | ------------------------------------------------------------------- |
| **Static Generation**               | 빌드 시점에 정적 HTML 생성     | `fetch`의 `cache: 'force-cache'` 설정                               |
| **Server-side Rendering**           | 요청 시마다 서버에서 HTML 생성 | `fetch`의 `cache: 'no-store'` 또는 `dynamic = 'force-dynamic'` 설정 |
| **Incremental Static Regeneration** | 일정 시간마다 정적 페이지 갱신 | `fetch`의 `next: { revalidate: <초 단위> }` 설정                    |

---

## CSR과 SSR의 문제

### 🌟 CSR의 경우

- 사용자가 사이트 접속 시, 서버는 비어있는 HTML 문서를 넘겨준다.
- 클라이언트는 HTML에 링크된 JS 파일을 서버에 요청하고, 서버는 해당 JS 파일을 클라이언트에게 전송한다.
- 클라이언트가 JS 파일을 받은 후에야 브라우저 화면에 웹 페이지가 뜨므로, 사용자가 웹 페이지와 바로 상호작용할 수 있다.
- CSR에서는 `TTV === TTI`

### 🚨 SSR의 경우

- 사용자가 사이트 접속 시, 서버는 이미 만들어진 HTML 문서를 클라이언트에게 넘겨준다.

- 이 때문에 사용자는 화면을 바로 볼 수 있다.

- 하지만 아직 JS 파일은 받지 못한 상태이므로, 바로 상호작용이 불가능하다.

- SSR에서는 `TTV !== TTI` (시간차가 존재한다.)

### 문제점

- CSR의 경우 처음 로딩 속도를 줄이기 위해 JS 파일을 분할하여 필요한 정보만 보낼 수 있는 방법이 필요하다.
- SSR의 경우 TTV와 TTI의 시간차를 줄이기 위한 방법이 필요하다.

## 이를 해결하기 위해 등장한 SSG!

- 미리 서버에 HTML 파일을 만들어두고, 이를 사용자에게 보여준다.
- 사용자와의 상호작용 시 데이터가 변하지 않을 페이지는 미리 서버에서 HTML 파일을 만들어두고, 클라이언트의 요청이 있을 때마다 이 `파일을 재사용하는 방식`
  - next build 명령어 입력 시 HTML 파일이 생성되고, CDN을 통해 캐싱된다.
- `SSR처럼 pre-rendering 된 페이지를 보여주지만 변경사항을 업데이트할 수 없다.`
- CSR, SSR보다 `렌더링 속도가 훨씬 빠르다.`

## 코드스플리팅이란 무엇인가요?

- SPA 프로젝트에서는 프로젝트 빌드 시, `하나의 JS 파일로 번들링`을 합니다. 이런 경우 번들링 사이즈가 너무 커지게 되면서 초기 로딩 속도가 상당히 느리게 됩니다`(TTV가 너무 오래걸림)`
- 코드스플리팅을 통해 `큰 번들의 js 파일을 작은 단위로 잘라 필요한 곳에만 로드`하여 `빠른 TTV`를 얻게 할 수 있습니다.
- 특히 Next.js에서는 사용자가 방문하는 페이지에 필요한 자바스크립트 코드만을 로드합니다. 이를 통해 필요한 부분만 우선 로딩하여 Time To View(TTV)를 향상시키고 전체적인 애플리케이션 성능을 개선합니다.
