---
description: React
---

# Week03\_FrontEnd DE

### thinking in react

1.  Break the UI into a component hierarchy

    (UI를 컴포넌트 계층으로 나눠라)
2.  Build a static version in React

    (정적인 리액트 버전을 구현해라)
3.  Find the minimal but complete representation of UI state

    (UI 상태에 대한 최소한이지만 완전한 표현을 찾아라)
4.  Identify where your state should live

    (어떤 상태를 유지해야하는지 파악하라 )
5. Add inverse data flow

### 데이터

JSON : BackEnd에서 데이터를 돌려주는 API를 제공하는 형태. 보통은 key로 사용할 수 있는 id 값을 넘겨줌.

* REST API
  * REpresentational State Transfer의 약자
  * 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것(Resource 중심)
  * fetch API ( GET, POST, PUT/PATCH, DELETE (CRUD) )
* GraphQL
  * Graph 자료구조 이용
  * Query에서 얻고자 하는 걸 지정
  * Query(Read), Mutation(Command: Create, Update, Delete), Subscription(Event)

FrontEnd는 이 데이터를 사용자가 볼 수 있도록 UI를 구성한다.

React는 선언형(HTML과 유사한 모양의 DSL을 사용)으로 UI를 구성할 수 있다.

도메인 특화 언어(DSL: Domain-Specific Languages) : 관련 특정 분야에 최적화된 프로그래밍 언어

* 명령형 프로그래밍 : HTML과 유사한 모양의 DSL을 사용
* 선언형 프로그래밍 : 선언을 한번 하면, 그 아래 있는 것들이 업데이트 됐을 때 자동으로 업데이트

#### 컴포넌트 계층구조

1.  React 특징

    * Component-Based
    * Build encapsulated components that manage their own state, then **compose** them to make **complex UIs**.

    ⇒ 단순한 기능의 컴포넌트들을 기반으로 복잡한 UI를 만들어낸다
2. 계층을 나누는 구조
   *   단일 책임 원칙(SRP : Single Responsibility Principle)

       : 하나의 컴포넌트는 하나의 기능(책임)을 가진다
   *   CSS

       : 이미 알고 있는 기준을 재활용.
   *   Design’s Layer

       : 디자인은 보통 트리 구조로 나옴. 이를 그대로 활용하는 방법
   *   Information Architecture (JSON Schema의 영향)

       : 실제로 엄청 많이 쓰게 됨. 자연스러운 SRP를 위해서 사실상 강제됨.



\--

### Extract Function

> [Extract Function](https://refactoring.com/catalog/extractFunction.html)

> [Inline Function](https://refactoring.com/catalog/inlineFunction.html)

아주 흔히 쓰이는 SRP를 위한 수단. 변화의 크기(영향 범위)를 제약한다.

일단 길게 코드를 작성하고, 적절히 자를 수 있는 부분이 보일 때 “함수로 추출”한다.

또는 코드를 작성하기 어려운 상황에 직면했을 때 함수로 추출. 바로 다른 파일을 만들어야 한다고 생각하지 않아도 됨.

컴포넌트 나누는 기준이 애매하면 다시 하나의 컴포넌트로 합쳤다가(Inline Method) 다시 나눠줘도 됨.

### Props

> [Passing Props to a Component](https://beta.reactjs.org/learn/passing-props-to-a-component)

> [Components와 Props](https://ko.reactjs.org/docs/components-and-props.html)

나눠진 컴포넌트를 서로 연결하는 방법.

TypeScript를 잘 쓰거나 잘못 쓰게 되는 포인트 중 하나. 적절한 균형점을 잡는 게 중요하다.

테스트 코드를 작성하면 재사용성을 평가하기 쉬워짐.

***

### 또 Thinking in React

> [Thinking in React](https://beta.reactjs.org/learn/thinking-in-react)

* “Step 3: Find the minimal but complete representation of UI state”
* “Step 4: Identify where your state should live”
* “Step 5: Add inverse data flow”

### React의 State

React의 state는 “변경”을 다루기 위한 요소. “Re-rendering”이란 주제에서 다뤄진다. 거칠게 이야기하면, 어떤 컴포넌트의 state가 바뀌면 해당 컴포넌트와 하위 컴포넌트를 다시 렌더링하게 된다.

아무렇게나 막 만들어도 되지만, 일관성과 효율을 위해 DRY 원칙을 따르는 SSOT를 만든다.

* [DRY (Don’t Repeat Yourself)](https://ko.wikipedia.org/wiki/%EC%A4%91%EB%B3%B5%EB%B0%B0%EC%A0%9C)
* [SSOT (Single Source of Truth)](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC\_%EC%A7%84%EC%8B%A4\_%EA%B3%B5%EA%B8%89%EC%9B%90)

React State의 조건:

1. 변경돼야 함. 변경되지 않는 건 state로 다룰 가치가 없다.
2. 부모 컴포넌트가 props를 통해 전달한다면 state가 아님.
3. 다른 state나 props를 이용해 계산 가능하다면 state가 아님.

다루는 상태가 너무 많으면 복잡함. TypeScript를 잘 쓰면 직접 관리하는 상태의 수를 줄여줄 수 있다.

그렇다면 그 상태를 누가 관리해야 할까? 더 정확히는, 상태를 누가 소유해야 할까?

(React만 쓴다면) 해당 상태에 의존적인 컴포넌트를 모두 포함하는 컴포넌트가 상태를 소유해야 함.

* [“Lifting State Up”](https://ko.reactjs.org/docs/lifting-state-up.html)
* [Sharing State Between Components](https://beta.reactjs.org/learn/sharing-state-between-components)

### Inverse Data Flow

하위 컴포넌트의 props로 함수를 전달. 흔히 콜백 함수라고 부름.

TypeScript(정확히는 JavaScript)는 함수가 일급(first-class) 객체다. 즉, 어떤 함수를 다른 함수에 인자로 넘겨주거나, 어떤 함수를 리턴값으로 사용할 수 있다. 익명 함수, 클로저 등과 함께 사용하면 시너지가 큼.

* [일급 함수](https://developer.mozilla.org/ko/docs/Glossary/First-class\_Function)
* 다시 [“Lifting State Up”](https://ko.reactjs.org/docs/lifting-state-up.html)
