# Webp란?

WebP는 `구글에서 개발한 고효율 이미지 포맷`으로, 이미지 파일의 용량을 줄여 웹 애플리케이션의 성능을 최적화하는 데 탁월한 성능을 보입니다.

`JPEG와 PNG 대비 압축 효율이 뛰어나`면서도 `이미지 품질을 유지`할 수 있어 최근 많은 웹 애플리케이션에서 활용되고 있습니다.

## Webp의 장점?

WebP는 구글에서 개발한 고효율 이미지 포맷으로, 이미지 파일의 용량을 줄여 **웹 애플리케이션의 성능을 최적화**하는 데 탁월한 성능을 보입니다. **JPEG**와 **PNG** 대비 **압축 효율이 뛰어나면서도 이미지 품질을 유지**할 수 있어 최근 많은 웹 애플리케이션에서 활용되고 있습니다.

---

### WebP 포맷의 장점

1. **압축 효율성**

   - WebP는 손실(Lossy) 및 무손실(Lossless) 압축 모두 지원합니다. 손실 압축 기준으로 **JPEG 대비 25~34% 더 적은 용량**을 사용하면서도 이미지 품질이 거의 손상되지 않습니다.
   - 무손실 압축의 경우 **PNG 대비 약 26% 더 작은 용량**으로 저장할 수 있어 네트워크 전송량을 줄이는 데 효과적입니다.

2. **투명도 지원**

   - WebP는 PNG와 유사하게 **알파 채널(투명도)**을 지원합니다. 이는 PNG와 같은 용도로 활용할 수 있으며, 투명도가 필요한 UI 구성 요소나 아이콘에 적합합니다.

3. **애니메이션 지원**

   - WebP는 GIF처럼 애니메이션을 지원하며, GIF와 비교할 때 파일 크기가 더 작아 **애니메이션 배너나 작은 영상**에도 사용할 수 있습니다.

4. **로딩 성능 개선**
   - 용량이 작아지면 로딩 시간이 단축되며, 모바일 환경이나 느린 네트워크에서도 빠르게 로드됩니다. 이는 웹 애플리케이션의 **First Contentful Paint(FCP)**와 같은 사용자 경험 지표를 개선하는 데 도움이 됩니다.

---

### WebP 적용 방법

1. **HTML에 WebP와 대체 이미지 제공**

   - 모든 브라우저가 WebP를 지원하지는 않으므로, `<picture>` 태그를 활용해 WebP와 다른 포맷(JPEG/PNG)을 모두 제공할 수 있습니다.
   - 예제:
     ```html
     <picture>
       <source srcset="image.webp" type="image/webp" />
       <source srcset="image.jpg" type="image/jpeg" />
       <img src="image.jpg" alt="sample image" />
     </picture>
     ```
   - 위와 같이 `srcset`에 WebP 이미지를 먼저 제공하고, WebP를 지원하지 않는 브라우저에서는 JPEG 이미지를 표시하도록 설정합니다.

2. **CSS에서 WebP 사용**

   - CSS의 `background-image`에서 WebP를 적용할 때는, **지원 브라우저별 분기 처리**가 필요합니다. `Modernizr`와 같은 JavaScript 라이브러리를 사용하여 WebP 지원 여부를 감지하고, WebP와 일반 이미지 스타일을 조건에 맞게 지정할 수 있습니다.
   - 예제:
     ```css
     .background {
       background-image: url("image.jpg");
     }
     .webp .background {
       background-image: url("image.webp");
     }
     ```

3. **서버에서 동적 변환 및 제공**
   - 서버에서 사용자 브라우저의 WebP 지원 여부를 감지하고, **자동으로 WebP 또는 다른 포맷을 제공**하는 방법도 있습니다. 일부 CDN과 웹 서버(Nginx, Apache)에서는 WebP 변환 및 지원 감지를 자동화하여 브라우저별로 최적의 이미지를 전달할 수 있습니다.

