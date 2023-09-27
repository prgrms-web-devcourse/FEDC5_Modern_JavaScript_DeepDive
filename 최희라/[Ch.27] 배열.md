# 배열

- 여러 개의 값을 **순차적으로** 나열한 자료구조
- 요소(element)와 인덱스(index), `length` 프로퍼티를 갖는다.
- 객체 타입이지만 일반 객체와는 구별되는 특징이 있다.  


## 자바스크립트 배열의 특징

- 일반적인 배열의 동작을 흉내 낸 특수한 객체다.
- 배열 요소의 메모리 공간이 동일한 크기가 아니어도 되며, 희소 배열이 존재한다.
- 프로퍼티 키로 인덱스(문자열)를, 프로퍼티 값으로 요소를 갖는다.
- 해시 테이블로 구현된 객체이므로 요소 접근은 일반적인 배열에 비해 느리나 요소 검색, 삽입, 삭제는 더 빠르다.


## length 프로퍼티

- 요소 개수로 결정되지만 임의의 숫자 값을 명시적으로 할당할 수도 있다.
- 요소 개수 보다 작은 값을 할당하면 배열의 길이가 줄어든다.
- 더 큰 값을 할당하면 `length` 프로퍼티 값은 변하지만 실제 배열은 아무 변화 없다. (희소 배열)
- 자바스크립트는 문법적으로 희소 배열을 허용하나 사용을 권장하지 않는다.


## 배열 생성

```jsx
// 1. 배열 리터럴
const arr = [1, 2, 3];

// 2. Array 생성자 함수
const arr1 = new Array(10);    // 인수가 1개면 length가 10인 배열 생성
const arr2 = new Array(1, 2, 3);   // [1, 2, 3]
const arr3 = new Array({});    // 인수가 1개여도 숫자가 아니면 인수를 요소로 갖는 배열 생성
// new 연산자와 함께 호출하지 않아도 생성자 함수로 동작한다.

// 3. Array.of
Array.of(1);   // [1]
Array.of(1, 2, 3);   // [1, 2, 3]
Array.of("string");  // ["string"]

// 4. Array.from
Array.from({ length: 2, 0: "a", 1: "b" });   // ["a", "b"]
Array.from("Hello");    // ["H", "e", "l", "l", "o"]
```


## 배열 요소 참조

- 대괄호 표기법 사용
- 존재하지 않는 요소에 접근하면 `undefined` 반환


## 배열 요소의 추가, 갱신, 삭제

- 동적으로 요소 추가 가능
- 존재하는 요소에 값을 재할당하면 요소 갱신
- 요소 삭제를 위해 `delete` 연산자를 사용할 수 있지만 희소 배열이 되므로 권장하지 않는다.
- 요소 삭제는 `Array.prototype.splice` 메서드 활용


## 배열 메서드

- JS는 배열을 다루기 유용한 빌트인 메서드를 제공한다.
- 반환 패턴이 두 가지가 있다.
    - 원본 배열을 직접 변경하는 메서드
    - 원본 배열을 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드
- 원본 배열을 직접 변경하면 부수 효과가 있으므로 가급적 원본 배열을 변경하지 않는 메서드를 사용하는 것이 좋다.

### Array.isArray

전달된 인수가 배열이면 true, 아니면 false를 반환한다.

```jsx
Array.isArray([1, 2]);  // true
Array.isArray(1);  // false
```

### Array.prototype.indexOf

배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.

```jsx
const arr = [1, 2, 2, 3];

// 중복된 요소가 여러 개 있으면 첫 번째로 검색된 요소의 인덱스 반환
arr.indexOf(2);  // 1
// 요소가 없으면 -1 반환
arr.indexOf(4);  // -1
// 두 번째 인수: 검색을 시작할 인덱스. 생략하면 처음부터 검색
arr.indexOf(2, 2); // 2
```

### Array.prototype.push

