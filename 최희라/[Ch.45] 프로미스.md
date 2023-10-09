# 프로미스

비동기 처리를 위한 패턴으로 ES6에 도입됐다. 전통적인 콜백 패턴의 단점을 보완하고 비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.

## 비동기 처리를 위한 콜백 패턴의 단점

### 콜백 헬

**비동기 함수 내의 비동기 코드는 비동기 함수가 종료된 후 완료**되기 때문에 처리 결과를 외부로 반환하거나 상위 스코프 변수에 할당하면 기대와 다르게 동작한다.

- 처리결과를 외부로 반환하는 경우
    
    비동기 코드가 실행되는 시점에는 이미 비동기 함수가 종료되었기 때문에 비동기 코드의 처리 결과가 외부로 반환되지 않는다.
    
- 처리 결과를 상위 스코프 변수에 할당하는 경우
    
    비동기 코드는 태스크 큐에 있다가 콜 스택에 있는 실행 컨텍스트가 모두 실행된 후에 실행된다. 그렇기 때문에 비동기 함수 실행 이후 상위 스코프 변수를 참조해도 응답 결과가 할당되기 전에 코드가 실행된다.
    

따라서 비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다. 이때 비동기 처리 결과로 또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생하는데, 이를 **콜백 헬(callback hell)** 이라 한다.

```javascript
get('/step1', a => {
	get('/step2/${a}', b => {
		get('/step3/${b}', c => {
			get('/step4/${c}', d => {
				console.log(d);
			});
		});
	});
});
```

### 에러 처리의 한계

```jsx
try {
	setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch(e) {
	console.error('캐치한 에러', e);
}
```

에러는 `catch` 코드 블럭에서 캐치되지 않는다. `setTimeout`의 콜백 함수를 호출하는 호출자(caller)가 `setTimeout` 함수가 아니기 때문이다. **에러는 호출자 방향으로 전파**되기 때문에 비동기 처리를 위한 콜백 패턴은 에러 처리가 곤란하다는 문제가 있다.

## 프로미스의 생성

- `Promise`는 표준 빌트인 객체로, `new` 연산자와 함께 `Promise` 생성자 함수를 호출하면 프로미스를 생성한다.
- 비동기 처리를 수행할 콜백 함수를 인수로 받는데, 이 함수는 `resolve`와 `reject` 함수를 인수로 받는다.
- 비동기 처리가 성공하면 `resolve` 함수를 호출하고, 비동기 처리가 실패하면 `reject` 함수를 호출한다.
    
    ```jsx
    const promise = new Promise((resolve, reject) => {
    	if (/* 비동기 처리 성공 */) {
    		resolve('result');
    	} else { /* 비동기 처리 실패 */
    		reject('failure reason');
    	}
    });
    ```
    
- 프로미스가 갖는 비동기 처리 상태 정보는 다음과 같으며, 프로미스 상태는 `resolve` 또는 `reject` 함수를 호출하는 것으로 결정된다.
    
    
    | 프로미스 상태 정보 | 의미 | 상태 변경 조건 |
    | --- | --- | --- |
    | `pending` | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
    | `fulfilled` | 비동기 처리가 수행된 상태 (성공) | `resolve` 함수 호출 |
    | `rejected` | 비동기 처리가 수행된 상태 (실패) | `reject` 함수 호출 |
- `pending`이 아닌 상태를 `settled` 상태라고 하며, `pending` 상태에서 `settled` 상태로 변할 수 있지만 `settled` 상태가 되면 다른 상태로 변할 수 없다.
- 비동기 처리 상태와 더불어 비동기 처리 결과도 상태로 갖는다.

⇒ 프로미스는 **비동기 처리 상태**와 **처리 결과**를 관리하는 객체다

.

## 프로미스의 후속 처리 메서드

- 후속 처리 메서드 `then`, `catch`, `finally`를 제공한다.
- 프로미스의 비동기 처리 상태가 변하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다.
- 후속 처리 메서드는 프로미스를 반환하며 비동기로 동작한다.

### `Promise.prototype.then`

두 개의 콜백 함수를 인수로 전달받는다.