---

### WebP의 성능 최적화 효과

1. **네트워크 비용 절감**

   - WebP는 용량이 적기 때문에 페이지 로딩에 필요한 네트워크 대역폭을 줄여줍니다. 특히, 모바일 네트워크 환경이나 데이터가 제한된 환경에서 성능과 비용 모두에 큰 이점을 줍니다.

2. **로딩 속도 개선**

   - JPEG나 PNG 대비 적은 용량 덕분에 **FCP**와 **Largest Contentful Paint(LCP)** 지표가 개선됩니다. 즉, 사용자가 웹페이지에서 주요 콘텐츠를 더 빨리 볼 수 있게 되어 **페이지 반응성**이 높아집니다.

3. **스토리지 최적화**
   - 이미지가 차지하는 용량이 줄어들어 웹 애플리케이션의 전체 스토리지 요구량이 감소합니다. 이는 데이터베이스, 캐싱 시스템에서의 저장 공간을 절약하고 성능을 높이는 데 기여할 수 있습니다.

---

### WebP와 JPEG, PNG의 차이

| 포맷 | 주요 장점                                 | 주요 단점                   |
| ---- | ----------------------------------------- | --------------------------- |
| WebP | 높은 압축 효율, 투명도 및 애니메이션 지원 | 일부 오래된 브라우저 미지원 |
| JPEG | 손실 압축으로 인한 작은 용량              | 투명도 미지원               |
| PNG  | 무손실 압축, 투명도 지원                  | 용량이 큼                   |

---

### WebP 적용 시 고려사항과 브라우저 호환성 문제 해결

1. **대체 포맷 제공**

   - WebP가 지원되지 않는 브라우저를 대비해 `<picture>` 태그와 `source` 요소를 사용하여 **JPEG나 PNG 대체 이미지**를 제공해야 합니다.

2. **자동 변환과 조건부 제공**

   - Nginx나 Apache 서버 설정을 통해 브라우저가 WebP를 지원하는지 확인 후 동적으로 WebP 이미지를 제공하는 설정을 적용할 수 있습니다.
   - CDN을 사용한다면, CDN에서 브라우저에 따라 적절한 포맷을 제공하는 기능을 활용해 호환성을 해결할 수 있습니다.

3. **Modernizr와 같은 라이브러리 활용**
   - 브라우저에서 WebP 지원 여부를 감지하는 Modernizr와 같은 JavaScript 라이브러리를 사용하여, **CSS나 JavaScript에서 이미지 파일을 조건부로 로드**할 수 있습니다.

`<picture>` 태그와 `<source>` 요소는 다양한 이미지 포맷을 조건에 따라 **선택적으로 로드**하여, **브라우저 호환성과 성능 최적화**를 동시에 달성할 수 있도록 도와줍니다. 특히, WebP와 같이 일부 브라우저만 지원하는 이미지 포맷을 사용할 때 매우 유용합니다.

---

# `<picture>`와 `<source>` 태그의 사용 방법

#### 기본 구조

```html
<picture>
  <source srcset="image.webp" type="image/webp" />
  <source srcset="image.jpg" type="image/jpeg" />
  <img src="fallback-image.jpg" alt="Sample Image" />
</picture>
```

1. `<picture>` 태그: 여러 이미지 형식을 포함할 수 있는 컨테이너 역할을 합니다.
2. `<source>` 태그: 각 이미지의 조건(형식, 크기 등)을 지정합니다.
   - `srcset` 속성: 해당 이미지 파일의 경로를 지정합니다.
   - `type` 속성: 이미지 형식을 지정합니다(`image/webp`, `image/jpeg` 등).
3. `<img>` 태그: `<source>`에서 지정한 이미지가 **호환되지 않는 경우 로드될 기본 이미지**입니다. 모든 브라우저에서 지원되므로 반드시 포함해야 합니다.

