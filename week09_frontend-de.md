# Week09\_FrontEnd DE

## 용어

* Product: 상품
* Summary: 상품에 대한 요약정보
* Detail : 상품에 대한 상세 정보
* Image: 상품 이미지
* Option:  상품에대한 상세 정보
  * OptionItem: 옵션  값 (ex. 색상 옵션인 경우 Blue, Black과 같은 값)
* Category: 상품 분류
* Cart: 장바구니
  *   LineItem: 장바구니에 담긴 것 (상품, 옵션, 수량 등을 하나의 단위로 다룸)

      여기서도 Option과 OptionItem을 사용

      Product와 다른 구성 -> Product / Order 접두어 활용 (시스템을 근본적으로 분리하는 것이 더 좋음)
* Order: 주문
* &#x20;LineItem 활용
* User: 사용자(고객)

\


## 기능

1. 상품 목록 확인
2. 상품 상세 정보 확인
3. 장바구니에 상품 담기
4. 주문하기 -> 배송지 입력/결제
5. 주문 목록 확인
6. 주문 상세 확인
7. 로그인
8. 회원가입

* 1\~3은 필수
* 4\~8은 비즈니스에 따라 선택사항일 수 있음

\


## 화면

1. 홈페이지 : `/`
2. 상품 목록 페이지 : `/products`
3. 상품 상세 페이지 : `/products/{id}`
4. 장바구니 페이지 : `/cart`
5. 주문 페이지 : `/order`
6. 주문 완료 페이지 : `/order/complete`
7. 주문 목록 페이지 : `/orders`
8. 주문 상세 페이지 : `/orders/{id}`
9. 로그인 페이지 : `/login`
10. 회원 가입 페이지 : `/signup`
11. 회원 가입 완료 페이지 : `/signup/complete`

\


## 사용하는 라이브러리

1. [React Router](https://github.com/remix-run/react-router)
2. [styled-components](https://github.com/styled-components/styled-components)
3. [styled-reset](https://github.com/zacanger/styled-reset)
4. [usehooks-ts](https://github.com/juliencrn/usehooks-ts)
5. [Axios](https://github.com/axios/axios): REST API 사용을 위한 HTTP 클라이언트.
6. [tsyringe](https://github.com/microsoft/tsyringe)
7. [reflect-metadata](https://github.com/rbuckton/reflect-metadata)
8. [usestore-ts](https://github.com/seed2whale/usestore-ts)
9. [jest-dom](https://github.com/testing-library/jest-dom): React Testing Library에서 활용할 수 있는 DOM 확인용 Matcher 모음.
10. [MSW](https://github.com/mswjs/msw)

### E2E Test

CodeceptJS로 E2E 테스트 케이스를 작성하고, 기능 테스트를 모두 통과시킨다\


### Styles

Styles-components를 사용하기 위해 defaultTheme과 GlobalStyles를 준비

### Routes

React Router로 여러 페이지를 표현

\- routes와 Layout 준비

### Test Helper

테스트코드에서 styled-components의 Theme과 React Router의 Link 등을 사용할 때 문제가 발생하지 않도록

React Testing Library의 render를 한번 감싼 테스트용 헬퍼 함수 준비

### Types

기본적인 타읻ㅂ 준비

특별한 경우가 아니라면 사전에 준비한 용어집과 REST API 스펙에 맞춘다

```typescript
export type Category = {
  id: string;
  name: string;
}


export type Image = {
  url: string;
}


export type ProductSummary = {
  id: string;
  category: Category;
  thumbnail: Image;
  name: string;
  price: number;
}


export type ProductOptionItem = {
  id: string;
  name: string;
};


export type ProductOption = {
  id: string;
  name: string;
  items: ProductOptionItem[];
};


export type ProductDetail = {
  id: string;
  category: Category;
  images: Image[];
  name: string;
  price: number;
  options: ProductOption[];
  description: string;
}


export type OrderOptionItem = {
  name: string;
};


export type OrderOption = {
  name: string;
  item: OrderOptionItem;
};


export type LineItem = {
  id: string;
  product: {
    id: string;
    name: string;
  };
  options: OrderOption[];
  unitPrice: number;
  quantity: number;
  totalPrice: number;
}


export type Cart = {
  lineItems: LineItem[];
  totalPrice: number;
}
```

### MSW 세팅

REST API 스펙에 맞춘 MSW 핸들러

```typescript
import { rest } from 'msw';
import { ProductSummary } from '../types';
import fixtures from '../../fixtures';

const BASE_URL = 'https://shop-demo-api-01.fly.dev';

const productSummaries: ProductSummary[] = fixtures.products
  .map((product) => ({
    id: product.id,
    category: product.category,
    thumbnail: { url: product.images[0].url },
    name: product.name,
    price: product.price,
  }));


const handlers = [
  rest.get(`${BASE_URL}/categories`, (req, res, ctx) => (
    res(ctx.json({ categories: fixtures.categories }))
  )),
  rest.get(`${BASE_URL}/products`, (req, res, ctx) => (
    res(ctx.json({ products: productSummaries }))
  )),
  rest.get(`${BASE_URL}/products/:id`, (req, res, ctx) => {
    const product = fixtures.products.find((i) => i.id === req.params.id);
    if (!product) {
      return res(ctx.status(404));
    }
    return res(ctx.json(product));
  }),
  rest.get(`${BASE_URL}/cart`, (req, res, ctx) => (
    res(ctx.json(fixtures.cart))
  )),
  rest.post(`${BASE_URL}/cart/line-items`, (req, res, ctx) => (
    res(ctx.status(201))
  )),
];


export default handlers;
```

## 상품목록

API로부터 상품 목록을 얻어서 화면을 만든다

\- 상품 목록 얻기 : ProductListPage에서 useFetchProduct Hook을 실현해 구현

\- 상품목록 보여주기 : Products 컴포넌트로 구현

페이지에 진입했을 떄, 데이터를 불러오기 위함(책임을 확실히 하고 역할을 나눔)

\