- 첫 번째 콜백 함수는 프로미스가 `fulfilled` 상태가 되면 호출되며 프로미스의 비동기 처리 결과를 인수로 전달받는다.
- 두 번째 콜백 함수는 프로미스가 `rejected` 상태가 되면 호출되며 프로미스의 에러를 인수로 전달받는다.

```jsx
new Promise(resolve => resolve('fulfilled'))
	.then(v => console.log(v), e => console.error(e));  // fulfilled

new Promise((_, reject) => reject(new Error('rejected')))
	.then(v => console.log(v), e => console.error(e));  // Error: rejected
```

### `Promise.prototype.catch`

한 개의 콜백 함수를 인수로 전달받으며 이는 프로미스가 `rejected` 상태인 경우만 호출된다.

```jsx
new Promise((_, reject) => reject(new Error('rejected')))
	.catch(e => console.log(e));  // Error: rejected
```

`then(undefined, onRejected)`과 동일하게 동작한다.

### `Promise.prototype.finally`

프로미스의 성공 여부와 상관 없이 무조건 한 번 호출되며, 한 개의 콜백 함수를 인수로 전달받는다. 프로미스의 상태와 상관 없이 공통적으로 수행할 처리 내용이 있을 때 유용하다.

```jsx
new Promise(() => {})
	.finally(() => console.log('finally');  // finally
```

## 프로미스의 에러 처리

- 비동기 처리에서 발생한 에러는 `then` 메서드의 두 번째 콜백 함수와 `catch` 메서드를 사용해 처리할 수 있다.
- `then` 메서드의 두 번째 콜백 함수는 첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못하고 가독성이 좋지 않기 때문에 **`catch` 메서드 사용을 권장**한다.

## 프로미스 체이닝

프로미스의 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있으며, 이를 프로미스 체이닝(promise chaining)이라 한다.

- 후속 처리 메서드의 콜백 함수가 프로미스가 아닌 값을 반환하더라도 암묵적으로 `resolve` 또는 `reject`하여 프로미스 반환한다.
- 프로미스는 콜백 헬이 발생하지 않으나 콜백 패턴을 사용하므로 콜백 함수를 사용하지 않는 것은 아니다.
- 콜백 패턴은 가독성이 좋지 않기 때문에 ES8에서 도입된 `async`/`await`를 사용하여 후속 처리 메서드 없이 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현한다.

## 프로미스의 정적 메서드

### `Promise.resolve` / `Promise.reject`

각각 인수로 전달받은 값을 `resolve`, `reject`하는 프로미스를 생성한다.

```jsx
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log);  // [1, 2, 3]

// 위 코드는 아래 코드와 같이 동작한다.
const resolvedPromise = new Promise(resolve => resolve([1, 2, 3]);
resolvedPromise.then(console.log);  // [1, 2, 3]
```

### `Promise.all`

여러 개의 비동기 처리를 모두 병렬(parallel) 처리할 때 사용하며 프로미스를 요소로 갖는 이터러블을 인수로 전달받는다.

```jsx
const requestData1 = () => new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise(resolve => setTimeout(() => resolve(2), 2000));
const requestData3 = () => new Promise(resolve => setTimeout(() => resolve(3), 1000));

Promise.all([requestData1, requestData2, requestData3]);
	.then(console.log)  // [1, 2, 3] (약 3초 소요)
	.catch(console.error);
```

- 인수로 전달받은 배열의 프로미스가 모두 `fulfilled` 상태가 되면 종료하기 때문에 가장 늦게 `fulfilled` 상태가 되는 프로미스의 처리 시간보다 조금 더 긴 시간이 소요된다.
- 인수로 전달받은 배열 순서에 따라 처리 결과를 배열에 저장하여 그 배열을 `resolve`하는 새로운 프로미스를 반환하기 때문에 처리 순서가 보장된다.
- 인수로 전달받은 배열의 프로미스가 하나라도 `rejected` 상태가 되면 즉시 종료된다.
- 인수로 전달받은 이터러블의 요소가 프로미스가 아닌 경우 `Promise.resolve` 메서드를 통해 프로미스로 래핑한다.

### `Promise.race`

