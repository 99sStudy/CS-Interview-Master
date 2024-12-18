# ✨HTTP 캐싱에 대해서 설명해주세요.

HTTP 캐싱은 웹 브라우저나 서버가 웹 콘텐츠를 `임시 저장`해 두었다가, 동일한 콘텐츠를 다시 요청할 때 `저장된 콘텐츠를 재사용하여 요청을 처리하는 방식`입니다

## 💡HTTP 캐시의 종류

HTTP 응답을 저장해 두는 저장소는 크게 두기지로 분류된다.

1.  사설 캐시(지역 캐시)

- 이는 한 사용자에 의해서만 재활용될 수 있는 것들이 저장되는 저장소이다.
- 최초의 HTTP 요청은 서버에게 전송되어 HTTP 응답을 받아오게 되고, 이를 사설 캐시에 저장해 두면 다음번에 동일한 HTTP 요청이 시도될 때는 서버에 해당 HTTP 요청을 다시 보내지 않고 `저장되어 있는 HTTP 응답을 재활용`하게 된다.
- 대표적인 사설 캐시로는 `브라우저 캐시`가 있다.
- 브라우저 캐시는 기본적으로 사용자가 `HTTP 요청을 통해 다운로드한 모든 문서들을 저장`하고 있다. 이렇게 저장된 문서들은 `뒤로 가기` 혹은 `앞으로 가기`를 할 때, `문서 저장`을 할 때, `페이지 소스 보기`를 할 때 등의 경우에 재활용이 된다.

2. 공유 캐시

- 이는 여러 사용자들에 의해 재활용될 수 있는 것들이 저장되는 저장소이다
- `최초의 HTTP 요청은 공유 캐시를 거쳐` 서버에게 전송되어 HTTP 응답을 받아오게 되고, 이를 공유 캐시에 저장해 두면 다음번에 동일한 HTTP 요청이 공유 캐시에게 전달될 때 서버에 해당 HTTP 요청을 다시 보내지 않고 `저장되어 있는 HTTP 응답을 클라이언트에게 반환`하게 된다.
- 대표적인 공유 캐시로는 프록시 캐시가 있다.
- ISP 혹은 회사 측에서 로컬 네트워크 인프라의 일부로서 구축해둔 `웹 프록시`가 이러한 역할을 담당할 수 있다.
- 그러면 자주 사용되는 데이터들의 경우에 `네트워크 트래픽을 최소화`하여 많은 구성원들이 해당 `데이터를 효율적으로 사용`할 수 있게 되는 것이다.

3. CDN 캐시 (Content Delivery Network Cache)

- CDN 캐시는 전 세계에 분산된 데이터 센터에 콘텐츠를 캐싱하여 사용자에게 더 가까운 서버에서 빠르게 데이터를 제공하는 역할을 합니다.
- CDN은 주로 이미지, 비디오, JavaScript와 같은 정적 콘텐츠를 캐싱하며, 웹사이트가 전 세계 어디서 접속되든 일관된 빠른 응답 속도를 제공합니다.

4. 서버 캐시 (Server Cache)

- 서버 캐시는 웹 애플리케이션 서버에서 생성된 동적 콘텐츠를 임시로 저장하여 서버의 응답 속도를 높입니다.
- 특히 데이터베이스 조회나 복잡한 연산을 통해 생성되는 페이지의 경우, 동일한 요청에 대해 서버가 반복적으로 같은 결과를 제공해야 한다면 서버 캐시를 사용하여 처리 속도를 높일 수 있습니다.

## 💡캐시 제어 헤더

- HTTP 캐싱은 `Cache-Control`, `Expires`, `ETag`, `Last-Modified`와 같은 `HTTP 헤더`를 사용하여 캐시 동작을 제어합니다.

- 각 헤더는 캐시 유지 기간, 캐시 가능 여부, 캐시 갱신 조건 등을 지정합니다.

HTTP 캐싱을 제어하기 위한 HTTP 헤더들은 웹 애플리케이션의 성능을 최적화하고 사용자에게 빠른 응답을 제공하기 위해 중요한 역할을 합니다. 각 헤더가 어떤 방식으로 캐시를 관리하는지 하나씩 자세히 설명하겠습니다.

---

### 1. **Cache-Control 헤더**

- `Cache-Control` 헤더는 `캐시의 저장 방식, 만료 시간, 접근 정책 등을 설정`하여 브라우저나 프록시 서버가 `어떻게 캐시를 관리할지를` 지정합니다.

