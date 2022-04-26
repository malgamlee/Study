# let, const, var

<details>
<summary>let</summary>
<div markdown="1">

## let

-  블록 스코프의 범위를 가지는 지역 변수를 선언하며, 선언과 동시에 임의의 값으로 초기화할 수도 있다.

```javascript
let x = 1;

if (x === 1) {
  let x = 2;

  console.log(x);
  // expected output: 2
}

console.log(x);
// expected output: 1
```

- 위 구문 대신 구조 분해 할당을 사용해서 변수를 선언할 수 있다.

```javascript
let { bar } = foo; // foo = { bar: 10, baz: 12 };
/* 10의 값을 가진 'bar' 변수를 생성 */
```

### 설명
- let을 사용하면 블록 명령문이나 let을 사용한 표현식 내로 범위가 제한되는 변수를 선언할 수 있다. 
  - let이 var 키워드와 다른 점
    - var는 변수를 블록을 고려하지 않고 현재 함수(또는 전역 스코프) 어디에서나 접근할 수 있는 변수를 선언한다.
    - let은 파서가 구문을 평가해야 변수를 값으로 초기화 한다. 

#### 스코프 규칙
- let으로 선언한 변수는 자신을 선언한 블록과 모든 하위 블록을 스스로의 스코프로 가진다.
  - 이런 점에서는 let이 var와 유사하다.
- 하지만 둘의 중요한 차이는, var의 경우 스코프가 '자신을 선언한 블록'이 아니라, '자신의 선언을 포함하는 함수'라는 점이다.

```javascript
function varTest() {
  var x = 1;
  if (true) {
    var x = 2; // 같은 변수!
    console.log(x); // 2
  }
  console.log(x); // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2; // 다른 변수
    console.log(x); // 2
  }
  console.log(x); // 1
}
```

- 프로그램 최상위에서 사용할 경우 var는 전역 객체에 속성을 추가하지만, let은 추가하지 않는다.

```javascript
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```

#### 비공개 멤버 모사
(이 부분은 이해못했으니 꼭 다음에 기억하고 이해하기)

- 생성자와 함께 let을 사용하면 클로저를 사용하지 않아도 비공개 멤버를 나타낼 수 있다.

```javascript
var Thing;

{
  let privateScope = new WeakMap();
  let counter = 0;

  Thing = function() {
    this.someProperty = 'foo';

    privateScope.set(this, {
      hidden: ++counter,
    });
  };

  Thing.prototype.showPublic = function() {
    return this.someProperty;
  };

  Thing.prototype.showPrivate = function() {
    return privateScope.get(this).hidden;
  };
}

console.log(typeof privateScope);
// "undefined"

var thing = new Thing();

console.log(thing);
// Thing {someProperty: "foo"}

thing.showPublic();
// "foo"

thing.showPrivate();
// 1
```

#### 재선언

- 같은 변수를 같은 함수나 블록 스코프 안에서 다시 선언하려고 시도하면 SyntaxError가 발생한다.

```javascript
if (x) {
  let foo;
  let foo; // SyntaxError
}
```

- switch 명령문에는 블록이 하나밖에 없으므로 이 오류를 자주 마주칠 수 있다.

```javascript
let x = 1;
switch(x) {
  case 0:
    let foo;
    break;

  case 1:
    let foo; // 재선언으로 인한 SyntaxError
    break;
}
```

- 분기에 블록을 배치하면 블록 스코프도 생성하므로 재선언으로 인한 오류가 발생하지 않는다.

```javascript
let x = 1;

switch(x) {
  case 0: {
    let foo;
    break;
  }
  case 1: {
    let foo;
    break;
  }
}
```

#### 시간상 사각지대

- let 변수는 초기화하기 전에는 읽거나 쓸 수 없다. (선언 구문에 초기값을 지정하지 않은 경우 undefined로 초기화함)
  - var 변수는 선언 전에 접근을 시도하면 undefined 이다. 
- 초기화 전에 접근을 시도하면 ReferenceError가 발생한다.
- 변수 스코프의 맨 위에서 변수의 초기화 완료 시점까지의 변수는 '시간상 사각지대'(Temporal Dead Zone, TDZ)에 들어간 변수라고 표현한다.