- 인수로 전달한 모든 값을 원본 배열의 마지막 요소로 추가하고 `length` 프로퍼티 값을 반환한다.
- 원본 배열을 직접 변경한다.
- 추가할 요소가 한 개라면 `push`보다 `length` 프로퍼티를 사용하여 직접 추가하는 것이 더 빠르다.
- 부수 효과가 있기 때문에 ES6의 스프레드 문법을 사용하는 것이 좋다.

```jsx
const arr = [1, 2];
const length = arr.push(3, 4);
console.log(arr, result);  // [1, 2, 3, 4] 4

// arr.push(5)와 같은 처리
arr[arr.length] = 5
console.log(arr);  // [1, 2, 3, 4, 5]
```

### Array.prototype.pop

- 원본 배열에서 마지막 요소를 제거하고 반환한다.
- 원본 배열이 빈 배열이면 `undefined`를 반환한다.
- 원본 배열을 직접 변경한다.
- `push` 메서드를 함께 사용하면 스택을 쉽게 구현할 수 있다.

### Array.prototype.unshift

- 인수로 전달한 모든 값을 원본 배열의 맨 앞에 추가하고 `length` 프로퍼티 값을 반환한다.
- 원본 배열을 직접 변경한다.
- 부수 효과가 있기 때문에 ES6의 스프레드 문법을 사용하는 것이 좋다.

```jsx
const arr = [1, 2];
const result = arr.unshift(3, 4);
console.log(arr, result);  // [3, 4, 1, 2] 4
```

### Array.prototype.shift

- 원본 배열에서 첫 번째 인수를 제거하고 반환한다.
- 원본 배열이 빈 배열이면 `undefined`를 반환한다.
- 원본 배열을 직접 변경한다.
- `push` 메서드를 함께 사용하면 큐를 쉽게 구현할 수 있다.
    - O(n)이라 `shift`를 사용하지 않고 큐를 직접 구현하는 것을 권장하지만, 요소가 적을 경우 JS 엔진에서 최적화를 해준다.

```jsx
const arr = [1, 2];
const result = arr.shift();
console.log(arr, result);  // [2] 1
```

### Array.prototype.concat

- 인수로 전달한 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다.
- 배열을 인수로 전달할 경우 해체하여 추가한다.
- 원본 배열은 변경되지 않는다.
- `push`, `unshift`, `concat` 메서드를 사용하기 보다 일관성있게 스프레드 문법을 사용하는 것이 좋다.

```jsx
const arr = [3, 4];

// unshift 메서드 대체
let result = [1, 2].concat(arr);
console.log(result);  // [1, 2, 3, 4]

// push 메서드 대체
result = result.concat(5, 6);
console.log(result);  // [1, 2, 3, 4, 5, 6]
```

### Array.prototype.splice

- 원본 배열의 중간에 요소를 추가/제거할 경우 사용한다.
- 원본 배열을 직접 변경한다.
- 3개의 매개변수가 있다.
    - `start`: 원본 배열의 요소를 제거하기 시작할 인덱스. start만 지정하면 start부 모든 요소를 제거하며, 음수 값으로 뒤에서부터 인덱스를 지정할 수 있다.
    - `deleteCount`: start부터 제거할 요소의 개수. (옵션)
    - `items`: 제거한 위치에 삽일할 요소의 목록. (옵션)

```jsx
const arr = [1, 2, 3, 4];

const result = arr.splice(1, 2, 20, 30);
console.log(arr, result);  // [1, 20, 30, 4] [2, 3]
```

### Array.prototype.slice

- 인수로 전달한 범위의 요소들을 복사하여 배열로 반환한다.
- 원본 배열은 변경되지 않는다.
- 2개의 매개변수가 있다.
    - `start`: 복사를 시작할 인덱스. 음수값도 가능.
    - `end`: 복사를 종료할 인덱스로, 해당 요소는 복사되지 않는다. (옵션, 기본값: `length`)