- 다양한 디렉티브(directive)를 사용해 세밀한 캐싱 제어가 가능합니다.

#### 주요 디렉티브

- `max-age=<seconds>`: 캐시가 유지되는 시간을 초 단위로 지정합니다. 예를 들어, `max-age=3600`이라면 캐시가 1시간(3600초) 동안 유효합니다.
- `no-cache`: 캐시된 콘텐츠를 사용하기 전에 `반드시 서버에서 유효성 검사를 수행`하도록 합니다. 즉, 캐시된 콘텐츠는 유지하되 매번 서버에 변경 여부를 확인합니다.
- `no-store`: `캐시에 저장하지 않도록 설정`합니다. 주로 민감한 정보(예: 로그인 페이지, 결제 정보 등)에 사용됩니다.
- `public`: 모든 캐시 저장소(브라우저, 프록시 캐시 등)가 `이 콘텐츠를 캐시할 수 있음을 명시`합니다.
- `private`: 브라우저와 같은 개별 사용자 단위 캐시에만 저장할 수 있으며, 프록시 서버와 같은 공유 캐시에 저장되지 않습니다.
- `must-revalidate`: 캐시된 데이터의 유효 기간이 지나면 `반드시 서버에 재검증을 요청해야 함을 명시`합니다.

#### 📌 **예시**

- `Cache-Control: max-age=600, public`: 캐시를 10분 동안 유지하며, 모든 캐시 저장소(브라우저, 프록시 등)에서 사용할 수 있습니다.
- `Cache-Control: no-cache, private`: 브라우저 캐시에는 저장할 수 있지만, 사용 시마다 서버에 유효성 검사를 요청하여 최신 데이터를 가져오게 합니다.

---

### 2. **Expires 헤더**

- `Expires` 헤더는 특정 날짜와 시간을 기준으로 `캐시의 만료 시점을 설정`합니다.
- `Cache-Control: max-age`와 같은 방식으로 작동하지만, `절대 시간을 사용한다는 점에서 차이`가 있습니다.
- 일반적으로, `Cache-Control` 헤더가 `Expires`보다 `우선`하며, 현대적인 웹 환경에서는 `Cache-Control`을 더 많이 사용합니다.

#### 형식

- `Expires: Wed, 21 Oct 2023 07:28:00 GMT`  
  위와 같이 `절대적인 시간(UTC)을 지정`합니다. 지정된 시간을 초과하면 캐시가 만료됩니다.

#### 📌 **예시**

- `Expires: Fri, 01 Dec 2023 12:00:00 GMT`: 2023년 12월 1일 정오까지 캐시가 유효하며, 이후에는 캐시가 만료되어 새로 데이터를 가져와야 합니다.

> ⚠️ **주의**: `Cache-Control: max-age`와 함께 설정된 경우, `Cache-Control`이 우선합니다. 즉, `Cache-Control`이 설정되어 있으면 `Expires`는 무시됩니다.

---

### 3. **ETag 헤더**

- `ETag`(Entity Tag) 헤더는 `서버에서 리소스의 버전을 나타내는 고유한 식별자`입니다.
- `리소스가 변경될 때마다 ETag 값이 달라`지므로, 클라이언트는 ETag 값을 통해 캐시된 콘텐츠가 `여전히 최신인지 확인`할 수 있습니다.
- 이를 통해 변경된 리소스에만 새로 요청을 보내고, 그렇지 않으면 캐시된 콘텐츠를 사용할 수 있습니다.

#### 사용 방식

- 클라이언트가 서버에 요청할 때, 이전에 받은 `ETag` 값을 `If-None-Match` 헤더에 포함시킵니다.
- 서버는 `ETag` 값이 동일하면 "304 Not Modified" 응답을 보내고, 클라이언트는 캐시된 데이터를 재사용합니다. 만약 `ETag` 값이 다르다면, 새로운 데이터를 전달합니다.

#### 📌 **예시**

1. **서버 응답 예시**
   ```
   ETag: "12345abcde"
   ```
2. **클라이언트가 다음 요청 시**
   ```
   If-None-Match: "12345abcde"
   ```
3. **서버 응답**
   - `ETag`가 동일하면: `304 Not Modified`로 응답 (데이터 갱신 불필요)
   - `ETag`가 다르면: 새로운 리소스와 함께 200 상태 코드로 응답

> ETag는 파일의 해시값을 이용해 생성하는 경우가 많아, 파일 내용이 조금이라도 바뀌면 새로운 ETag가 생성됩니다. 이를 통해 캐시가 즉시 갱신되므로, 정교한 캐시 제어가 가능합니다.

