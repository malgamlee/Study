# 'useState' 훅 자세히 살펴보기

- useState 는 몇몇 state를 등록한다.
- state 값이고 호출된 컴포넌트를 위한 값이다.
- 특정 컴포넌트의 인스턴스를 위해 state를 등록한다.

```jsx
import ExpenseItem from "./ExpenseItem";
import Card from "../UI/Card";
import "./Expense.css";

const Expense = (props) => {
  return (
    <Card className="expenses">
      <ExpenseItem
        title={props.items[0].title}
        amount={props.items[0].amount}
        date={props.items[0].date}
      />
      <ExpenseItem
        title={props.items[1].title}
        amount={props.items[1].amount}
        date={props.items[1].date}
      />
      <ExpenseItem
        title={props.items[2].title}
        amount={props.items[2].amount}
        date={props.items[2].date}
      />
      <ExpenseItem
        title={props.items[3].title}
        amount={props.items[3].amount}
        date={props.items[3].date}
      />
    </Card>
  );
};

export default Expense;
```

- Expenses.js 는 네 개의 ExpenseItem을 가지고 있다.
- 모든 아이템들은 별도의 스테이트를 갖는데 다른 상태와는 분리되어 있다.
- ExpenseItem을 네 번 생성했기 때문에 네 번 함수를 호출한다.
- 그리고 매번 호출할 때마다 동일한 방법으로 새로운 별도의 state가 생성되지만 리액트에 의해 독립적으로 관리된다
- 첫번째 ExpenseItem을 수정해도 다른 아이템들은 영향을 받지 않는다.
    - 자신들만의 state를 가지고 있기 때문
    - 컴포넌트별 인스턴스를 기반으로 해서 한 개 이상의 컴포넌트를 생성해도 독립적인 state를 갖는다.

```jsx
import React, { useState } from "react";

import "./ExpenseItem.css";
import ExpenseDate from "./ExpenseDate";
import Card from "../UI/Card";

const ExpenseItem = (props) => {
	// ******************
  const [title, setTItle] = useState(props.title);
	// ******************

  const clickHandler = () => {
    setTItle("updated!");
    console.log("click");
  };
  return (
    <Card className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
      <button onClick={clickHandler}>Change Title</button>
    </Card>
  );
};

export default ExpenseItem;
```

> const [title, setTItle] = useState(props.title);

- 새로운 값을 할당할 때 const 상수를 사용하는 이유?
    - 우선 등호를 이용해서 값을 할당하지 않는다.
    - state를 업데이트하는 함수 **(useState)** 를 호출하고, 구체적인 값은 리액트에 의해 어딘가에서 관리된다.
    - useState를 호출해서 리액트에게 어떤 값을 관리해야 한다고 선언한다.
    - 변수 자체를 볼 수는 없고 함수만 호출한다.

- 가장 최신의 title 값은 어떻게 가져오는가?
    - 컴포넌트형 함수에서는 state가 업데이트되면 재실행된다.
    - 위 코드도 컴포넌트형 함수가 다시 실행될 때마다 다시 실행된다.
    - 그래서 setTitle을 호출해서 새로운 title을 할당하면 이 컴포넌트를 다시 불러오게 되고 이 업데이트 된 title은 우리를 위해 state를 관리하는 리액트에서 가져온 것이다.
    - 기본적으로 리액트에게 관리하라고 했던 최신 제목 상태를 달라고 요구하면 리액트는 useState가 반환하는 배열에서 가장 최신의 상태를 우리에게 제공한다.
    - 그래서 컴포넌트가 재실행될 때마다 우리는 항상 가장 최신 상태의 화면을 갖는다.

- 특별히 리액트는 처음으로 주어진 컴포넌트 인스턴스에서 useState를 호출할 때 기록한다.
    - 그래서 우리가 처음 호출할 때 해당 인자를 초깃값으로 취한다.
    - 위 코드에서는 **props.title**
- 만약 컴포넌트가 그 때 재실행되면 상태가 변했기 때문에 리액트는 state를 다시 초기화하지는 않을 것이다.
    - 어떤 state가 업데이트 된 것에 기반해서 가장 최신 state를 우리에게 제공할 것이다.
- 이 초깃값은 주어진 컴포넌트 인스턴스에 대해 처음으로 이 컴포넌트형 함수가 실행될 때만 고려되는 값이다.

## 정리

- useState를 사용해서 상태를 등록하면 항상 두 개의 값을 얻는다. (현재 상태값, 업데이트하는 함수)
- 그리고 state가 변할 때마다 업데이트 함수를 호출한다.
- JSX 코드에서 그것을 출력하기 위해 상태값을 사용하고 싶을 때마다 첫 번째 요소를 사용한다.
- 그 다음 리액트가 나머지 작업을 한다.
    - 상태가 변할 때마다 컴포넌트형 함수를 다시 실행하고 JSX 코드를 다시 평가한다.
- 그것이 바로 state이고, state가 응용 프로그램에게 반응성을 추가하기 때문에 중요한 개념이다.
    - state가 없다면 사용자 인터페이스는 절대 바뀌지 않을 것이다.
- state를 보고 이벤트를 수신하면서 사용자 입력에 반응할 수 있다는 것을 확인한다.
    - 그런 입력은 화면에 가시적인 변화를 가져온다.
