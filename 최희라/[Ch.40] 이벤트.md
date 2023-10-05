# 이벤트


## 이벤트 드리븐 프로그래밍

- 이벤트 핸들러: 이벤트 발생 시 호출될 함수
- 이벤트 핸들러 등록: 이벤트 발생 시 브라우저에게 이벤트 핸들러 호출을 위임하는 것

이벤트 드리븐 프로그래밍은 **프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식**이다.

## 이벤트 타입

이벤트 종류를 나타내는 문자열로, 약 200여 가지가 있다.

[🔗MDN Event reference](https://developer.mozilla.org/en-US/docs/Web/Events)

## 이벤트 핸들러 등록

### 1. 이벤트 핸들러 어트리뷰트 방식

- HTML 요소의 어트리뷰트로 작성
- 이름은 `on` 접두사 + 이벤트 타입으로 구성
- 값으로 문(statement)을 할당하면 이벤트 핸들러가 등록된다.
    - 함수 참조가 아닌 문을 할당하는 이유는 이벤트 핸들러 어트리뷰트 값이 **암묵적으로 생성될 이벤트 핸들러의 함수 몸체**를 의미하기 때문이다.
    - 이벤트 핸들러에 **인수를 전달**하기 위해 이같이 동작한다.
    
    ```jsx
    onclick="sayHi("Choi")"
    
    function sayHi(event) {
    	sayHi("Choi");
    }
    ```
    
- 사용하지 않는 것이 좋지만, 모던 JS 라이브러리/프레임워크에서 쓰이는 경우가 있다.

### 2. 이벤트 핸들러 프로퍼티 방식

- DOM 노드 객체의 프로퍼티로 함수 바인딩
- 이름은 `on` 접두사 + 이벤트 타입으로 구성
- 이벤트 타깃 또는 전파된 이벤트를 캐치할 DOM 노드 객체에 바인딩된다.
- **하나의 이벤트 핸들러만 바인딩할 수 있다.**

```jsx
const $button = document.querySelector('button');
$button.onclick = function() {
	console.log('button click');
};
```

### 3. `addEventListener` 메서드 방식

- `EventTarget.prototype.addEventListener` 메서드를 사용하여 등록
    - 첫 번째 매개변수: 이벤트 타입
    - 두 번째 매개변수: 이벤트 핸들러 전달
    - 마지막 매개변수(선택): 이벤트를 캐치할 전파 단계 지정(`false`: 버블링 `true`: 캡처링)
- 이벤트 핸들러 프로퍼티 방식과 중복으로 적용된다.
- 하나 이상의 이벤트 핸들러를 등록할 수 있다. (등록 순서대로 호출)
- 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나만 등록된다.

```jsx
const $button = document.querySelector('button');
$button.addEventListener('click', function () {
	console.log('button click');
});
```

## 이벤트 핸들러 제거

- `EventTarget.prototype.removeEventListener` 메서드를 사용하여 `addEventListener` 메서드로 등록한 이벤트 핸들러 제거한다.
    - `addEventListener` 메서드에 전달한 인수와 일치하지 않으면 동작하지 않는다.
    
    ```jsx
    const $button = document.querySelector('button');
    $button.addEventListener('click', handleClick);
    $button.removeEventListener('click', handleClick);  // 이벤트 핸들러 제거
    ```
    
- 무명 함수로 이벤트 핸들러를 등록하면 제거할 수 없다.
    - 내부에서 `arguments.callee`와 `removeEventListener`를 활용하면 제거할 수 있지만 권장하지 않는다.
- 기명 이벤트 핸들러 내부에서 `removeEventListener` 메서드를 호출하여 제거할 수도 있다.
- 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러는 `null`을 할당하여 제거한다.

## 이벤트 객체

- 이벤트 발생 시 이벤트 관련 정보를 담고 있는 이벤트 객체가 동적으로 생성된다.
- 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
- 이벤트 객체를 전달받으려면 **이벤트 핸들러에 매개변수를 명시적으로 선언**해야 한다.

### 이벤트 객체의 상속 구조

이벤트 객체는 생성자 함수와 더불어 생성되는 프로토타입으로 구성된 프로토타입 체인의 일원이 된다.

### 이벤트 객체의 공통 프로퍼티

- `Event.prototype`에 정의되어 있는 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티다.
- `type`, `target`, `currentTarget`, `eventPhase` 등의 프로퍼티가 있다.

### 마우스 정보 취득

`MouseEvemt` 타입의 이벤트 객체는 다음과 같은 고유 프로퍼티를 갖는다.

- 마우스 포인터 좌표 정보를 나타내는 프로퍼티: `screenX`/`screenY`, `clientX`/`clientY`, `pageX`/`pageY`, `offsetX`/`offsetY`
- 버튼 정보를 나타내는 프로퍼티: `altKey`, `ctrlKey`, `shiftKey`, `button`

### 키보드 정보 취득

`KeyboardEvent` 타입의 이벤트 객체는 다음과 같은 고유 프로퍼티를 갖는다.

- `altKey`, `ctrlKey`, `shiftKey`, `metaKey`, `key`, `keyCode`

[🔗JavaScript Key Code](http://keycode.info)

## 이벤트 전파

- DOM 트리 내의 DOM 요소 노드에서 발생한 이벤트는 **DOM 트리를 따라 전파(propagation)** 된다.
    1. 캡처링 단계: 이벤트가 상위 요소에서 하위 요소로 전파
    2. 타깃 단계: 이벤트가 이벤트 타깃에 도달
    3. 버블링 단계: 이벤트가 하위 요소에서 상위 요소로 전파
- 이벤트 핸들러 어트리뷰트/프로퍼티 방식으로 등록된 이벤트 핸들러는 타깃, 버블링 단계의 이벤트만 캐치할 수 있다.
- `addEventListener` 메서드는 세 단계의 이벤트를 모두 캐치할 수 있다.
- 일부 이벤트는 버블링을 통해 전파되지 않지만 대체할 수 있는 이벤트가 존재한다.

## 이벤트 위임

- 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 **상위 DOM 요소 하나에 등록**하는 방법
- `Element.prototype.matches` 메서드로 반응이 필요한 DOM 요소에 한정하여 이벤트 핸들러를 등록할 수 있다.

## DOM 요소의 기본 동작 조작

### DOM 요소의 기본 동작 중단

`preventDefault` 메서드 사용.

### 이벤트 전파 방지

`stopPropagation` 메서드 사용. 하위 DOM 요소의 이벤트를 개별적으로 처리하기 위해 쓰인다.

## 이벤트 핸들러 내부의 `this`

### 이벤트 핸들러 어트리뷰트 방식

```jsx
<button onclick="handleClick(this)">Click me</button>
<script>
	function handleClick(button) {
		cnosole.log(button);  // 이벤트를 바인딩한 button 요소
		console.log(this);    // 전역 객체 windeow
	}
</script>
```

### 이벤트 핸들러 프로퍼티 방식과 `addEventListener` 메서드 방식

- 일반적으로 이벤트 핸들러 내부의 `this`는 이벤트를 바인딩한 DOM 요소(`currentTarget`)를 가리킨다.
- 화살표 함수로 정의할 경우 상위 스코프의 `this`를 가리킨다.
- 클래스에서 이벤트 핸들러를 바인딩하는 경우 메서드 내부의 `this`는 클래스가 생성할 인스턴스가 아닌 이벤트를 바인딩한 DOM 요소를 가리킨다. → `bind` 메서드 사용

## 이벤트 핸들러에 인수 전달

- 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
    
    ```jsx
    $input.onblur = () => {
    	checkUserNameLength(MIN_USER_NAME_LENGTH);
    };
    ```
    
- 이벤트 핸들러를 반환하는 함수를 호출하면서 인수를 전달한다.
    
    ```jsx
    const checkUserNameLength = min => e => {
    	$msg.textContent = $input.value.length < min ? `${min}자 이상 입력하세요.` : '';
    }
    
    $input.onblur = checkUserNameLength(MIN_USER_NAME_LENGTH);
    ```
    

## 커스텀 이벤트

### 커스텀 이벤트 생성

- 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있다.
- 전달 받는 인수는 다음과 같다.
    - 첫 번째 인수: 이벤트 타입(문자열)
    - 두 번째 인수(선택): 이벤트 객체 고유 프로퍼티를 갖는 객체
    
    ```jsx
    const mouseEvent = new MouseEvent('click, {
    	bubbles: true,  // 버블링 가능
    	cancelable: true,  // preventDefault 메서드 사용 가능
    	clientX: 50,
    	clientY: 100
    });
    ```
    
- 커스텀 이벤트는 `isTrusted` 프로퍼티 값이 언제나 `false`다.

### 커스텀 이벤트 디스패치

- 커스텀 이벤트는 `dispatchEvent` 메서드로 디스패치(이벤트를 발생시키는 행위)할 수 있다.
- `dispatchEvent` 메서드는 동기 처리 방식으로 이벤트 핸들러를 호출하기 때문에 디스패치하기 전에 이벤트 핸들러를 등록해야 한다.
- `CustomEvent` 생성자 함수에는 두 번째 인수로 `detail` 프로퍼티를 포함하는 객체를 전달할 수 있다.
- 커스텀 이벤트 객체를 생성한 경우 반드시 `addEventListener` 메서드 방식으로 등록해야 한다.