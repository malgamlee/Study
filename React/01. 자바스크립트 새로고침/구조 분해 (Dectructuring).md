# 구조 분해 (Dectructuring)
# Destructuring

- 배열 요소나 객체 속성을 추출해서 변수로 저장하는 역할
    - 하나의 요소나 속성만을 배열이나 객체를 위한 변수로 저장

## Array Destructuring (배열구조분해)

```jsx
[a, b] = ['Hello', 'Max']
console.log(a) // Hello
console.log(b) // Max
```

- 배열을 만드는 것 같지만, 변수 a, b를 hello, max에 할당하는 것
- example

```jsx
const numbers = [1,2,3];

// 1, 2 출력하기
[num1, num2] = numbers;
console.log(num1, num2);
// 출력
// 1
// 2

// 1, 3 출력하기
[num1, , num3] = numbers;
console.log(num1, num3);
// 출력
// 1
// 3
```

## Object Destructuring

```jsx
{name} = {name : 'Max', age : 28}
console.log(name) // Max
console.log(age) // undefined
```

- 속성의 이름이 정함
- 왼쪽 중괄호가 오른쪽에 있는 속성을 지정하고 값을 추출함
    - 그래서 age가 undefined 된 이유