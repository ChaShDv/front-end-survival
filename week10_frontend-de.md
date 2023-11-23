# Week10\_FrontEnd DE

### 1. 로그인

주문 관리를 위한 ‘계정’ 도입

사용자의 user name 또는 email로 identity를 파악하기 위함이다. 이때 password는 identity를 인증하기 위한 수단이다. 인증을 한번 완료하면 일정 기간 또는 시간 동안 중복된 인증을 피하기 위해 ‘토큰’을 발행한다.

[**프론트엔드 입장에서 로그인은 사용자의 username(여기서는 email)과 password를 백엔드로 전송해서 Access Token(여기서는 JWT를 사용)를 얻는 과정이다. 이렇게 얻은 Access Token을 관리하는 방법은 여러 가지가 있지만, 여기서는 usehooks-ts의 useLocalStorage를 사용해서 전역적으로 동기화한다**](#user-content-fn-1)[^1]**.**\


헤더에서 카테고리를 요청할 때, 그 부분을 테스터가 넘어가는 현상을 방지하기 위해 routes.test.tsx에 아래 코드를 추가한다.

```typescript
await waitFor(() => {
    screen.getByText(/Category #1/);
});
```

#### 컴포넌트 개발 시,

\- 속성 값을 undefined로 잡아주면, 속성을 생략할 수 있다.

\
(LoginPage와 관련) 기본 Props 값을 지원하는 ESlint 룰을 추가한다

```javascript
'react/require-default-props': [2, { functions: 'defaultArguments' }],Java
```



Submit type의 경우, 이벤트가 실행되면 자동으로 다른 페이지로 넘어감&#x20;

—> 방지를 위해 `event.preventDefault();` 추가



tsyringe로 expect()에 container로 처리하는게 편하기 때문에, 여기서 container와 구분하기 위해 tsyringe의 container를 import할 때, iocContainer로 받아온다.

```typescript
import { container as iocContainer } from "tsyringe";
```

### API 호출할 때 AccessToken 사용

`ApiService`에 setAccessToken 메서드를 추가해서 API 호출할 때 Access Token을 전달하게 한다. Axios 인스턴스를 다시 만들어 주면 다른 메서드를 수정하지 않아도 된다.

### Access Token 확인

사이트에 접속하거나 새로 고침 등을 했을 땐 Access Token을 확인하고, 사용자 정보를 얻는 작업이 필요하다. 만약 회원 정보를 얻는 API가 실패하면 Access Token에 문제가 있는 상황이니 로그아웃시킨다.



### 2. 로그아웃

단순히 AccessToken 값을 지워주면 로그아웃 처리가 된다.

```typescript
const handleClickLogout = async () => {
    await apiService.logout();
    setAccessToken('');
    navigate('/');
};
```



### 3. 회원가입

이메일/UserName과 Password로 AccessToken을 발급받는다.

이때 Name은 토큰 발급에 사용되기보다는 서비스를 위한 목적으로 (필수로)받는다.\


### 4. 주문목록 &주문상세

주문목록/주문상세는 사실상 상품목록/상품상세와 유사하다.

Cart의 LineItem은 주문의 LineItem에서 가져왔다. 그럼에도 비즈니스적으로 둘을 분리해서 관리하는 것이 좋다.\


\


[^1]: 
