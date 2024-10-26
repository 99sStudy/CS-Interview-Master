### ES6에서 생긴 큰 변화들에 대해 설명해 주세요.

1. let & const 키워드

   - 함수 스코프를 가지는 var 키워드와는 다르게 블록 스코프를 가짐

   - 재선언 불가

   - const의 경우 재할당 불가

2. 화살표 함수(Arrow function)

   - 간결한 표현

   - this를 가지고 있지 않아 객체 메서드로 사용하기 부적합

   - 호이스팅 되지 않음

3. for...of 반복문

   - 반복 가능한 객체(Array, String, Map, Set 등등)의 값을 순회

   - Object에 for...of 문법 사용 시 TypeError 발생

   - value를 리턴

4. for...in 반복문

   - 객체의 속성을 순회

   - key를 리턴 (배열의 경우 index)

5. 클래스(class)

   - class 키워드로 클래스 생성

   - constructor로 생성자 정의

   - this로 인스턴스 객체에 접근 가능

   - new로 인스턴스 객체 생성 가능

   - 호이스팅 X

6. 프로미스(Promise)

   - 비동기 처리를 위한 패턴

7. 템플릿 리터럴(Template literals)

   - 문자열 내 표현식을 삽입할 수 있는 리터럴

8. 기본 매개 변수(Default parameters)

   - 함수의 매개변수에 기본값 설정 가능

9. 나머지 매개 변수(Rest parameter)

   - 개수가 정해지지 않은 매개변수들을 배열로 받음

10. 전개 연산자(Spread operator)

    - 반복 가능한 객체를 전개하여 전달

11. 구조 분해 할당(Destructuring assignment)

    - 배열이나 객체의 속성을 해체하여 그 값을 별도의 변수에 할당할 수 있는 표현식

12. 제너레이터(Generator)

    - 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수

13. import & export(가져오기 및 내보내기)
