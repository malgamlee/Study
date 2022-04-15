# 여러 State 다루기

```jsx
const titleChangeHandler = (event) => {
    console.log(event.target.value);
  };
```
- 위 코드의 event.target.value 을 통해 얻은 값으로 무엇을 하고 싶은가?
  - 나중에 폼이 넘겨졌을 때 그 값을 사용할 수 있도록 한다.

- 함수(ExpenseForm 컴포넌트 함수)가 어떤 이유든 다시 실행된다고 해도 값을 저장해서 살릴 수 있는 방법
  - useState 사용!

```jsx
// 처음에 이 컴포넌트가 렌더링될 때
// 아무 것도 입력되지 않았으므로 빈 값 저장
const [enteredTitle, setEnteredTitle] = useState("");
const titleChangeHandler = (event) => {
  setEnteredTitle(event.target.value);
};
```
- useState 사용
  - 두 요소를 얻기 위해 구조 분해 할당 사용
    - 첫 번째 요소(현재 값)로 enteredTitle 입력
    - 두 번째 요소는 상태를 업데이트 하는 함수 setEnteredTItle
  -  사용자가 무언가를 입력하는 이벤트에 반응할 때 setEnteredTitle을 호출해서 event.target.value를 전달할 수 있다.
  
  > setEnteredTitle(event.target.value);
  - 매개변수, 인자로 현재 입력된 값을 setEnteredTItle에 전달할 수 있고, 저장될 것이다.
  - 컴포넌트를 업데이트하기 위해 이러한 작업을 하는 것이 아니라 상태를 업데이트 할 때 항상 업데이트될 것이다.
    - 컴포넌트 함수의 수명 주기와는 별개인 어떤 변수에 저장하고 있다.
    - 이 컴포넌트 함수가 얼마나 자주 다시 실행되는 지에 상관없이 이 상태는 저장되고 살아있다.
  
  - 하나 이상의 state를 다루기 위해선 useState를 한 번 이상 호출하면 된다.
  - 여러 state, 여러 개의 state 조각들 또는 컴포넌트 별로 state 조각들을 가질 수 있다.
  - 같은 컴포넌트 안에 있는 모든 state 들은 완전히 별개이다.
  
  > const [enteredTitle, setEnteredTitle] = useState("");
  - useState를 다시 호출해서 빈 문자열을 저장할 때, 숫자형이 아닌 문자열로 저장한다.
    - 기본적으로 입력에 대해 변경 이벤트를 수신할 때마다 입력 요소값을 읽는다면 그것은 항상 문자열이기 때문
  