```javascript
function do_something() {
  console.log(bar); // undefined
  console.log(foo); // ReferenceError
  var bar = 1;
  let foo = 2;
}
```

- '시간상' 사각지대인 이유는, 사각지대가 코드의 작성 순서(위치)가 아니라 코드의 실행 순서(시간)에 의해 형성되기 때문이다.
- 아래 코드의 경우 let 변수 선언 코드가 그 변수에 접근하는 함수보다 아래에 위치하지만, 함수의 호출 시점이 사각지대 밖이므로 정상 동작한다.

```javadscript
{
    // TDZ가 스코프 맨 위에서부터 시작
    const func = () => console.log(letVar); // OK

    // TDZ 안에서 letVar에 접근하면 ReferenceError

    let letVar = 3; // letVar의 TDZ 종료
    func(); // TDZ 밖에서 호출함
}      
```

#### 어휘적 스코프와 결합한 TDZ

```javascript
function test(){
   var foo = 33;
   if(foo) {
      let foo = (foo + 55); // ReferenceError
   }
}
test();
```

- 바깥 스코프의 var foo가 값을 가지므로 if 블록 또한 평가된다.
- 그러나 어휘적 스코프로 인해, var foo의 값은 블록 내에서 사용할 수 없다.
  - 블록 내 foo 식별자는 let foo를 가리키기 때문
- 따라서 (foo + 55) 표현식은 let foo의 초기화가 끝나지 않은, 즉 TDZ의 내부이며 ReferenceError가 발생하게 된다. 

```javascript
function go(n) {
  // 이 n은 매개변수 n
  console.log(n); // Object {a: [1,2,3]}

  for (let n of n.a) { // ReferenceError
    console.log(n);
  }
}

go({a: [1, 2, 3]});
```

- 반복문의 let n of n.a는 for 블록의 스코프에 속하므로, 식별자 n.a는 반복문 스스로가 선언(let n)하는 n 객체의 a 속성을 가리킨다.
- 그리고 n 선언 후 초기화가 아직 끝나지 않았으므로 n.a는 let n의 TDZ에 속한다.


#### 기타 예제

```javascript
var a = 1;
var b = 2;

if (a === 1) {
  var a = 11; // 전역 변수
  let b = 22; // if 블록 변수

  console.log(a);  // 11
  console.log(b);  // 22
}

console.log(a); // 11
console.log(b); // 2
```

- 블록 내에서 사용한 경우 let은 변수의 스코프를 해당 블록으로 제한한다.
  - var는 스코프를 함수로 제한한다는 차이에 주의

```javascript
let x = 1;

{
  var x = 2; // 재선언으로 인한 SyntaxError
}
```

- 위와 같이 var와 let을 같이 사용하면 SyntaxError이다.
  - 호이스팅으로 인해 var가 블록 최상단으로 끌어올려져, 변수 재선언을 하는 것과 같아진다.

</div>
</details>

<details>
<summary>const</summary>
<div markdown="1">

## const

- const 선언은 블록 범위의 상수를 선언한다.
- 상수의 값은 재할당할 수 없으며, 다시 선언할 수도 없다.

```javascript
const number = 42;

try {
  number = 99;
} catch (err) {
  console.log(err);
  // expected output: TypeError: invalid assignment to const `number'
  // Note - error messages will vary depending on browser
}

console.log(number);
// expected output: 42
```

### 설명
- const는 선언된 함수에 전역 또는 지역일 수 있는 상수를 만든다.
- 상수 초기자(initializer)가 필요하다.
  - 즉, 선언되는 같은 문에 그 값을 지정해야 한다.
- 상수는 let문을 사용하여 정의된 변수와 마찬가지로 블록 범위(block-scope)이다.
- 상수의 값은 재할당을 통해 바뀔 수 없고 재선언될 수 없다.
- let에 적용한 '일시적 사각 지대'에 관한 모든 고려는 const에도 적용한다.
- 상수는 같은 범위의 상수 또는 변수와 그 이름을 공유할 수 없다.

```javascript
// 주의: 상수 선언에는 대소문자 모두 사용할 수 있지만,
// 일반적인 관습은 모두 대문자를 사용하는 것입니다.