프로미스를 요소로 갖는 이터러블을 인수로 전달받는다. 해당 메서드는 가장 먼저 `fulfilled` 상태가 된 프로미스의 처리 결과를 `resolve`하는 새로운 프로미스를 반환한다.

```jsx
Promise.race([
	new Promise(resolve => setTimeout(() => resolve(1), 3000),
	new Promise(resolve => setTimeout(() => resolve(2), 2000),
	new Promise(resolve => setTimeout(() => resolve(3), 1000)
])
	.then(console.log)  // 3
	.catch(console.log);	
```

프로미스가 `rejected` 상태가 되면 `Promise.all` 메서드와 동일하게 처리된다.

### `Promise.allSettled` <img src="https://img.shields.io/badge/ES11-violet?style=flat"/>

프로미스를 요소로 갖는 이터러블을 인수로 전달받는다. 전달받은 프로미스가 모두 `settled` 상태가 되면 처리 결과를 배열로 반환한다.

```jsx
Promise.allSettled([
	new Promise(resolve => setTimeout(() => resolve(1), 2000)),
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error!)), 1000)
]).then(console.log);
/*
[
	{status: "fulfilled", value: 1},
	{status: "rejected", reason: Error: Error! at <anonymous>:3:54}
*/
```

## 마이크로태스크 큐

- 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아닌 마이크로태스크 큐에 저장된다.
- 태스크 큐와는 별도의 큐로, 콜백 함수나 이벤트 핸들러를 일시 저장한다는 점은 동일하지만 **태스크 큐보다 우선순위가 높다.**
- 아래 코드의 실행 결과는 2 → 3 → 1 순으로 출력된다.
    
    ```jsx
    setTimeout(() => console.log(1), 0);
    
    Promise.resolve()
    	.then(() => console.log(2))
    	.then(() => console.log(3));
    ```
    

## `fetch`

`XMLHttpRequest` 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API다. `XMLHttpRequest` 객체보다 사용법이 간단하고 프로미스를 지원한다.

- HTTP 요청을 전송할 URL과 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달한다.
    
    ```jsx
    const promise = fetch(url [, options])
    ```
    
- HTTP 응답을 나타내는 `Response` 객체를 래핑한 `Promise` 객체를 반환한다.
    - `Response` 객체는 HTTP 응답을 나타내는 다양한 프로퍼티를 제공한다.
    - `Response.prototype.json` 메서드는 `Response` 객체에서 HTTP 응답 몸체를 취하여 역직렬화한다.
- `fetch` 함수가 반환하는 프로미스는 기본적으로 404, 500 같은 HTTP 에러가 발생해도 에러를 `reject`하지 않고 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 `reject`한다.
    - `fetch` 함수가 반환한 프로미스가 `resolve`한 불리언 타입의 `ok` 상태를 확인해 명시적으로 에러를 처리해야 한다.
        
        ```jsx
        fetch(wrongUrl)
        	.then(response => {
        		if (!response.ok) throw new Error(response.statusText);
        		return response.json();
        	})
        	.then(todo => console.log(todo))
        	.catch(err => console.error(err));
        ```
        
    - axios는 모든 HTTP 에러를 `reject`하는 프로미스를 반환하며 인터셉터, 요청 설정 등 `fetch`보다 다양한 기능을 지원한다.
- `fetch` 함수를 통해 HTTP 요청을 전송하는 코드
    
    ```jsx
    const request = {
    	// 1. GET 요청
    	get(url) {
    		return fetch(url);
    	},
    	// 2. POST 요청
    	post(url, payload) {
    		return fetch(url, {
    			method: 'POST',
    			header: { 'content-Type': 'application/json' },
    			body: JSON.stringify(payload)
    		});
    	},
    	// 3. PATCH 요청
    	patch(url, payload) {
    		return fetch(url, {
    			method: 'PATCH',
    			header: { 'content-Type': 'application/json' },
    			body: JSON.stringify(payload)
    		});
    	},
    	// 4. DELETE 요청
    	delete(url) {
    		return fetch(url, { method: 'DELETE' });
    	}
    };	
    ```
    

[🔗MDN Using Fetch](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Fetch의_사용법)