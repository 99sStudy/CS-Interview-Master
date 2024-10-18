# ✨CORS란 무엇인가요?

CORS는 `Cross Origin Resource Sharing`의 약자로, 교차 출처 리소스 공유라고도 부릅니다.

2009년에 HTML5 표준으로 채택된 `프로토콜`이며, `SOP에 의해 제한된 교차 출처 간 리소스 공유를 허용하기 위한 방법`입니다.

애플리케이션의 `요구 사항이 복잡`해지면서, `다른 도메인의 리소스를 활용하는 경우가 많아졌기 때문에 등장한 프로토콜`로, 서버에서 CORS 관련 헤더를 설정하여 다른 도메인에서의 리소스 요청을 허용할 수 있습니다.

`CORS 에러`는 `CORS 헤더를 적절히 설정하지 않은 상태에서 교차 출처 리소스를 요청하는 경우 발생`할 수 있습니다.

### 🤔SOP란 무엇인지 설명해주세요

`Same Origin Policy`의 약자로, `동일 출처 정책을 의미`합니다.

1990년대 후반에 등장한 `보안 정책`으`로, `현재 출처와 동일한 출처의 리소스만 접근할 수 있도록 하는 정책`입니다.

여기서 동일 출처란, `도메인과, 프로토콜, 포트 번호가 모두 같은 경우를 의미`하며, 하나라도 다를 경우 동일 출처 정책에 의해 리소스 접근이 제한됩니다.

### 🤔SOP가 없을 경우 가능한 보안 취약점은 무엇인가요?

`사용자 인증 정보에 해당하는 세션 ID같은 정보들이 쿠키에 포함되어 있을 수 있기 때문에 이 세션 정보를 탈취하여 Cross Site Scripting(XSS) 혹은, CSRF와 같은 해킹 공격에 이용`할 수 있습니다

SOP 정책을 통해 리소스를 다른 도메인에서 접근하지 못하도록 제한한다면, 이러한 해킹 공격을 어느 정도 완화할 수 있습니다.

### 🤔CORS 프로토콜이 동작하는 원리를 설명해주세요

서버는 응답 처리 코드에서 CORS 관련 헤더를 설정할 수 있습니다.

```javascript
const express = exquire('express');
const cors = require("cors");
const app = express();

const corsOption = {
    origin: (origin, callback) =>{
        const allowedOrigins = ['http://example.com','http://anotherdomain.com']; // 요청을 허용할 도메인
        if(allowedOrigins.indexOf(origin) !== -1 || !origin){
            callback(null, true);
        }
        else{
            callback(new Error("Not allowed by CORS"))
        }
    },
    method:["GET,"POST","PUT","DELETE"], // 허용할 HTTP 메서드
    allowedHeaders:["Content-Type","Authorization"] // 허용할 요청 헤더의 종류
}

app.use(cors(corsOptions));
```

이 헤더를 통해 요청을 허용할 도메인과, HTTP 메서드, 그리고 요청 헤더의 종류를 정의할 수 있습니다.

이후, 브라우저에서 서버로 리소스를 요청할 때, 이 헤더에 설정한 정보와 일치하지 않는다면, 브라우저에서 CORS 에러가 발생하는 것입니다.

CORS 프로토콜 스펙에서 정의한 비교적 보안적으로 민감하지 않다고 판단되는 요청들이 있는데, 이를 단순 요청이라고 칭하며, 이 요청을 제외한 모든 CORS 요청에는 실제 요청을 전송하기 전, 요청 허가를 위한 preflight 요청이 발생할 수 있습니다.

### 🤔Preflight 요청이란 무엇인지 설명해주세요

preflight 요청은 `보안적으로 민감한 CORS 요청에 대해, 요청이 가능한지를 먼저 확인하는 과정`입니다.

브라우저에서 자동으로 실행되는 요청으로, OPTIONS 메서드를 사용하며, 서버에서 설정한 CORS 관련 설정들을 Header 값으로 확인할 수 있습니다.

이 과정을 통해, 허용되지 않는 요청에 대한 처리 부하를 낮출 수 있습니다.

### 🤔단순 요청이 무엇인지 설명해주세요

요청의 메서드가 `GET, HEAD, POST` 중 하나이며, `헤더`와 `Content-Type`이 `CORS 프로토콜에서 지정한 값인 경우`가 단순 요청에 해당합니다.

이 경우는 `Preflight 과정을 통한 권한 조회 과정 없이 CORS 요청이 가능`합니다.

### 🤔도메인 오리진 차이는 무엇인가요?

#### 도메인 (Domain)

- 도메인은 인터넷에서 `특정 웹사이트를 식별하는 주소`입니다. 예를 들어, example.com, google.com 등이 도메인입니다.
- 도메인은 일반적으로 `IP 주소를 사람이 이해할 수 있는 형태로 변환한 것`입니다.

#### 오리진 (Origin)

- 오리진은 `프로토콜(HTTP/HTTPS), 도메인, 포트 번호로 구성`된 개념입니다. 예를 들어, https://example.com:443와 같이 표현됩니다.
- 만약 프로토콜이나 포트가 다르면, 같은 도메인이라도 서로 다른 오리진으로 간주됩니다. 예를 들어:
- https://example.com (HTTPS, 포트 443)
- http://example.com (HTTP, 포트 80)
- https://example.com:8080 (HTTPS, 포트 8080)

![alt text](https://velog.velcdn.com/images/gnoeyah/post/c196eace-c2cd-4a9c-bc2e-5bdb03fd18ac/image.png)

### 🤔CORS 문제 해결 방법은 무엇인가요?

CORS 문제는 브라우저에서만 발생합니다.

서버와 서버의 요청에서는 COR문제가 일어나지 않습니다.

1. 첫 번째 해결 방법은 `서버에서 응답헤더로 Access-Control-Allow-Origin 헤더를 넣어주는 방법`이 있습니다.

2. 두 번째 해결 방법은` 프론트 자체에서 프록시 서버를 띄워서 대신 요청을 보내게 하는 방법`이 있습니다.

- 이때 프론트와 프록시 서버의 오리진이 같아야합니다.
- 오리진이 같음으로써 프론트와 프록시 서버 사이에서는 CORS문제가 발생하지 않고, 프록서 서버와 서버는 서버끼리의 통신이므로 CORS문제가 발생하지 않습니다.
