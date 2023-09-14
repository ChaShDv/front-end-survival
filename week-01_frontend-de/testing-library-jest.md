# Testing Library(JEST)

* 거의 모든 것을 갖춘 테스팅 도구
  *   거의 모든 것이란?

      \->&#x20;
*   Given-When-Then

    > ```typescript
    > test( [given], () => {
    >     expect([when]).toBe([then]);
    > });
    > ```

    * given : 조건
    * when : 테스트케이스
    * then : 테스트 예상 결과
* BDD 스타일의 장점 : 좀더 고민하며 코드를 작성하게 도와줌 -> 표현력이 좋아짐
* React Testing Library
  * jest-dom
  * UI 테스트에 특화된 라이브러리(거의 _E2E Test_처럼 사용 가능)
