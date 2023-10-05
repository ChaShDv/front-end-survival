---
description: JSX
---

# Week 02\_FrontEnd DE

## JSX

ECMA Script의 xml같은 **문법 확장**(XML-like syntax extension to ECMA Script)

*   ECMA Script : JavaScript의 표준표현. ECMA에서 표준화 중

    JavaScript를 확장한 문법
*   React 부산물이지만 다른 프레임워크에서 사용이 가능

    ⇒ /\* @jsx \[expression] \*/
* 리액트의 createElement를 자바스크립트 코드로 변환

## (React.)createElement

React Element를 만들어냄

<mark style="background-color:yellow;">JSX는 XML처럼 작성된 부분을 React.createElement을 쓰는 JavaScript 코드로 변환한다. 중괄호를 써서 JavaScript 코드를 그대로 쓸 수 있고, 결국은 JavaScript 코드와 1:1로 매칭된다.</mark>

#### Babel로 확인 가능(JSX 테스트 방법)

→ “Presets”에서 “react”를 체크

\-> “Plugins”에서 “@babel/plugin-transform-react-jsx”를 추가(Add Plugin)

#### React.createElement( component, props, …children)

* component : p, div, button … 등의 element. 함수, 특수한 형태의 객체
* props : attribute
*   …children : 태그 안에 들어갈 값. 묶음{} 단위. 기본 문자열이고 다른 형태의 값을 넣고싶으면 {}로 감싸줌



    #### \[특징]
* #### 자바스크립트 코드`{…}`를 기준으로 나눔
* #### \r\n을 무시(trim 처리함). 끝에 공백을 넣어줘야하는 경우`{' '}`처럼 추가해줌.
*   #### 두개의 태그를 나열하듯 사용 불가

    **`,`**나 **`;`**로 명시적 구분이 필요
*   **`<React.Fragment>`**( 혹은**`<> or <div>`** )로 감싸 하나의 태그로 만들어야함

    그 안의 태그들은 children 자리에 React.createElement로 들어감

#### + 상단에

`/* @jsx [expression] */`

선

: React.createElement 대신 사용할 모듈을 선언

#### 최신 JSX style

* Option의 React Runtime 값 을 Automatic으로 바꿔주면

`var _jsxRuntime = require(”react/jsx-runtime”);`

가 추가되고

`React.createElement` 대신 **`(0, _jsxRuntime.jsxs)`**가 들어가게 됨

대신 세번째 이후의 인자들이 두번째 인자의 children: 으로 들어감(재귀적 표현)



## Vertual DOM

가상의 돔을 만들어 사용

UI의 이상적인 또는 가상적인 표현을 메모리에 저장하고, 실제 DOM과 동기화

VDOM vs. DOM

DOM에 반영하기 전, VDOM에서 효율적인 방법을 찾아 그 방법을 반영



## 재조정

선언적 API를 제공하기 때문에, 갱신이 될 때마다 매번 무엇이 바뀌었는지를 걱정할 필요 없음.

비교를 효율적으로 해서 어떤 값이 바뀔지 예측이 가능 → (다른 방법에 비해)손색이 없을 정도로 빠름. 유지보수가 용이

→ 트리는 프랙탈과 같다. 트리의 구성요소는 트리다. 우리는 매번 작은 React Element 트리, VDOM 트리를 만든다. VDOM은 실제 DOM과 비교를 통해 변경사항을 적용한다.

## React Developer Tools

[react devtools extensions](https://github.com/facebook/react/tree/main/packages/react-devtools-extensions)

→ [Strict Mode](https://ko.reactjs.org/docs/strict-mode.html)를 쓰지 않으면 경고함.

### Strict Mode

잠재적인 문제점을 강조하기 위한 툴. 랜더링은 안 하고 경고한다.

React를 위한 strict mode

두번씩 호출해서 두번이 다르다면 side effect여부를 알리는 기능을 함

```
// React Strict Mode 사용
root.render( (
    <React.StrictMode>
        <App />
    </React.StrictMode>
) );
```

## VDOM 쓰는 이유

DOM보다 빠르진 않지만, 충분히 빠르다(느리지도 않다)

유지보수가 가능한 어플리케이션을 만든다(아무렇게나 만들어 느려지거나 오류나는 것을 방지)

신중하게 해야할 작업을 더 효율적으로 하도록 돕는다(일단 개발 후 비교, 비효율적이면 경고)

선언적인 API가 나오는 것이 중요

무조건 빠른 것보단 유지보수와 속도의 균형점을 찾는게 더 중요한 상황이기에 채택됨

VDOM : 이 상태를 효율적으로 그릴 수 있도록 지원해줄게.
