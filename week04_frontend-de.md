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
        "lint" : "eslint -fix ."
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

app.get('/', (req, res) => {
	res.send('Hello, world!');
});

app.listen(port, () => {
	console.log(`Server running at http://localhost:${port}`);
});
```