// MY_FAV를 상수로 정의하고 그 값을 7로 함
const MY_FAV = 7;

// 에러가 발생함
MY_FAV = 20;

// 7 출력
console.log("my favorite number is: " + MY_FAV);

// 상수를 재선언하려는 시도는 오류 발생 - Uncaught SyntaxError: Identifier 'MY_FAV' has already been declared
const MY_FAV = 20;

// MY_FAV라는 이름은 위에서 상수로 예약되어 있어서 역시 실패함.
var MY_FAV = 20;

// 역시 오류가 발생함
let MY_FAV = 20;

// 블록 범위의 특성을 아는게 중요
if (MY_FAV === 7) {
    // 블록 범위로 지정된 MY_FAV 라는 변수를 만드므로 괜찮습니다
    // (let으로 const 변수가 아닌 블록 범위를 선언하는 것과 똑같이 동작합니다)
    let MY_FAV = 20;

    // MY_FAV는 이제 20입니다
    console.log('my favorite number is ' + MY_FAV);

    // 이 선언은 전역으로 호이스트되고 에러가 발생합니다.
    var MY_FAV = 20;
}

// MY_FAV는 여전히 7
console.log('my favorite number is ' + MY_FAV);

// const 선언시에 초기값을 생략해서 오류 발생
const FOO;

// const는 오브젝트에도 잘 동작합니다
const MY_OBJECT = {'key': 'value'};

// 오브젝트를 덮어쓰면 오류가 발생합니다
MY_OBJECT = {'OTHER_KEY': 'value'};

// 하지만 오브젝트의 키는 보호되지 않습니다.
// 그러므로 아래 문장은 문제없이 실행됩니다
MY_OBJECT.key = 'otherValue'; // 오브젝트를 변경할 수 없게 하려면 Object.freeze() 를 사용해야 합니다

// 배열에도 똑같이 적용됩니다
const MY_ARRAY = [];
// 배열에 아이템을 삽입하는 건 가능합니다
MY_ARRAY.push('A'); // ["A"]
// 하지만 변수에 새로운 배열을 배정하면 에러가 발생합니다
MY_ARRAY = ['B']
```

</div>
</details>

<details>
<summary>var</summary>
<div markdown="1">

## var

- var 문은 변수를 선언하고 선택적으로 초기화할 수 있다.

```javascript
var x = 1;

if (x === 1) {
  var x = 2;

  console.log(x);
  // expected output: 2
}

console.log(x);
// expected output: 2
```

### 설명

- 어디에 선언이 되어있든 간에 변수들은 어떠한 코드가 실행되기 전에 처리된다.
- var로 선언된 변수의 범위는 현재 실행 문맥인데, 그 문맥은 둘러싼 함수, 혹은 함수의 외부에 전역으로 선언된 변수도 될 수 있다.
- 선언된 변수들의 값 할당은 할당이 실행될 때 전역 변수처럼 생성이 된다.

#### 선언된 변수들과 선언되지 않은 변수들의 차이점

1. 선언된 변수들은 변수가 선언된 실행 콘텍스트(exeution context) 안에서 만들어진다. 선언되지 않은 변수들은 항상 전역변수이다.

```javascript
function x() {
  y = 1;   // strict 모드에서는 ReferenceError를 출력합니다.
  var z = 2;
}

x();

console.log(y); // 로그에 "1" 출력합니다.
console.log(z); // ReferenceError: z is not defined outside x를 출력합니다.
```

2. 선언된 변수들은 어떠한 코드가 실행되기 전에 만들어진다. 선언되지 않은 변수들은 변수들을 할당하는 코드가 실행되기 전까지는 존재하지 않는다.

```javascript
console.log(a);                // ReferenceError를 출력합니다.
console.log('still going...'); // 결코 실행되지 않습니다.
```
```javascript
var a;
console.log(a);                // 브라우저에 따라 로그에 "undefined" 또는 "" 출력합니다.
console.log('still going...'); // 로그에 "still going..." 출력합니다.
```

3. 선언된 변수들은 변수들의 실행 콘텍스트(execution context)의 프로퍼티가 변경되지 않는다. 선언되지 않은 변수들은 변경 가능하다.

```javascript
var a = 1;
b = 2;

