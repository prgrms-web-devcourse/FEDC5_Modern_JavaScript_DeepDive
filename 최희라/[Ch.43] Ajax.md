# Ajax

## Ajax(Asynchronous JavaScript and XML)란?

- 자바스크립트로 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식
- Web API인 `XMLHttpRequest` 객체를 기반으로 동작

### 전통적인 방식의 문제점

- 이전 웹페이지와 차이가 없어 변경할 필요가 없는 부분까지 포함된 HTML을 서버로부터 매번 다시 전송받기 때문에 **불필요한 데이터 통신** 발생
- 처음부터 다시 렌더링하기 때문에 화면 전환 시 순간적으로 깜박이는 현상 발생
- 클라이언트-서버 통신이 동기 방식으로 동작하기 때문에 블로킹 발생

### Ajax의 장점

- 갱신이 필요한 부분의 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
- 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. (화면 깜박임 ❌)
- 클라이언트-서버 통신이 비동기 방식으로 동작하기 때문에 블로킹이 없다.

## JSON(JavaScript Object Notation)

클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷

### 표기 방식

- 객체 리터럴과 유사하게 키, 값으로 구성된 순수한 텍스트다.
- 키를 포함한 문자열은 반드시 큰따옴표로 묶어야 하며 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다.

### JSON.stringify

- 객체를 JSON 포맷의 문자열로 변환하는 메서드
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화(serializing)라 한다.
- `JSON.stringify(value[, replacer[, space]])`
    - `value`: JSON 포맷의 문자열로 변환할 객체 또는 배열
    - `replacer`: `value`를 변환할 수 있는 함수
    - `space`: 들여쓰기 할 숫자

### JSON.parse

- JSON 포맷의 문자열을 객체로 변환하는 메서드
- 문자열을 객체로 사용하기 위해 JSON 포맷의 문자열을 객체화해야 하는데 이를 역직렬화(deserializing)라 한다.
- `JSON.parse(text[, reviver])`
    - `text`: 객체 또는 배열 객체로 변환할 문자열
    - `reviver`: `text`를 변환할 수 있는 함수

```jsx
const obj = {
	name: "Choi",
	age: 25,
	alive: true,
	hobby: ['traveling', 'cycling']
};

const json = JSON.stringify(obj, null, 2);
console.log(typeof json, json);
/*
string {
  "name": "Choi",
  "age": 25,
  "alive": true,
  "hobby": [
    "traveling",
    "cycling"
  ]
}
*/

const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {name: 'Choi', age: 25, alive: true, hobby: ['traveling', 'cycling']}
```

## XMLHttpRequest

Web API로, HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

### 생성

`XMLHttpRequest` 생성자 함수를 호출하여 생성하며, Web API이므로 브라우저 환경에서만 정상적으로 실행된다.

```jsx
const xhr = new XMLHttpRequest();
```

### 프로퍼티와 메서드

`XMLHttpRequest` 객체는 다양한 프로퍼티와 메서드를 제공하는데, 이중 중요한 프로퍼티와 메서드는 다음과 같다.

**프로토타입 프로퍼티**

| 프로퍼티 | 설명 |
| --- | --- |
| `readyState` | HTTP 요청의 현재 상태를 나타내는 정수<br/>• UNSENT: 0<br/>• OPENED: 1<br/>• HEADERS_RECEIVED: 2<br/>• LOADING: 3<br/>• DONE: 4 |
| `status` | HTTP 요청에 대한 응답 상태(상태 코드)를 나타내는 정수 |
| `statusText` | HTTP 요청에 대한 응답 메시지를 나타내는 문자열 |
| `responseType` | HTTP 응답 타입 |
| `response` | HTTP 요청에 대한 응답 몸체로, `responseType`에 따라 타입이 다르다. |

**이벤트 핸들러 프로퍼티**

| 프로퍼티 | 설명 |
| --- | --- |
| `onreadystatechange` | `readyState` 프로퍼티 값이 변경된 경우 |
| `onerror` | HTTP 요청에 에러가 발생한 경우 |
| `onload` | HTTP 요청이 성공적으로 완료한 경우 |

**메서드**

| 메서드 | 설명 |
| --- | --- |
| `open` | HTTP 요청 초기화 |
| `send` | HTTP 요청 전송 |
| `abort` | 이미 전송된 HTTP 요청 중단 |
| `setRequestHeader` | 특정 HTTP 요청 헤더의 값을 설정 |

**정적 프로퍼티**

| 프로퍼티 | 값 | 설명 |
| --- | --- | --- |
| `DONE` | 4 | 서버 응답 완료 |

### HTTP 요청 전송

HTTP 요청 전송은 다음 순서를 따른다.

1. `XMLHttpRequest.prototype.open` 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 `XMLHttpRequest.prototype.setRequestHeader` 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. `XMLHttpRequest.prototype.send` 메서드로 HTTP 요청을 전송한다.

```jsx
const xhr = new XMLHttpRequest();

xhr.open('GET', '/users');  // 1
xhr.setRequestHeader('content-type', 'application/json');  // 2
xhr.send();  // 3
```

**`XMLHttpRequest.prototype.open`**

`open` 메서드에 인수로 전달할 주요 HTTP 요청 메서드는 다음과 같다.

| HTTP 요청 메서드 | 종류 | 목적 | 페이로드 |
| --- | --- | --- | --- |
| GET | index/retrieve | 모든/특정 리소스 취득 | ❌ |
| POST | create | 리소스 생성 | ⭕ |
| PUT | replace | 리소스의 전체 교체 | ⭕ |
| PATCH | modify | 리소스의 일부 수정 | ⭕ |
| DELETE | delete | 모든/특정 리소스 삭제 | ❌ |

**`XMLHttpRequest.prototype.send`**

`send` 메서드에 페이로드(요청 몸체에 담아 전송할 데이터)를 인수로 전달할 수 있으며, 객체인 경우 반드시 `JSON.stringify` 메서드로 직렬화한 다음 전달해야 한다.

**`XMLHttpRequest.prototype.setRequestHeader`**

특정 HTTP 요청의 헤더 값을 설정한다. 반드시 `open` 메서드를 호출한 뒤에 호출해야 한다.

| HTTP 요청 헤더 | 설명 |
| --- | --- |
| Content-type | 페이로드의 MIME 타입 정보를 표현한다. |
| Accept | 서버가 응답할 데이터의 MIME 타입을 지정한다. |

### HTTP 응답 처리

1. `readystatechange` 이벤트를 캐치하여 HTTP 요청의 현재 상태를 확인한다.
2. `xhr.readyState`가 `XMLHttpRequest.DONE`인지 확인하여 서버 응답이 완료되었는지 확인한다.
    
    → `load` 이벤트를 캐치하면 해당 과정 생략 가능
    
3. 응답이 완료되면 `xhr.status`가 200인지 확인한다. (200이 아니라면 필요한 에러 처리를 한다.)
4. `xhr.response`에서 서버가 전송한 데이터를 취득한다.
