## ✨useCallback에 대해서 설명해주세요

useCallback은 인수로 넘겨받은 콜백 자체를 기억해두고 반환하는 훅입니다.

함수의 재생성을 막아 불필요한 리소스 또는 리렌더링을 방지하기 위해 사용합니다.

### 🛠️사용법

```
function ProductPage({ productId, referrer, theme }) {
 // Every time the theme changes, this will be a different function...
 // 테마가 변경될 때마다, 이 함수는 달라집니다...
 function handleSubmit(orderDetails) {
   post('/product/' + productId + '/buy', {
     referrer,
     orderDetails,
   });
 }

 return (
   <div className={theme}>
     {/* ... so ShippingForm's props will never be the same, and it will re-render every time */}
     {/*따라서 ShippingForm의 props는 절대 같지 않으며, 매번 리렌더링 됩니다.*/}
     <ShippingForm onSubmit={handleSubmit} />
   </div>
 );
}
```

[코드 출처](https://react-ko.dev/reference/react/useLayoutEffect)
