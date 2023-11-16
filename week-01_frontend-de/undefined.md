# 개발환경 세팅

Keyword-focused summary

1.  Node.js

    JavaScript
2.  NPM(Node Package Manager)

    FNM(Fast Node Manager) : NPM보다 더 빠른 노드 패키지 매니저

    1. package.json / package-lock.json
       1. npx
       2. node\_modules
3. ES Modules vs CommonJS

#### JavaScript(Node.js) 설치

* Deno 로 간단하게 설치 가능(도전적인 개발문화에서 많이 사용)
* 도구는 더 좋은 것이 나옴. 어떤 것을 선택할 것인지 안목을 기르는 것이 중요
*   FNM(Fast Node Manager)

    * 노드 설치 및 실행을 더 빠르게 하는 도구
    * 최신버전 : 사이트에서 LTS를 설치하는 것이 좋다(**LTS라고 해도 항상 최신버전이 좋은 것은 아님**)
      * LTS (Long Term Support) : 장기적으로 서포드되어 안정적이고 신뢰도가 높음
      * 최신버전 : 안정적이지는 않지만 가장 최신 버전

    디렉토리 생성 & 이동 -> VS code  실행 ->&#x20;
* npm 패키지 준비
  * 옵션을 하나씩 설정 가능
    * 또는 npm init -y ( 모든 옵션을 디폴트/yes )
    * package.json 에 선택한 옵션 확인가능 ; version, name, ...&#x20;
    * <mark style="background-color:yellow;">package.json은 설치 중에 수정하게 되면 충돌이 일어날 수 있으므로 주의</mark>
    *   <mark style="color:red;">.gitignore 파일 작성</mark>

        touch .gitignore로 생성

        /node\_modules/[^1] : 루트 폴더에 있는

        node\_modules/ : 폴더임을 강조

        node\_modules

        \=> 세가지 모두 가능

        * 구글에 " gitignore > node 로 생성
        * 레포지토리에서 받아오는 경우, 거기에 포함된 gitignore 사용해도 좋음
    * parcel-cache, node\_modules, dist < 필수
* TypeScript 설정
  * npm i -D typescript ( = npm install --save-dev typescript)
    * dependencies : 프로그램에 사용할 것
    * devDependencies : 개발환경에서 사용할 것( ex.툴) ->[ <mark style="background-color:orange;">-D</mark>](#user-content-fn-2)[^2] 옵션 추가
  * npx ts
    * npx :  내가 설치하지 않은 패키지도 패키지 매니저에서 다운받아 실행
    * tsconfig.json 에서 "jsx" : "react-jsx" 를 설정(수정)
  *   ESLint

      > \# 설치
      >
      > npm i -D eslint

      > npx eslint --init
      >
      > \#각 질문에 대한 옵션 선택

      * .eslintignore 설치 필요
        * touch .eslintignore
        * node\_modules, parcel-cache, dist
  *   JEST 설치( swc ) <- 테스팅 도

      * npm i -D jest @types/jest @swc/core @swc/jest \jest-environment-jsdom
        * swc/jest : transform(변환하는 속성). 자바스크립트 -> 타입스크립트
      * jest.config.js&#x20;

      > \# .bin 폴더 아래 파일들 코드를 정리해줌
      >
      > npx eslint --fix .
  * Parcel 설치
    * npm i -D parcel
    * 웹서버 등 파일을 번들로 묶어서 실행시켜줌
    * 기존 브라우저에는 제공하지 않는 기능이였음



[^1]: 

[^2]: 
