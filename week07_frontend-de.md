# Week07\_FrontEnd DE

\#Routing

\- 웹사이트를 만들 때, 하나의 웹페이지를 하나의 컴포넌트로 만들고 url에 따라 컴포넌트를 다르게 보여줌

\- Window.local —> localhost로 접속했을 떄

\- (window.)location.hash : 인코딩 url값

\- location.pathname :&#x20;

(- p 안에 div는 넣을 수 없다)

\###React.FC( React의 Function Component )

\`\`\`

const pages = {

// 주소: 컴포넌트

‘/‘ : HomePage,

‘/about’ : AboutPage,

};

export default function App() {

const path = window.location.pathname;

const Page = Reflect.get(pages, path) || HomePage; // url에 아무것도 안 붙으면 홈페이지로 이동

return (

\<div>

\<Header />

\<main>&#x20;

//{pathname === '/' && \<HomePage />} {pathname === '/about' && \<AboutPage />}&#x20;

\<Page />

\</main>&#x20;

\<Footer />&#x20;

\</div>

);

}

\`\`\`

\


\


\##React Router

&#x20;: 테스트할 때, 주소에 따라 바꾼다고 할 때 실제로 window는 없다. 이 때, 윈도우를 추상화하는 도구를 활용

\


\#Routes

\


\#Router

\


\#Navigation

브라우저는 페이지를 이동할 때마다 새로 로딩한다

HTML5부터는 History.pushState API가 이를 해결

(그 전엔 path에 #을 붙이거나 #!을 붙여 새로 로딩하는것을 막았음)

링크 자체를 가로채서 리로딩을 막고, 원하는 페이지로 강제 이동

onClick이벤트를 handleClick로 잡아줌

event.preventDefault()로 기본적으로 처리해야하는 이벤트를 막아줌

history.pushState(현재 상태, 타이틀, 주소);

\- 상태는 아무 객체나 가능. 현재 값을 저장하고싶을 떄

\- 주소(url) : 이동하고자 하는 페이지 주소(path)

—> 현재는 웹브라우저가 자동으로 url 주소를 변경해준다(ex. 블로그에서 다른 글로 이동하면 상단 url의 path가 자동으로 바뀜)

\##Link

\- history.pushState()를 자동으로 해주는 API(리액트 라우터에서 제공)

\- 페이지 넘어갈 때 주소, 페이지 변경을 제공

\- 사용방법 : Link 컴포넌트 사용, props를 to(이동할 페이지 주소)만 잡아주면 됨

\- 단순함

\##NavLink

\- 링크로 페이지가 이동하는 경우, 해당 컴포넌트에 class=“active” 옵션이 붙음

\- 이걸로 디자인을 변경하는 등의 처리를 해줄 수 있음

\##Navigate

\- 무조건 지정한 페이지로 이동(redirection)

\- 테스트에서 “ReferenceError: Request is not defined”에러가 나면 whatwg-fetch를 import해서 해결

\##useNavigate

가장 많이 사용

훅으로 사용할 수 있음

\


\