- 모든 인수를 생략하면 얕은 복사를 통해 배열을 복사한다.

### Array.prototype.join

- 원본 배열의 모든 요소를 문자열로 변환하고 인수로 전달받은 문자열로 연결한 문자열을 반환한다.
- 인수는 생략 가능하며 기본값은 콤마다.

```jsx
const arr = [1, 2, 3, 4];
arr.join();  // '1,2,3,4'
arr.join("");  // '1234'
```

### Array.prototype.reverse

- 원본 배열의 순서를 반대로 뒤집고 변경된 배열을 반환한다.
- 원본 배열을 직접 변경한다.

### Array.prototype.fill <img src="https://img.shields.io/badge/ES6-yellow?style=flat"/>

- 인수로 전달받은 값으로 요소를 채운다.
- 원본 배열을 직접 변경한다.

```jsx
const arr1 = [1, 2, 3];
const arr2 = [1, 2, 3, 4, 5];

// 두 번째 인수: 요소 채우기를 시작할 인덱스
arr1.fill(0, 1);
console.log(arr1);  // [1, 0, 0]

// 세 번째 인수: 요소 채우기를 멈출 인덱스(해당 인덱스 미포함)
arr2.fill(0, 1, 3);
console.log(arr2);  // [1, 0, 0, 4, 5]
```

### Array.prototpye.includes <img src="https://img.shields.io/badge/ES7-green?style=flat"/>

- 배열 내에 특정 요소가 있는지 확인하여 `true` 또는 `false`를 반환한다..
- 두 번째 인수(옵션)는 검색을 시작할 인덱스로, 음수를 전달하면 `length + index`로 계산된다.

```jsx
const arr = [1, 2, 3];

arr.includes(1, 1);  // false
arr.includes(3, -1); // true
```

### Array.prototype.flat <img src="https://img.shields.io/badge/ES10-blue?style=flat"/>

인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.

```jsx
// 기본값은 1
[1, [2, 3, 4]].flat();  // [1, 2, 3, 4]

// Infinity 전달시 중첩 배열 모두 평탄화
[1, [2, [3, [4]]]].flat(Infinity);  // [1, 2, 3, 4]
```


## 배열 고차 함수

고차함수(Higher-Order Function, HOF)는 **함수를 인수로 전달받거나 반환하는 함수**를 말하며, 외부 상태의 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에 기반을 두고 있다.

<aside>
💡 함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 로직 내에 존재하는 **조건문과 반복문을 제거**하여 복잡성을 해결하고 **변수의 사용을 억제**하여 상태 변경을 피하려는 프로그래밍 패러다임이다.

</aside>

### Array.prototype.sort

- 배열의 요소를 정렬하며 정렬된 배열을 반환한다.
- 원본 배열을 직접 변경한다.
- 배열의 요소를 일시적으로 문자열로 변환한 뒤 유니코드 코드 포인트 순으로 정렬하기 때문에 숫자 배열을 정렬할 때는 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다.

```jsx
const arr = [40, 100, 1, 5, 2, 25, 10];

// 비교 함수의 반환값이 음수면 a를 우선하여 정렬 (오름차순)
arr.sort((a, b) => a - b);
console.log(arr);  // [1, 2, 5, 10, 25, 40, 100] 

// 비교 함수의 반환값이 양수면 b를 우선하여 정렬 (내림차순)
arr.sort((a, b) => b - a);
console.log(arr);  // [100, 40. 25, 10, 5, 2, 1]
```

### Array.prototype.forEach

- `for` 문을 대체할 수 있는 고차 함수로, `for` 문에 비해 성능이 좋지 않지만 가독성이 좋다.
- 내부에서 반복문을 통해 자신을 호출한 배열을 순회하며 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출한다.
- (요소값, 인덱스, `this`)를 콜백함수의 인수로 전달할 수 있다.
- 원본 배열을 변경하지 않지만 콜백함수를 통해 변경할 수 있다.
- 메서드의 두 번째 인수로 콜백 함수 내부에서 `this`로 사용할 객체를 전달할 수 있다.
    - 화살표 함수를 사용하면 상위 스코프의 `this`를 그대로 참조한다.