---

### 4. **Last-Modified 헤더**

- `Last-Modified` 헤더는 `서버에서 리소스를 마지막으로 수정한 날짜와 시간`을 나타냅니다.
- 클라이언트는 `If-Modified-Since` 헤더를 사용하여 `서버에 리소스의 수정 여부를 확인`하고, `변경이 없으면 캐시된 콘텐츠를 사용`합니다.

#### 사용 방식

- 클라이언트가 요청할 때, 이전에 받은 `Last-Modified` 날짜를 `If-Modified-Since` 헤더에 포함시켜 서버에 전송합니다.
- 서버는 리소스가 해당 시간 이후로 변경되지 않았다면 "304 Not Modified" 응답을 반환합니다. 그렇지 않으면 새로운 데이터와 함께 200 상태 코드로 응답합니다.

#### 📌 **예시**

1. **서버 응답 예시**
   ```
   Last-Modified: Tue, 15 Nov 2023 12:45:26 GMT
   ```
2. **클라이언트가 다음 요청 시**
   ```
   If-Modified-Since: Tue, 15 Nov 2023 12:45:26 GMT
   ```
3. **서버 응답**
   - 리소스가 수정되지 않았으면: `304 Not Modified`로 응답
   - 리소스가 수정되었으면: 새 데이터와 함께 200 상태 코드로 응답

> `Last-Modified`는 파일 단위의 시간 정보를 기준으로 하기 때문에, 특정 상황에서는 `ETag`보다 덜 정밀할 수 있습니다. 하지만 `ETag`와 함께 사용하면 더 정교한 캐시 제어가 가능합니다.

---

### 요약 표: 캐시 제어 헤더 비교

| 헤더          | 역할                                 | 주요 속성 및 사용 방식                                    |
| ------------- | ------------------------------------ | --------------------------------------------------------- |
| Cache-Control | 캐시 방식, 시간, 접근 정책 설정      | `max-age`, `no-cache`, `no-store`, `public`, `private` 등 |
| Expires       | 캐시 만료 시간을 절대 시간으로 설정  | `Expires: <날짜 및 시간>`                                 |
| ETag          | 리소스의 고유 버전을 나타내는 식별자 | `If-None-Match`와 함께 변경 여부 확인                     |
| Last-Modified | 리소스의 마지막 수정 시간 표시       | `If-Modified-Since`와 함께 변경 여부 확인                 |

---

## 💡브라우저 캐시를 효과적으로 활용하여 웹 성능을 최적화하는 전략과, 캐싱으로 인해 발생할 수 있는 잠재적인 문제들을 해결하는 방법에 대해 얘기해주세요.

### 브라우저 캐시를 효과적으로 활용하는 전략

1. 정적 리소스에 장기 캐시 설정

- 이미지, CSS, JavaScript와 같은 정적 파일들은 일반적으로 자주 변경되지 않으므로, `장기 캐시를 설정하여 최대한 오래 브라우저에 저장되도록 하는 것`이 좋습니다.
- Cache-Control: max-age와 Expires 헤더를 활용하여 장기 캐시를 설정할 수 있습니다.

2. 파일 버전 관리 (Cache Busting)

- CSS나 JavaScript와 같은 파일이 업데이트될 때 캐시된 오래된 파일을 무시하고 새로운 파일을 가져오도록 하는 방법입니다.
- 주로 파일명에 버전 정보를 추가하거나, URL 쿼리 파라미터를 이용해 캐시를 갱신합니다.

3. 동적 콘텐츠와 정적 콘텐츠 분리

- 자주 변경되는 동적 콘텐츠와 그렇지 않은 정적 콘텐츠를 분리하여 캐시 전략을 다르게 적용합니다.
- 예를 들어, 사용자 데이터나 최신 뉴스 기사처럼 실시간으로 업데이트되는 데이터는 캐싱하지 않거나 Cache-Control: no-cache 설정을 적용해 최신 데이터를 매번 요청하게 합니다.

4. ETag와 Last-Modified를 사용해 조건부 요청 설정

- 파일이 변경되었을 때만 새로운 데이터를 요청하도록 ETag와 Last-Modified 헤더를 활용하여 조건부 요청을 설정합니다.
- 클라이언트가 캐시된 콘텐츠를 서버에 유효성 검사를 요청할 때, 변경이 없는 경우 304 Not Modified 응답을 반환하여 네트워크 트래픽을 줄이고 로딩 속도를 높일 수 있습니다.

