#### Outputing Dynamic Data:
```js
// ExpenseItem.js
import React from 'react';
import './ExpenseItem.css';

function ExpenseItem() {
    const expenseDate = new Date(2023, 1, 2);
    const expenseTitle = 'Car Insurance';
    const expensePrice = 20349.99;
    return(
        <div className='expense-item'>
            <div>{expenseDate.toISOString()}</div>
            <div className='expense-item-description'>
                <h2>{expenseTitle}</h2>
            </div>
            <div className='expense-item-price'>Rs. {expensePrice}</div>
        </div>
    )
}
export default ExpenseItem;
```

```css
// ExpenseItem.css
.expense-item{
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #4b4b4b;
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.25);
    padding: 8px;
    margin: 16px 0;
}
.expense-item-description{
    display: flex;
    flex-direction: column;
    flex-flow: column-reverse;
    align-items: flex-end;
    justify-content: flex-start;
    flex: 1;
    gap: 16px;
}
.expense-item h2{
    color: #3a3a3a;
    flex: 1;
    color: white;
    font-size: 16px;
    margin: 0 16px;
}
.expense-item-price{
    font-size: 16px;
    font-weight: bold;
    color: white;
    background-color: #40005d;
    border:1px solid white;
    border-radius: 12px;
    padding: 8px;
}
@media (min-width: 580px) {
    .expense-item-description {
      flex-direction: row;
      align-items: center;
      justify-content: flex-start;
      flex: 1;
      font-size: 20px;
    }
    .expense-item__price {
      padding: 8px 22px;
    }
}
```

#### Passing Data via props:

```js
// <App />
goalItem = 'Finish';
// <CourseGoalItem />
<li>{goalItem}</li>
```
* Here, the `goalItem` should be output dynamically.
* The problem is that the goalItem variable lives in the App Component, not in the CourseGoalItem component.
* To certain extent it is good because it makes the CourseGoalItem independent if it doesn't store the concrete value internally.
* But we dont have direct access to that variable i.e., Components can't just use data stored in other components.
* We can access by using props

```js
// <App />
goalItem = 'Finish';
// <CourseGoalItem text={goalItem}/>
<li>{props.text}</li>
```

```js
// ExpenseItem.js
import React from 'react';
import './ExpenseItem.css';

function ExpenseItem(props) {
    return(
        <div className='expense-item'>
            <div>{props.date.toISOString()}</div>
            <div className='expense-item-description'>
                <h2>{props.title}</h2>
            </div>
            <div className='expense-item-price'>Rs. {props.amount}</div>
        </div>
    )
}
export default ExpenseItem;

// App.js
import './App.css';
import ExpenseItem from './components/ExpenseItem';

function App() {
  const expenses = [
    {
      id: 'e1',
      title: 'Toilet Paper',
      amount: 94.12,
      date: new Date(2020, 7, 14),
    },
    { 
      id: 'e2', 
      title: 'New TV',
      amount: 799.49,
      date: new Date(2021, 2, 12) 
    },
    {
      id: 'e3',
      title: 'Car Insurance',
      amount: 294.67,
      date: new Date(2021, 2, 28),
    },
    {
      id: 'e4',
      title: 'New Desk (Wooden)',
      amount: 450,
      date: new Date(2021, 5, 12),
    },
  ];
  return (
    <div className="App">
      <ExpenseItem
        title = {items[0].title}
        amount = {items[0].amount}
        date = {items[0].date}
      ></ExpenseItem>
      <ExpenseItem
        title = {items[1].title}
        amount = {items[1].amount}
        date = {items[1].date}
      ></ExpenseItem>
      <ExpenseItem
        title = {items[2].title}
        amount = {items[2].amount}
        date = {items[2].date}
      ></ExpenseItem>
      <ExpenseItem
        title = {items[3].title}
        amount = {items[3].amount}
        date = {items[3].date}
      ></ExpenseItem>
      
    </div>
  );
}
export default App;
```

#### Composition:
* Building a user interface from smaller building blocks is called 'Composotion'
* Example - `App.js -> Expense.js -> ExpenseItem.js -> ExpenseDate.js & Card.js`

```js
// App.js
import './App.css';
import Expenses from './components/Expenses';

function App() {
  const expenses = [
    {
      id: 'e1',
      title: 'Toilet Paper',
      amount: 94.12,
      date: new Date(2020, 7, 14),
    },
    { 
      id: 'e2', 
      title: 'New TV',
      amount: 799.49,
      date: new Date(2021, 2, 12) 
    },
    {
      id: 'e3',
      title: 'Car Insurance',
      amount: 294.67,
      date: new Date(2021, 2, 28),
    },
    {
      id: 'e4',
      title: 'New Desk (Wooden)',
      amount: 450,
      date: new Date(2021, 5, 12),
    },
  ];
  return (
    <div className="App">
      <Expenses items={expenses}/>
    </div>
  );
}
export default App;

// Expense.js
import ExpenseItem from "./ExpenseItem";
function Expenses(props) {
    return(
        <div className="expenses">
            <ExpenseItem
                title = {props.items[0].title}
                amount = {props.items[0].amount}
                date = {props.items[0].date}
            />
            <ExpenseItem
                title = {props.items[1].title}
                amount = {props.items[1].amount}
                date = {props.items[1].date}
            />
            <ExpenseItem
                title = {props.items[2].title}
                amount = {props.items[2].amount}
                date = {props.items[2].date}
            />
            <ExpenseItem
                title = {props.items[3].title}
                amount = {props.items[3].amount}
                date = {props.items[3].date}
            />
        </div>
    )
}
export default Expenses;

// ExpenseItem.js
import './ExpenseItem.css';
import ExpenseDate from './ExpenseDate';
import Card from './Card';

function ExpenseItem(props) {
    return(
        <Card className='expense-item'>
            <ExpenseDate date={props.date}/>
            <div className='expense-item-description'>
                <h2>{props.title}</h2>
            </div>
            <div className='expense-item-price'>Rs. {props.amount}</div>
        </Card>
    )
}
export default ExpenseItem;

// Card.js
import './Card.css';
function Card(props) {
    return(
        <div className="card">{props.children}</div>
        /*
        children is a reserved name
        The value of this special children prop will 
        always be the content between the opening and 
        closing tags of our custom component
        */
    )
}
export default Card;

// ExpenseDate.js
import './ExpenseItem.css';

function ExpenseDate(props) {
    const month = props.date.toLocaleString('en-US', { month: 'long'});
    const day = props.date.toLocaleString('en-US', {day: '2-digit'});
    const year = props.date.getFullYear();
    return(
        <div className="expense-date">
            <div className="expense-date-month">{month}</div>
            <div className="expense-date-year">{year}</div>
            <div className="expense-date-day">{day}</div>
        </div>
    )
}
export default ExpenseDate;
```

#### Understanding JSX:
```js
// JSX
return (
  <div>
    <h2>Let's get started!</h2>
    <Expenses items={expenses} />
  </div>
);
// JSX under the hood
return React.createElement(
  'div',
  {},
  React.createElement('h2',{},"Let's get started!"),
  React.createElement(Expenses, {items: expenses})
)
```

#### An Alternate Function Syntax:
```js
function App() {
  const expenses = []
}
// Arrow Function
const App = () => {
  const expenses = []
}
```