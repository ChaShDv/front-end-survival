# React

* 참고문서
  * React 공식문서 vs React Beta 문서
  * React Beta 문서가 더 최신 사용법을 다룸(업데이트가 빠름)
  * Thinking in React : React로 작업하는 프로세스 참고할 때
  * [React 코어 개발자가 쓴 React에 대한 이해를 돕는 글](https://overreacted.io/ko/react-as-a-ui-runtime/) (필독!)
*   렌더링

    * 여러 개의 Root를 만들거나, 여러번 rander하는게 가능

    ```typescript
    function Greeting() {
        return ( <p>Hello, friend!</p> );
    }

    // 아래처럼 함수 안에 넣어서 짜는 습관
    function main() {
        const element = document.getElementById('root');
        if (!element) {
            return;
        }
        const root = ReactDOM.createRoot(element);
        root.render(<Greeting />);
    }

    main();
    ```

    * 여러번 rendering 하더라도 변경된 부분만 업데이트 하기때문에 다른 컨트롤이 변경되더라도 영향을 받지 않음
*   리랜더링

    * 코드로 정의된 내용이 실제 브라우저 화면에 그려지는 과
    * React는 컴포넌트의 state 값이 변경될 때 리랜더링한다