### 캐싱으로 인한 문제와 해결 방법

1. 오래된 콘텐츠가 제공되는 문제

- 캐싱된 리소스가 오래되었는데도 사용자가 계속해서 오래된 콘텐츠를 보는 문제입니다.
- 이는 특히, UI가 자주 변경되는 웹 애플리케이션에서 큰 문제가 될 수 있습니다.

- 해결 방법:
  - 파일 버전 관리(Caching Busting)를 통해 리소스가 변경될 때마다 새로운 버전의 파일로 인식되도록 합니다.
  - 파일명에 버전을 추가하거나 쿼리 파라미터를 활용하여 캐시 무효화를 확실히 하는 것이 중요합니다.

2. 개발 중 캐시 문제로 인한 갱신 확인 어려움

- 개발 중에 CSS나 JavaScript 파일을 수정했지만, 브라우저가 캐시된 파일을 사용하여 변경사항이 즉시 반영되지 않는 경우가 있습니다.

- 해결 방법:
  - 브라우저 `개발자 도구에서 "Disable cache(캐시 비활성화)" 옵션을 사용`하거나, `Cache-Control: no-cache와 같은 캐시 비활성화 헤더를 임시로 설정`하여 캐싱을 막을 수 있습니다.
  - 또한, 개발 환경에서 캐시를 무시하도록 설정하는 것도 도움이 됩니다.

3. 캐시된 민감 정보로 인한 보안 문제

- 로그인 페이지나 결제 페이지와 같이 민감한 정보가 포함된 페이지가 캐시에 저장되면 보안 문제가 발생할 수 있습니다.

- 해결 방법:
  - 민감한 정보가 포함된 페이지에는 Cache-Control: no-store와 private 설정을 적용하여 캐시 저장을 금지합니다.
  - 이로 인해 브라우저나 프록시 캐시에 해당 페이지가 저장되지 않아 보안 위험을 줄일 수 있습니다.

4. 모바일 환경에서의 캐시 불일치

- 모바일 네트워크는 데스크탑 환경과 다르기 때문에, 캐시된 콘텐츠가 최신 상태가 아니거나 캐시된 리소스를 새로 다운로드하는 데에 문제가 발생할 수 있습니다.

- 해결 방법:
  - 모바일 사용자에게 자주 업데이트되는 콘텐츠의 경우 Cache-Control: no-cache를 설정하여 네트워크 상태가 불안정해도 매번 최신 콘텐츠를 가져올 수 있게 설정합니다.
  - 네트워크 상태에 따라 캐시 전략을 다르게 적용하는 것도 방법입니다.

5. 빈번한 캐시 무효화로 인한 성능 저하

- 캐시를 너무 자주 무효화하면 오히려 캐시의 장점을 충분히 활용하지 못하고, 리소스 다운로드가 빈번해져 성능이 저하될 수 있습니다.

- 해결 방법:
  - 정적 콘텐츠에 대한 `캐시 만료 기간을 적절하게 설정`하고, `조건부 요청(ETag, Last-Modified)을 통해 필요한 경우에만 리소스를 갱신`하도록 합니다.
  - 이를 통해 불필요한 캐시 갱신을 최소화하고, 서버와의 트래픽을 줄일 수 있습니다.

### 브라우저 캐시 최적화 전략 예시

1. 정적 파일에 버전 파라미터 사용

   - CSS와 JavaScript 파일에 version 파라미터를 추가합니다.
   - 예를 들어 main.css?v=1.2와 같은 형식으로 `파일에 버전을 포함`하면, 파일이 변경될 때마다 새 URL로 인식되므로 캐시 무효화가 쉽게 이루어집니다.

2. API 응답의 캐시 관리

   - 자주 변하지 않는 데이터(API 응답)에는 `Cache-Control: max-age=300`과 같은 캐시 기간을 설정해 `짧은 시간 동안 캐시를 사용`하도록 하고, 자주 변하는 데이터에는 `Cache-Control: no-store`를 설정하여 `항상 최신 데이터를 요청`하도록 합니다.

3. 서비스 워커 활용 (PWA)
   - PWA(Progressive Web App)에서는 `서비스 워커를 이용해 파일을 캐싱`하고, 네트워크 요청을 인터셉트하여 필요한 경우에만 네트워크를 사용하는 캐싱 전략을 구현할 수 있습니다.
   - 이를 통해 `오프라인에서도 빠른 응답을 제공`할 수 있습니다.
