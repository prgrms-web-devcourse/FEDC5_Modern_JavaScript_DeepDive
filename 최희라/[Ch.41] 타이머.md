# 타이머

## 호출 스케줄링

타이머 함수를 이용하여 일정 시간 이후 함수를 실행하도록 함수 호출을 예약하는 것

- `setTimeout`, `setInterval`, `clearTimeout`, `clearInterval` 함수 제공 ⇒ 호스트 객체
- 타이머 함수는 비동기 처리 방식으로 동작한다.

## 타이머 함수

### `setTimeout`/`clearTimeout`

- `setTimeout` 함수는 전달 받은 시간으로 **단 한 번 동작하는 타이머**를 생성하며 인수로 전달받은 콜백함수를 호출한다.
- `setTimeout(func|code[, delay, param1, param2, …])` 매개변수
    - `func`: 타이머가 만료된 뒤 호출될 콜백 함수
    - `delay`: 타이머 만료 시간(ms) → 최소 지연 시간 4ms 존재
    - `param1, param2, ...`: 호출 스케줄링된 콜백 함수에 전달해야 하는 인수
- 타이머가 만료되면 콜백 함수가 즉시 호출되는 것이 보장되지 않는다.
- 고유한 타이머 `id`를 반환한다. (브라우저 - 숫자, `Node.js` - 객체)
- 타이머가 반환한 `id`를 `clearTimeout` 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```jsx
// 1초 뒤 콜백 함수 호출
const timerId = setTimeout(() => console.log("Hello!"), 1000);

// 콜백 함수가 실행되지 않음.
clearTimeout(timerId);
```

### `setInterval`/`clearInterval`

- `setInterval` 함수는 전달 받은 시간으로 **반복 동작하는 타이머**를 생성하며 인수로 전달받은 콜백함수를 호출한다.
- `setInterval` 함수에 전달할 인수는 `setTimeout` 함수와 동일하다.
- 고유한 타이머 `id`를 반환한다. (브라우저 - 숫자, `Node.js` - 객체)
- 타이머가 반환한 `id`를 `clearInterval` 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```jsx
let count = 1;

const timeoutId = setInterval(() => {
	console.log(count);  // 1 2 3 4 5

	if (count++ === 5) clearInterval(timeoutId);
}, 1000);
```

## 디바운스와 스로틀

짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화하여 **과도한 이벤트 핸들러 호출을 방지**하는 프로그래밍 기법

### 디바운스(debounce)

- 짧은 시간 간격으로 발생하는 이벤트를 그룹화하여 **마지막에 한 번만** 이벤트 핸들러가 호출되게 한다.
- 두 번째 인수로 전달한 시간(`delay`) 안에 이벤트가 또 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정한다.
- 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등에 사용된다.
- Underscore의 `debounce` 함수나 Lodash의 `debounce` 함수 사용을 권장한다.

### 스로틀(throttle)

- 짧은 시간 간격으로 발생하는 이벤트를 그룹화하여 **일정 시간 단위로** 이벤트 핸들러가 호출되게 한다.
- 두 번째 인수로 전달한 시간(`delay`) 동안은 콜백 함수를 호출하지 않고 이후 이벤트가 발생하면 콜백 함수를 호출하고 새로운 타이머를 재설정한다.
- `scroll` 이벤트 처리, 무한 스크롤 UI 구현 등에 사용된다.
- Underscore의 `throttle` 함수나 Lodash의 `throttle` 함수 사용을 권장한다.