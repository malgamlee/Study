# 스프레드 및 나머지 연산자 (Spread & Rest Operators)
# Spread & Rest Operators(연산자 ...)

## Spread(전개 연산자)

- Used to split up array elements OR object properties
    - 배열 요소 또는 개체 속성을 분할하는 데 사용
    - 그러므로 배열이나 객체를 전개할 수 있음

```jsx
const newArray = [...oldArray, 1, 2]
const newObject = {...oldObject, newProp:5}
```

- 예전 배열의 모든 요소를 새 배열에 추가할 때
    - 예전 배열 앞에 있는 점 3개는 모든 요소를 빼내서 새 배열에 추가할 것
- 객체일 때
    - 점 3개가 예전 객체의 모든 속성을 빼내서 새 객체에 추가
    - 예전 객체가 새 속성을 가지고 있을 때 사이드 노트로 추가하는 것
        - 새 속성 : 5
- example

```jsx
// ===== 배열 =====
const numbers = [1,2,3];
const newNumbers = [...numbers, 4];

console.log(newNumbers);
// 출력
// [1,2,3,4]

// ===== 객체 =====
const person = {
  name : 'Max'
};

const newPerson = {
  ...person,
  age : 28
};

console.log(newPerson);
// 출력
//[object Object] {
//  age: 28,
//  name: "Max"
//} 
```

## Rest

- Used to merge a list of function arguments into an array
    - 매개변수 리스트를 배열로 통합

```jsx
function sortArgs(...args) {
	return args.sort()
}
```

- sortArgs는 매개변수를 무한정 받음
- 점 3개와 매개변수 하나를 쓰더라도 하나 이상의 매개변수를 받고, 그게 배열로 합쳐짐
- 따라서 배열 방법을 매개변수 리스트에 적용하거나, 기존의 저장된 매개변수에 뭐든지 할 수 있음
- example
```jsx
const filter = (...args) => {
  return args.filter(el => el === 1);
}
console.log(filter(1,2,3));
// 출력
// [1]

const sortArgs = (...args) => {
  return args.sort();
};

console.log(sortArgs(5,2,4,6));
// 출력
// [2,4,5,6]
```