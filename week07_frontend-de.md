# Week07\_FrontEnd DE

## Routing

\- 웹 사이트를 만들 때, 하나의 웹페이지를 하나의 컴포넌트로 만들고 URL에 따라 컴포넌트를 다르게 보여주도록 구현한다.

```typescript
function App() {
	const { pathname } = window.location;	// localhost로 접속했을 때, '/' 뒤에 붙어오는 값
	
	return (
		<div>
			<Header />
			<main>
				{pathname === '/' && <HomePage />}
				{pathname === '/about' && <AboutPage />}
			</main>
			<Footer />
		</div>
	);
}
```

\- (window.)location.hash : 인코딩 url값

&#x20; (- p 안에 div는 넣을 수 없다)

#### React.FC( React의 Function Component )

```typescript
const pages = {
    // 주소: 컴포넌트
    ‘/‘ : HomePage,
    ‘/about’ : AboutPage,
};

export default function App() {
    const path = window.location.pathname;
    const Page = Reflect.get(pages, path) || HomePage; // url에 아무것도 안 붙으면 홈페이지로 이동
    return (
        <div>
            <Header />
            <main> 
                //{pathname === '/' && <HomePage />} {pathname === '/about' && <AboutPage />} 
                <Page />
            </main> 
            <Footer /> 
        </div>
    );
}
```



### React Router

&#x20;: 테스트할 때, 주소에 따라 바꾼다고 할 때 실제로 window는 없다. 이 때, 윈도우를 추상화하는 도구를 활용

### Routes

1. 설치

```bash
npm i react-router-dom
```

개발환경에서만 사용하는 게 아니므로 -D는 붙이지 않는다

(읽어오는게 느려 vscode를 재실행해야할 수 있다)

1.  Route 컴포넌트

    * 공식문서를 참조해 props를 찾아 넣는다.
    * 공식문서는 코드 내의 도움말이나 홈페이지 참조
    * browser router가 필요함; 라우터를 관리해줄 수 있는 관리 컴포넌트가 필요

    > 위에서 라우터라는걸 관리해주고, 거기서 브라우저에서 라우터를 사용하고 있다. 윈도우에서는 브라우저만 있고, 테스트 환경에서는 테스트 만을 위한게 따로 있다.

    —> BrowserRouter가 App 컴포넌트를 감싸야함

    ```html
    <BrowserRouter>
    	<App />
    </BrowserRouter>

    ```

    테스트 환경에서는 MemoryRouter를 사용

### Router React Router&#x20;

\-6.4 버전부터 지원

> 라우터 객체를 만들어 사용하는 방법

1. url의 ‘/’ 다음에 오는 값(path)을 보여줄 페이지(element)와 연결해 배열로 정의(routes)
2. createBrowserRouter(routes); 로 router를 생성
3. RouterProvider에 props로 추가

분기하는 시점이 `<Page />`에서, 통째로 바뀜

이것을 Layout으로 바꿔줌

→ Layout 자체를 컴포넌트로 잡아줄 것

Layout을 적용한다는 것을 잡아주는 두 가지가 필요&#x20;

> 1. routes를 계층형으로 잡아서 자녀(children )로 route 내용을 넣어준다
> 2. Layout에 path에 따른 페이지 레이아웃 처리를 정의하도록 한다 → Outlet 컴포넌트 사용

이렇게 처리하는 경우, App 컴포넌트에 대한 테스트를 진행하기가 어렵다. 그래서 routes.tsx에 대한 테스트로 대체한다(routes.test.tsx)

또 다른 문제점으로, path를 바꿀 때마다 전체를 다시 랜더링한다(SPA를 따르지 않는 문제)

\


## Navigation

브라우저는 페이지를 이동할 때마다 새로 로딩

HTML5부터는 History.pushState API가 이를 해결

(그 전엔 path에 #을 붙이거나 #!을 붙여 새로 로딩하는것을 막았음)

링크 자체를 가로채서 리로딩을 막고, 원하는 페이지로 강제 이동

onClick이벤트를 handleClick로 잡아줌

event.preventDefault()로 기본적으로 처리해야하는 이벤트를 막아줌

history.pushState(현재 상태, 타이틀, 주소);

\- 상태는 아무 객체나 가능. 현재 값을 저장하고싶을 떄

\- 주소(url) : 이동하고자 하는 페이지 주소(path)

—> 현재는 웹브라우저가 자동으로 url 주소를 변경해준다(ex. 블로그에서 다른 글로 이동하면 상단 url의 path가 자동으로 바뀜)

### Link

\- history.pushState()를 자동으로 해주는 API(리액트 라우터에서 제공)

\- 페이지 넘어갈 때 주소, 페이지 변경을 제공

\- 사용방법 : Link 컴포넌트 사용, props를 to(이동할 페이지 주소)만 잡아주면 됨

\- 단순함

### NavLink

\- 링크로 페이지가 이동하는 경우, 해당 컴포넌트에 class=“active” 옵션이 붙음

\- 이걸로 디자인을 변경하는 등의 처리를 해줄 수 있음

### Navigate

\- 무조건 지정한 페이지로 이동(redirection)

\- 테스트에서 “ReferenceError: Request is not defined”에러가 나면 whatwg-fetch를 import해서 해결

### useNavigate

가장 많이 사용

훅으로 사용할 수 있음

\


\
