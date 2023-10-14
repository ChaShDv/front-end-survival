---
description: Express, Fetch API & CORS, React의 Hook, useRef & Custom Hook, usehooks-ts
---

# Week04\_FrontEnd DE

## Express란?

Node.js를 위한 빠르고 개방적인 간결한 (서버)웹 프레임워크



## 간단한 서버 앱 npm 패키지 세팅

<pre class="language-bash"><code class="lang-bash"><strong># 1. Node.js 설치 후 애플리케이션을 보관할 디렉토리 설정
</strong>mkdir week04
cd mkdir week04

# 2. .gitignore 파일 준비
touch .gitignore
echo "/node_modules/" > .gitignore

# 3. 패키지 초기화
npm init -y

# 4. typescript 설치
npm i -D typescript
npx tsc --init

# 5. ESLint 설치
npm i -D eslint
npx eslint --init
    # 자동으로 고치기
    # package.json에서
    "script" : {
        "lint" : "eslint --fix ."
    }
    #추가 후
    npm run lint

# 6. ts-node 설치
npm i -D ts-node
    # package.json에서
    "script" : {
        "start" : "ts-node app.ts"
    }
    #추가 후
    npm start

# 7. Express 설치
npm i express
npm i -D @types/express

</code></pre>



## app.ts

```typescript
import express from 'express';

const port = 3000;

const app = express();

//추가(요청)
app.get('/', (req, res) => {
	res.send('Hello, world!');
});

// 클라이언트에서 받아볼 때 사용
app.listen(port, () => {
	console.log(`Server running at http://localhost:${port}`);
});
```

ts-node로 실행

`$ npx ts-node app.ts`



코드를 수정 후 재실행해야함 -> 번거로움을 줄이기 위해  codemon 사용

`$ npm i -D nodemon`&#x20;

`$ npx nodemon app.ts`



## REST API

“필딩 제약 조건” 4가지 중, Resource와 HTTP Verb만 도입하는 수준으로 사용.

/write-post -> /posts : /뭔가(CRUD)를 한다

\=> CRUD에 대해 HTTP Method를 대입. Read는 Collection(복수)과 Item(Element)(단수)로 나뉨.

#### 기본 리소스 URL: /products

1. Read (Collection) → GET /products ⇒ 상품 목록 확인
2. Read (Item) → GET /products/{id} ⇒ 특정 상품 정보 확인
3. Create (Collection Pattern 활용) → POST /products ⇒ 상품 추가 **(JSON 정보 함께 전달)**
4. Update (Item) → PUT 또는 PATCH /products/{id} ⇒ 특정 상품 정보 변경 **(JSON 정보 함께 전달)**
5. Delete (Item) → DELETE /products/{id} ⇒ 특정 상품 삭제









## useRef

컴포넌트의 생애주기 전체에 걸쳐서 유지되는 객체. 즉, 컴포넌트가 없어질 때까지 동일한 객체가 유지된다.

객체 자체가 값은 아니고, 값을 참조하기 위한 객체. 즉, 언제든지 값을 변경할 수 있다.

```typescript
# 예시 코드
const compObj = {
    value: number;
}
```

\=> 참조하는 compObj의 주소를 변경할 수는 없지만, compObj의 속성인 value는 compObj를 통해 값에 접근 및 업데이트가 가능



상태(state)가 변경되면 해당 컴포넌트와 하위 컴포넌트를 다시 렌더링하지만, 레퍼런스 객체의 현재 값(current)이 바뀌더라도 렌더링에 영향을 주지 않는다.

```typescript
const refContainer = useRef(initialValue);
// initialValue 값이 refContainer의 current 속성값에 전달
```



## Custom Hook

정의 : 로직을 재사용하기 위한 제일 쉬운 방법.

사용자 정의 Hook 만들기 : 평범하게 Extract Function을 수행하면 된다. 컴포넌트가 대문자로 시작하는 PascalCase로 이름을 붙인다면, Hook은 “use”로 시작하는 camelCase로 이름을 붙이면 된다.







##







