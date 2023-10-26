# Week06\_ForntEnd DE

## External Store

### 1. 관심사의 분리

: 컴포넌트를 분리면서, 각 컴포넌트의 기능과 목적을 분리하여 관리

\- 기능의 관점으로 분리

\- 설계의 관점으로 분리 : 사용자와 얼마나 가까운가에 따라 분리(**Layered Architecture**)

### 2. Flux Architecture

&#x20;: 2-way binding을 썼을 때 생길 수 있는 Model-View의 복잡한 관계(전통적인 MVC에선 이런 상황을 지양한다)를 겨냥해 명확히 “unidirectional data flow”를 강조

1. Action → 이벤트/메시지 같은 객체.
2. Dispatcher → (여러) Store로 Action을 전달. 메시지 브로커와 유사하다.
3. Store (여러 개) → 받은 Action에 따라 상태를 변경. 상태 변경을 알림.
4. View → Store의 상태를 반영.

#### Redux : 단일 Store 사용하여 좀더 단순한 모델 제안

1. Action
2. Store → dispatch를 통해 Action을 받고, 사용자가 정의한 reducer를 통해 State를 변경한다.
3. View → State를 반영.

#### 3단계 프로세스와 대략적으로 매핑

1. Input -> Action + dispatch
2. Process → reducer
3. Output → View(React)

## External Store

React는 컴포넌트의 상태가 바뀌면, 해당 컴포넌트와 그 하위 컴포넌트를 랜더링한다.

```typescript
const [, setState] = useState({});
const forceUpdate = () => setState({});typist

// Custom Hooks
function useForceUpdate() {
    const [, setState] = useState({});
    return useCallback(() => setState({}), []);
}
```

\==> UI와 비즈니스 로직을 분리하여 유지보수를 효율적으로 하기 위해 사용할 수 있다.

\==> 자주 바뀌는 UI 요소에 대한 테스트 대신, 오래 유지되는(바뀌면 치명적인) 비즈니스 로직에 대한 테스트 코드를 작성해 유지보수에 도움이 되는 테스트 코드를 치밀하게 작성할 수 있다.



## TSyringe

TypeScript용 DI 도구(IoC Container)

External Store를 관리하는데 활용

React 컴포넌트 입장에서는 “전역”처럼 간주

“Prop Drilling” 문제를 우아하게 해결할 수 있는 방법 중 하나(React로 한정하면 Context도 쓸 수 있다)

#### 의존성 설치

```bash
npm i tsyringe reflect-metadata
```

#### 사용 방법

```typescript
//src/main.tsx 파일과 src/setupTests.ts 파일에서 reflect-metadata 임포트.
import 'reflect-metadata';


//싱글톤으로 관리할 CounterStore 클래스 준비
import { singleton } from 'tsyringe';

@singleton()
class CounterStore {
    // 내용
}


// 싱글톤 CounterStore 객체를 사용
import { container } from 'tsyringe';

const counterStore = container.resolve(CounterStore);


//테스트에서 TSyringe에서 관리하는 객체를 초기화
container.clearInstances();
```

## Redux 따라하기

<mark style="color:red;">src/stores/BaseStore.ts</mark>

```typescript
export type Action<Payload> = {
	type: string;
	payload?: Payload;
}

type Reducer<State, Payload> = (state: State, action: Action<Payload>) => State;

type Reducers<State> = Record<string, Reducer<State, any>>;

type Listener = () => void;

export default class BaseStore<State> {
	state: State;

	reducer: Reducer<State, any>;

	listeners = new Set<Listener>();
	
	constructor(initialState: State, reducers: Reducers<State>) {
		this.state = initialState;

		this.reducer = (state: State, action: Action<any>): State => {
			const reducer = Reflect.get(reducers, action.type);			
			if (!reducer) {
				return state;
			}
			return reducer(state, action);
		};
	}
	
	dispatch<T>(action: Action<T>) {
		this.state = this.reducer(this.state, action);
		this.listeners.forEach((listener) => listener());
	}
	
	addListener(listener: Listener) {
		this.listeners.add(listener);
	}
	
	removeListener(listener: Listener) {
		this.listeners.delete(listener);
	}
}
```

src/stores/Store.ts 파일

