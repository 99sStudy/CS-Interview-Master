## ✨React-Query를 사용해보셨나요? 그렇다면 React-Query가 뭔지 설명해주실 수 있나요?

리액트에서 `서버에 데이터를 요청해서 가져오기` , `데이터를 서버에 업데이트` , `캐싱 작업` 등을 하는데 쉽게 처리할 수 있도록 도와주는 라이브러리 입니다.

쉽게 말해서 리액트에서 `서버 상태를 관리하는데 도움을 주는 라이브러리` 입니다.

## 🔁꼬리질문

### 🤔React-Query를 왜 사용하는건가요? 장점이 무엇인가요?

리액트 쿼리를 사용함으로써 개발자는 `클라이언트 상태`와 `서버 상태`를 `명확하게 분리`하여 관리하고 처리할 수 있습니다.

useState, useEffect로 상태를 관리하고 비동기 로직을 처리하면 되는 거지 왜 굳이 새로운 라이브러리를 배워서 처리를 해야 하지?라고 궁금해할 수 있습니다.

1. 클라이언트 상태와 서버 상태의 분리

   - 리액트를 사용해서 개발하다 보면 `아주 많은 상태들을 생성하고 관리`하게 됩니다.
   - 클라이언트 상태, 서버 상태 등을 따로 구분하지 않고 애플리케이션에 필요한 상태들을 생성합니다.
   - `리액트 쿼리를 사용하면` **서버 상태를 생성하지 않아**도 되고 **isLoading 같은 로딩 상태**인 **클라이언트 상태를 따로 만들지 않아도 됩니다**. 리액트 쿼리가 로딩, 에러 관련 상태를 제공 해줍니다.

2. 최신 상태 유지

   - 두번째로는 리액트에서 사용자가 애플리케이션에 접속 후 어떤 행동도 하지 않는 경우 서버에 데이터를 요청하거나 업데이트를 하지 않았기 때문에 서버 상태와 현재 애플리케이션에서 사용하는 데이터가 달라서 사용자에게 잘못된 데이터를 보여 줄 수 있습니다.
   - 리액트 쿼리에서는 `서버 상태 동기화 기능`을 통하여 서버 상태를 받아와 데이터를 갱신하는 기능이 있습니다.

3. 캐싱, 중복 요청 방지

   - 세 번째로는 `캐싱 기능`이 있어 불필요한 서버 요청을 줄일 수 있습니다.

4. 비동기 요청에 대한 상태 핸들링

   - 비동기 API 요청에 대한 로딩 상태, 결과 값, 에러 상태와 같은 여러가지 상태를 확인하는 기능을 제공합니다.

`그럼 개발자는 관리해야 할 상태가 줄어들고 자연스럽게 코드도 간단해질 것입니다.`

### 🤔서버 상태 동기화 기능에 대해서 자세히 설명해줄 수 있나요?

리액트 쿼리(React Query)의 장점 중 하나는 바로 서버 상태 동기화(Server State Synchronization)입니다.

이 기능은 `클라이언트 애플리케이션이 서버와의 상태를 지속적으로 일치시키는 것을 의미`합니다.

이를 통해 사용자가 애플리케이션에서 아무런 행동을 하지 않더라도 서버의 최신 데이터를 자동으로 가져와 애플리케이션의 상태를 업데이트할 수 있습니다.

서버 상태 동기화의 주요 기능

1. 자동 데이터 갱신 (Automatic Refetching):

   - `특정 이벤트(예: 페이지 포커스, 네트워크 재연결) 발생 시` 데이터를 자동으로 다시 요청하여 최신 상태를 유지합니다.

2. 백그라운드 데이터 갱신 (Background Fetching):

   - 사용자가 애플리케이션을 사용하는 동안 `백그라운드`에서 데이터를 주기적으로 갱신하여 최신 데이터를 유지합니다.

3. 캐싱 (Caching):

   - 데이터를 캐시에 저장하여 불필요한 서버 요청을 줄이고, 캐시된 데이터가 오래되었을 경우 자동으로 갱신합니다.

4. 데이터 무효화 (Invalidation):
   - 특정 조건이 만족되면 데이터를 무효화하고, 무효화된 데이터는 다시 요청하여 최신 상태를 유지합니다.

이러한 기능들을 통해 리액트 쿼리는 서버와 클라이언트 간의 데이터 일관성을 유지하며, 사용자에게 항상 최신의 정확한 데이터를 제공합니다.

### 🤔클라이언트 상태와 서버 상태란 무엇인가요?

#### 클라이언트 상태

웹 브라우저 세션과 관련된 모든 정보.
서버와 무관하게 사용자의 상태를 추적하는 것!

#### 서버 상태

서버에 저장되어 클라이언트에 표시되는 데 필요한 데이터!

### 🤔React-Query의 등장배경

React-Query는 클라이언트 애플리케이션에서 서버 상태를 효율적으로 관리하고 데이터를 가져오는 과정을 단순화하기 위해 등장한 라이브러리입니다. 그 등장 배경에는 여러 가지 이유가 있습니다.

1. 서버 상태 관리의 복잡성

   - 전통적으로, 클라이언트 애플리케이션에서 서버 데이터를 관리하는 것은 상당히 복잡한 작업이었습니다. 데이터를 가져오고, 캐싱하고, 업데이트하고, 상태를 동기화하는 과정에서 많은 코드와 복잡한 로직이 필요했습니다.
   - Redux와 같은 상태 관리 라이브러리는 클라이언트 상태 관리에는 강력했지만, 서버 상태 관리에는 다소 불편한 점이 있었습니다. 서버 상태는 비동기적이고 외부 소스에서 가져오는 데이터이기 때문에, 이를 효율적으로 다루기 위해서는 추가적인 도구가 필요했습니다.

2. 반복적인 데이터 페칭 로직

   - 대부분의 클라이언트 애플리케이션에서는 서버로부터 데이터를 가져오는 로직이 반복적으로 사용됩니다. 이 과정에서 데이터 페칭, 로딩 상태 관리, 에러 처리, 캐싱, 데이터 갱신 등의 작업이 자주 발생합니다.
   - 이러한 반복적인 작업을 간소화하고, 코드의 중복을 줄이기 위해서는 보다 추상화된 도구가 필요했습니다.

3. 데이터 일관성 유지

   - 클라이언트 애플리케이션에서 서버 상태와의 일관성을 유지하는 것은 중요합니다. 서버 데이터가 변경되었을 때 이를 클라이언트 애플리케이션에 즉시 반영하여 사용자에게 최신 상태를 보여주는 것이 필요합니다.
   - 이를 위해서는 데이터를 주기적으로 갱신하고, 특정 이벤트(예: 페이지 포커스, 네트워크 재연결) 발생 시 데이터를 다시 가져오는 기능이 필요했습니다.

4. 비동기 데이터 관리의 필요성
   - 비동기적으로 데이터를 가져오고 관리하는 것은 필수적입니다. 이를 위해서는 비동기 데이터 페칭을 쉽게 처리할 수 있는 도구가 필요했습니다.
