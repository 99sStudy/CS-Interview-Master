# Next.js edge runtime은 무엇이고 어떤 장점이 있을까요?

- edge runtime은 `웹 애플리케이션의 일부분을 사용자에게 가장 가까운 서버에서 실행하는 기술`입니다. 이를 통해 애플리케이션의 응답 시간을 단축하고, 사용자 경험을 향상시킬 수 있습니다.
- Next.js는 edge runtime을 통해 `글로벌 사용자에게 더 빠른 웹 페이지 로딩 속도를 제공`합니다.

### The Edge는 유저와 가장 가까운 network의 가장자리 라는 일반화된 컨셉이다.

`CDNs` 또한 network의 가장자리에 정적인 컨텐츠(HTML, images)을 저장하기 때문에, the Edge의 일종이라고 할 수 있다.

the Edge 역시 전세계 여러 곳에 분포하고 있다는 점에서 CDNs와 더 비슷하게 다가오는데, `두 개념의 차이는 무엇일까?`

`CDNs`는 `정적인 컨텐츠를 저장하는 것`에 그치지만,
`Edge`의 경우, `code를 저장(caching)` 할 수 있을 뿐만 아니라 `코드를 실행(code-execution) 할 수 도 있다는 점`에서 그 차이가 있다.

`User와 더 가까운 the Edge에서 코드를 실행 함`으로써,
지금까지 Client-Side에서 진행되거나 Server-Side에서 진행되던 `작업을 Edge로 옮길 수 있게 되었다.`
(see examples with Next.js here)

이를 통해서, client쪽으로 전달되어야 하는 코드의 양을 줄일 수 있을 뿐만아니라, request가 code-execution을 위해 origin server까지 가야 했던 빈도를 줄일 수 있게 됐다.

Next.js에서, 우리는 Middleware와 React Server Components를 통해서 Edge에서 코드를 실행 시킬 수 있다.