- `for` 문과 달리 `break`, `continue` 문을 사용할 수 없다.

```jsx
const numbers = [1, 2, 3];
const pows = [];

// for 문
for (let i = 0; i < numbers.length; i++) {
	pows.push(numbers[i]**2);
}

// forEach 메서드
numbers.forEach(item => pow.push(item**2));
```

### Array.prototype.map

- 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달된 콜백 함수를 반복 호출한다.
- 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.
- 원본 배열은 변경되지 않는다.
- `map` 메서드가 생성하여 반환하는 새로운 배열의 `length` 프로퍼티 값은 원본 배열의 `length` 프로퍼티 값과 반드시 일치한다. (1:1 매핑)
- `forEach` 메서드와 마찬가지로 요소, 인덱스, 배열을 인수로 전달할 수 있다.

### Array.prototype.filter

- 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달된 콜백 함수를 반복 호출한다.
- 콜백 함수의 반환값이 `true`인 요소로만 구성된 새로운 배열을 반환한다.
- 원본 배열은 변경되지 않는다.

### Array.prototype.reduce

- 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달된 콜백 함수를 반복 호출한다.
- 콜백 함수의 반환값을 다음 순회 시에 첫 번째 인수로 전달하면서 하나의 결과값을 만들어 반환한다.
- 원본 배열은 변경되지 않는다.
- 콜백함수는 네 개의 인수를 가진다.  → 초기값 또는 콜백함수의 이전 반환값, 요소값, 인덱스, 배열(`this`)
- 평균, 최대값, 요소 중복 횟수 등을 구하거나 평탄화, 중복 요소 제거 등에 활용할 수 있다.
- 두 번째 인수(초기값)를 생략할 수 있지만 전달하는 것이 안전하다.

```jsx
const sum = [1, 2, 3, 4].reduce((accumulator, currentValue, index, array) => accumulator + currentValue, 0);

console.log(sum);  // 10
```

### Array.prototype.some

- 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달된 콜백 함수를 호출한다.
- 콜백 함수의 반환값이 한 번이라도 참이면 `true`, 모두 거짓이면 `false`를 반환한다.
- 호출한 배열이 빈 배열인 경우 `false`를 반환한다.

```jsx
[5, 10, 15].some(item => item > 10);  // true
```

### Array.prototype.every

- 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달된 콜백 함수를 호출한다.
- `some` 메서드와 반대로 콜백 함수의 반환값이 한 번이라도 거짓이면 `false`, 모두 참이면 `true`를 반환한다.
- 호출한 배열이 빈 배열인 경우 `true`를 반환한다.

```jsx
[5, 10, 15].every(item => item > 3);  // true
```

### Array.prototype.find <img src="https://img.shields.io/badge/ES6-yellow?style=flat"/>

- 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달된 콜백 함수를 호출하여 반환값이 `true`인 첫 번째 요소를 반환한다.
- 반환값이 `true`인 요소가 없다면 `undefined`를 반환한다.

### Array.prototype.findIndex <img src="https://img.shields.io/badge/ES6-yellow?style=flat"/>

- `find` 메서드와 비슷하지만 요소가 아닌 인덱스를 반환한다.
- 반환값이 `true`인 요소가 없다면 `-1`을 반환한다.

### Array.prototype.flatMap <img src="https://img.shields.io/badge/ES10-blue?style=flat"/>

- `map` 메서드로 생성된 새로운 배열을 평탄화한다.
- `flat` 메서드처럼 평탄화 깊이를 지정할 수는 없다.

```jsx
const arr = ['hello', 'world'];

// 같은 결과
arr.map(x => x.split('')).flat();  // ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
arr.flatMap(x => x.split(''));  // ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
```