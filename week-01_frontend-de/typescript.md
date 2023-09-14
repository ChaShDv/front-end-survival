# TypeScript

* [공식문서](https://typescriptlang.org) 적극 활용
*   REPL 사용하기

    > npx ts-node # ts-node를 실행
*   기본 문법(구조)

    ```typescript
    // 선언
    let [변수명]: [타입];
    let [변수명]: {
        //타입 정의
        [variable_1]: [type1];
        [variable_2]: [type2];
    };
    ```
*   타입 재정의

    ```typescript
    // 타입 정의
    type [타입명] = {
        변수: 타입;
        ...
    }
    // 사용
    let [오브젝트]: [타입명];
    ```
* 변수의 타입에는
  * 기본형 : string, number, ...
  * 구조체
  * 특정 정의 값&#x20;
    * Union Type : [`'a' | 'b' | 'c'`](#user-content-fn-1)[^1]로 정의하면, 세가지 값 중에 하나만 할당이 가능
  * Tuple : 깐깐하게 정의. 타입, 순서를 포맷에 맞추게 하고싶을 때
  *   undefine : 가능은 하지만, 사용하는건 비추천.

      *   그 대신 Optional Parameter로 처리 : `?` 기호 사용

          * ex) `let fruits?: 'apple' | 'berry' | 'orange'`
          * 기본값을 설정

          > `function greeting(name: string = 'newbie'): [return type]{`&#x20;
          >
          > &#x20; `return 'Hi, {name}'`
          >
          > `}`


* 타입추론
  * \[변수명: 타입;] 형태로 지정하지 않더라도, 할당된 값으로 타입을 (나중에)정한다
* Intersection Type
  * 타입을 확장할 수 있다.
  * 구조체 같은 형태로 정의한 것들을 '&' 기호로 묶어, 모든 타입을 넣도록 할 수 있다
  * 최대한 타입을 쪼개 공통으로 만들고, 필요에 따라 타입을 확장하는 방식을 사용하면 괜찮을 것 같다&#x20;

[^1]: 