delete this.a; // strict 모드에서는 TypeError를 출력합니다. 그렇지 않으면 자동적으로 실패합니다.
delete this.b;

console.log(a, b); // ReferenceError를 출력합니다.
// 'b' 프로퍼티는 삭제되었고, 어디에도 존재하지 않습니다.
```

- 세 가지 다른점 때문에 변수 선언 오류는 예기치 않은 결과로 이어질 가능성이 높다.
- 그러므로 함수 또는 전역 범위인지 여부와 상관없이 항상 변수를 선언하는 것을 추천한다.

#### var 호이스팅(hoisting)
- 변수 선언들 (그리고 일반적인 선언은) 어느 코드가 실행되기 전에 처리하기 때문에, 코드 안에서 어디서든 변수 선언은 최상위에 선언한 것과 동등하다.
- 이것은 변수가 선언되기 전에 사용될 수 있다는 것을 의미한다.
- 변수 선언이 함수 또는 전역 코드의 상단에 이동하는 것과 같은 행동을 '호이스팅(hoisting)'이라고 부른다.

```javascript
bla = 2
var bla;
// ...

// 위 선언을 다음과 같이 암묵적으로 이해하면 됩니다:

var bla;
bla = 2;
```
- 이러한 이유로, 그들의 범위 (전역 코드의 상단 그리고 함수 코드의 상단) 상단에 변수를 항상 선언하기를 권장한다.
  - 그러면 변수는 함수 범위 (지역)이 되며, 스코프 체인으로 해결될 것이다. 

- 두 변수들의 선언 및 초기화

`var a = 0, b = 0;`

- 단일 문자열 값으로 두 변수 할당

```javascript
var a = "A";
var b = a;

// 다음과 같음:

var a, b = a = "A";
```
  - 순서에 유의
  ```javascript
  var x = y, y = 'A';
  console.log(x + y); // undefinedA
  ```
  
  -  x와 y는 어떠한 코드가 실행되기 전에 선언되었고, 할당은 후에 발생했다.
  -  'x=y'가 실행될 때, y는 존재하여 ReferenceError를 출력하지는 않고, 값은 undefined 이다.
    - 그러므로 x는 undefined 값이 할당된다.
  - 그 후 y는 'A' 값이 할당된다.
  - 결과적으로 첫번째 줄 이후에 x === undefined && y === 'A' 결과가 발생한다.

- 다수의 변수들의 초기화

```javascript
var x = 0;

function f(){
  var x = y = 1; // x는 지역변수로 선언됩니다. y는 아닙니다!
}
f();

console.log(x, y); // 0, 1
// x는 예상대로 전역이다
// y leaked outside of the function, though! 
```

- 암묵적인 전역변수와 외부 함수 범위
  - 암묵적인 전역변수가 될 것으로 보이는 변수는 함수 범위 밖에서 변수들을 참조할 수 있다. 

  ```javascript
  var x = 0;  // x는 전역으로 선언되었고, 0으로 할당됩니다.

  console.log(typeof z); // undefined, z는 아직 존재하지 않습니다.

  function a() { // a 함수를 호출했을 때,
    var y = 2;   // y는 함수 a에서 지역변수로 선언되었으며, 2로 할당됩니다.

    console.log(x, y);   // 0 2

    function b() {       // b 함수를 호출하였을때,
      x = 3;  // 존재하는 전역 x값에 3을 할당, 새로운 전역 var 변수를 만들지 않습니다.
      y = 4;  // 존재하는 외부 y값에 4를 할당, 새로운 전역 var 변수를 만들지 않습니다.
      z = 5;  // 새로운 전역 z 변수를 생성하고 5를 할당 합니다.
    }         // (strict mode에서는 ReferenceError를 출력합니다.)

    b();     // 호출되는 b는 전역 변수로 z를 생성합니다.
    console.log(x, y, z);  // 3 4 5
  }

  a();                   // 호출되는 a는 또한 b를 호출합니다.
  console.log(x, z);     // 3 5
  console.log(typeof y); // undefined y는 function a에서 지역 변수입니다.
  ```

</div>
</details>


## 참고
- [let](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let)
- [const](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/const)
- [var](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/var)
