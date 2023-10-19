---
description: TDD
---

# Week05\_FrontEnd DE

1.  TDD(Test Driven Development : 테스트 주도 개발)

    : 테스트 코드를 먼저 작성하는, 즉 구현보다 인터페이스와 스펙을 먼저 정의함으로써 개발을 진행하는 방식.

    **TDD Cycle**

    1. Red → 실패하는 테스트 코드를 작성. 인터페이스와 스펙에 집중한다.
    2. Green → 재빨리 테스트를 통과시킨다. 올바른 방법이 아니어도 괜찮다.
    3. Refactor → 리팩터링을 통해 코드를 올바르게 만든다. TDD에서 가장 중요한 부분이지만, 간과될 때가 많다.

    TDD 핵심 포인트

    → 최소한의 단위/단계(반복을 위함)를 찾고, 코드로부터 피드백을 얻는 것이 중요(어려움)

    Green 단계가 어렵다면 Red 단계로 돌아가 더 작고 쉬운 문제 정의 + Refactor를 위해 의도를 드러내고 중복을 찾아 제거하는 연습

    → 익숙하지 않다면 TDD 뿐만 아니라 일반적인 개발이나 클린코드는 거의 불가능

    TDD 핵심 목표 : 제대로 작동하는 클린코드
2.  React Testing Library

    : React 컴포넌트를 사용자 입장에 가깝게 테스트할 수 있는 도구.

    컴포넌트를 사용하는 코드(테스트 코드)를 통해 컴포넌트의 인터페이스를 점검(누락되거나 더 좋은 표현법을 찾도록 유도)
3.  MSW(Mock Service Worker)

    : 코드 레벨이 아니라 네트워크 레벨에서 가짜 구현. 오프라인 작업 등을 지원하기 위한 서비스 워커의 기능을 유용히 활용한 것.

    웹 브라우저도 지원하기 때문에, API 스펙은 나왔지만 아직 구현되지 않은 경우 임시로 사용 가능

    테스트 코드도 지원하면서 겸사겸사 웹 브라우저를 지원하는 용도로는 나쁘지 않은 선택
4.  Playwright

    : 웹 브라우저 기반 E2E 테스트 자동화 도구. Headless Chrome을 기반으로 한 Puppeteer를 계승하면서, 더 많은 웹 브라우저를 지원한다.

    Playwright 패키지 설치

    ```bash
    npm i -D @playwright/test eslint-plugin-playwright
    ```

    playwright.config.ts 파일

    ```jsx
    import { PlaywrightTestConfig } from '@playwright/test';

    const config: PlaywrightTestConfig = {
    	testDir: './tests',
    	retries: 0,
    	use: {
    		baseURL: '<http://localhost:8080>',
    		headless: !!process.env.CI,
    		screenshot: 'only-on-failure',
    	},
    };

    export default config;
    ```

    tests/.eslintrc.js 파일

    ```jsx
    module.exports = {
    	env: {
    		jest: false,
    	},
    	extends: ['plugin:playwright/playwright-test'],
    	rules: {
    		'import/no-extraneous-dependencies': 'off',
    	},
    };
    ```