---

### 예제 1: WebP와 JPEG 형식의 호환성 고려

아래 예제는 WebP 형식을 우선적으로 제공하며, WebP를 지원하지 않는 브라우저에서는 JPEG 이미지를 대신 보여줍니다.

```html
<picture>
  <source srcset="image.webp" type="image/webp" />
  <img src="image.jpg" alt="Sample Image" />
</picture>
```

- **동작 방식**: 브라우저가 WebP 형식을 지원하면 `image.webp`를 로드합니다. WebP를 지원하지 않는 경우 `<img>` 태그의 JPEG 이미지를 사용합니다.

---

### 예제 2: 반응형 이미지와 다양한 해상도 지원

반응형 디자인을 위해 해상도에 따라 다른 이미지를 로드하는 경우도 `<source>` 태그를 활용할 수 있습니다.

```html
<picture>
  <source srcset="image-large.webp 2x, image-small.webp 1x" type="image/webp" />
  <source srcset="image-large.jpg 2x, image-small.jpg 1x" type="image/jpeg" />
  <img src="image-small.jpg" alt="Responsive Image" />
</picture>
```

- **동작 방식**: 화면 해상도가 2배 이상인 장치에서는 `image-large.webp`나 `image-large.jpg`가 로드되며, 그렇지 않은 경우 `image-small.webp` 또는 `image-small.jpg`가 로드됩니다.
- **설명**:
  - `srcset="image-large.webp 2x, image-small.webp 1x"`와 같이 해상도에 따라 적합한 이미지를 지정할 수 있습니다.
  - `type` 속성으로 파일 형식을 구분하여, 우선적으로 로드할 형식(WebP)을 선택하게 합니다.

---

### 예제 3: 화면 크기에 따라 다른 이미지 제공

화면 크기별로 다른 이미지를 로드하여 페이지 로딩 속도를 최적화할 수 있습니다.

```html
<picture>
  <source
    media="(min-width: 800px)"
    srcset="desktop-image.webp"
    type="image/webp"
  />
  <source
    media="(min-width: 800px)"
    srcset="desktop-image.jpg"
    type="image/jpeg"
  />
  <source
    media="(max-width: 799px)"
    srcset="mobile-image.webp"
    type="image/webp"
  />
  <source
    media="(max-width: 799px)"
    srcset="mobile-image.jpg"
    type="image/jpeg"
  />
  <img
    src="mobile-image.jpg"
    alt="Responsive Image for Different Screen Sizes"
  />
</picture>
```

- **동작 방식**: 화면 크기가 800px 이상이면 `desktop-image.webp`(또는 `desktop-image.jpg`)을 로드하고, 799px 이하인 경우 `mobile-image.webp`(또는 `mobile-image.jpg`)을 로드합니다.
- **설명**:
  - `media` 속성을 통해 화면 너비에 따른 조건을 지정할 수 있습니다.
  - 모바일과 데스크톱에서 다른 이미지를 로드하여 **사용자 경험과 성능을 최적화**할 수 있습니다.

---

### `<picture>`와 `<source>` 태그 사용 시 주의사항

1. **대체 이미지 필수 제공**: `<img>` 태그에 기본 이미지 소스를 설정해 호환성 문제를 방지합니다.
2. **파일 형식 순서**: 최신 형식을 먼저 제공하고, 브라우저가 해당 형식을 지원하지 않으면 다음 형식을 로드하는 방식으로 **형식 순서를 지정**해야 합니다.
3. **반응형 이미지 주의**: 해상도와 화면 크기에 따라 최적화된 이미지를 제공해 로딩 속도를 개선하고, 데이터 소모를 최소화하는 것이 중요합니다.

`<picture>`와 `<source>` 태그를 통해 이미지 로딩을 조건부로 최적화하면, 다양한 환경에서 **최적의 성능과 사용자 경험**을 제공할 수 있습니다.