```typescript
import { singleton } from 'tsyringe';

import BaseStore, { Action } from './BaseStore';

const initialState = {
	count: 0,
	name: 'Tester',
};

export type State = typeof initialState;

function increase(state: State, action: Action<number>) {
	return {
		...state,
		count: state.count + (action.payload ?? 1),
	};
}

function decrease(state: State) {
	return {
		...state,
		count: state.count - 1,
	};
}

const reducers = {
	increase,
	decrease,
};

@singleton()
export default class Store extends BaseStore<State> {
	constructor() {
		super(initialState, reducers);
	}
}
```

src/hooks/useDispatch.ts

```typescript
import { container } from 'tsyringe';

import { Action } from '../stores/BaseStore';

import Store from '../stores/Store';

export default function useDispatch<Payload>() {
	const store = container.resolve(Store);
	return (action: Action<Payload>) => store.dispatch(action);
}
```

src/hooks/useSelector.ts 파일

```typescript
import { container } from 'tsyringe';

import { useEffect, useState } from 'react';

import Store, { State } from '../stores/Store';

import useForceUpdate from './useForceUpdate';

type Selector<T> = (state: State) => T;

export default function useSelector<T>(selector: Selector<T>): T {
	const store = container.resolve(Store);
	
	const [state, setState] = useState<T>(selector(store.state));
	
	const forceUpdate = useForceUpdate();
	
	useEffect(() => {
		const update = () => {
			const newState = selector(store.state);
			// TODO: T가 object일 때 처리 필요함.
			if (newState !== state) {
				setState(newState);
				forceUpdate();
			}
		};
	
		store.addListener(update);
		return () => store.removeListener(update);
	}, [store, forceUpdate]);

	return state;
}
```

Dispatch와 Selector 사용.

```typescript
const dispatch = useDispatch();
const count = useSelector((state) => state.count);

dispatch({ type: 'increase' });
dispatch({ type: 'decrease' });

dispatch({ type: 'increase', payload: 10 });
```



### Action을 메서드로 처리

src/stores/ObjectStore.ts 파일

```typescript
type Listener = () => void;

export default class ObjectStore {
	private listeners = new Set<Listener>();

	addListener(listener: Listener) {
		this.listeners.add(listener);
	}
	
	removeListener(listener: Listener) {
		this.listeners.delete(listener);
	}
	
	protected publish() {
		this.listeners.forEach((listener) => listener());
	}
}
```

src/stores/CounterStore.ts 파일

```typescript
import { singleton } from 'tsyringe';

import ObjectStore from './ObjectStore';

@singleton()
export default class CounterStore extends ObjectStore {
	count = 0;
	
	increase(step = 1) {
		this.count += step;
		this.publish();
	}
	
	decrease() {
		this.count -= 1;
		this.publish();
	}
}
```

src/hooks/useObjectStore.ts 파일

```typescript
import { useEffect } from 'react';

import useForceUpdate from './useForceUpdate';

import ObjectStore from '../stores/ObjectStore';

export default function useObjectStore<T extends ObjectStore>(store: T) {
	const forceUpdate = useForceUpdate();
	
	useEffect(() => {
		store.addListener(forceUpdate);
		return () => store.removeListener(forceUpdate);
	}, [store]);

	return store;
}
```

src/hooks/useCounterStore.ts 파일

```typescript
import { container } from 'tsyringe';

import useObjectStore from './useObjectStore';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
	const store = container.resolve(CounterStore);

	return useObjectStore(store);
}
```



## usestore-ts

Store 작성

```typescript
import { singleton } from 'tsyringe';

import { Store, Action } from 'usestore-ts';

@singleton()
@Store()
export default class CounterStore {
	count = 0;
	
	@Action()
	increase(step = 1) {
		this.count += step;
	}
	
	@Action()
	decrease() {
		this.count -= 1;
	}
}
```

Custom Hook

```typescript
import { container } from 'tsyringe';

import { useStore } from 'usestore-ts';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
	const store = container.resolve(CounterStore);
	return useStore(store);
}
```

??? 비동기 함수에 @Action을 붙이면 다르게 작동할 수 있다는 점에 주의! 별도의 액션을 만들면 신경 쓸 부분이 줄어든다.

```typescript
@singleton()
@Store()
class PostStore {
	posts: Post[] = [];
	
	async fetchPosts() {
		this.startLoading();
	
		const posts = await fetchPosts();

		this.completeLoading(posts);
	}
	
	@Action()
	startLoading() {
		this.posts = [];
	}
	
	@Action()
	completeLoading(posts: Post[]) {
		this.posts = posts;
	}
}
```









\
