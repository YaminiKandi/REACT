### React State & Working with events:
1. Listening to Events and Working with Event Handlers
2. How component functions are executed
3. How actually react parses the JSX code
4. Working with State
5. Adding Form Inputs.
6. Listening to user inputs
7. Working with multiple states
8. Using one state instead
9. Updating the state that depends on the previous state
10. Handling Form Submission
11. Adding Two-way Binding
12. Child-to-Parent Component Communication (Bottom-up)
13. Lifting the stateup

#### 1. Listening to Events and Working with Event Handlers:
```js
// ExpenseItem.js
function ExpenseItem(props) {
  const clickHandler = () => {
    console.log('Clicked!');
  }
  return (
    <Card className='expense-item'>
      <ExpenseDate date={props.date} />
      <div className='expense-item__description'>
        <h2>{props.title}</h2>
        <div className='expense-item__price'>Rs. {props.amount}</div>
      </div>
      <button onClick={clickHandler}>Change Title</button>
    </Card>
  );
}
```

`<button onClick={clickHandler}>Change Title</button>`
* we dont execute clickHandler here `clickHandler()` whether we use a normal function syntax or a pointer function.
* If we add parenthesis, Javascript would execute this when that line or code is being parsed.
* That means, it executes when the JSX Code is returned rather than executing when clicked. And that would be too early.
* So we pass a pointer to the `onClick`.

#### 2. How component functions are executed:
```js
function ExpenseItem(props) {
  let title = props.title;
  const clickHandler = () => {
    title = 'Updated!!';
  };
  return (
    <Card className='expense-item'>
      <ExpenseDate date={props.date} />
      <div className='expense-item__description'>
        <h2>{title}</h2>
        <div className='expense-item__price'>Rs. {props.amount}</div>
      </div>
      <button onClick={clickHandler}>Change Title</button>
    </Card>
  );
}
```
* We dont see any change in title, but the clickHandler function is being triggered

```js
let title = props.title;
const clickHandler = () => {
  title = 'Updated!!';
  console.log(title);
};
```
* Now we can see the change in the console.
* Even after the title is changed, we dont see that reflected in our DOM when we re outputting the title in h2.

##### 3. How actually react parses the JSX code:
```js
// ExpenseItem.js
function ExpenseItem(props) {
  let title = props.title;
  const clickHandler = () => {
    title = 'Updated!!';
    console.log(title);
  };
  return (
    <Card className='expense-item'>
      <ExpenseDate date={props.date} />
      <div className='expense-item__description'>
        <h2>{title}</h2>
        <div className='expense-item__price'>Rs. {props.amount}</div>
      </div>
      <button onClick={clickHandler}>Change Title</button>
    </Card>
  );
}
```

* Our component is just a regular function, the only special thing is it returns JSX.
* Since it is a function we have to call it and we know that we never called our component functions.
* Instead we use these functions like HTML elements in this JSX code.

```js
// Expenses.js
<ExpenseItem
  title={props.items[0].title}
  amount={props.items[0].amount}
  date={props.items[0].date}
/>
```
* Under the hood, this is almost like a function call.
* So, react goes through all the components, executes all the component functions and draws something on to the screen.
* The only problem with that is, react never repeats that.
* React goes through all of that when the application is intially rendered, How ever we sometimes what to update whats visible on the screen
* For example, because a button was clicked and that button should change some text which is being output.
* so we need a way of telling react that something changed and that a certain component should be re-evaluated and thats where react introduces a special concept called `state`.

#### 4. Working with State:
* `useState` - this function allows us to define the values as state where changes to these values should reflect in the component function being called again.
* useState is called a react hook.
* All the hooks start with the name `use..` and all these hooks must only be called inside of react component functions like ExpenseItem
* useState cant be used as `useState()`, it requires a default state value as with useState we basically create a special kind of variable, a variable where changes will lead this component function to be called again.

```js
function ExpenseItem(props) {
  useState(props.title);
  ....
}
```

* `useState` actually returns an array where the first value is variable itself, and the second value is that the updating function.
* We can use modern Javascript feature `array destructing` to store the both elements in separate variables or constants.

```js
function ExpenseItem(props) {
  const [title, setTitle] = useState(props.title);
  const clickHandler = () => {
    setTitle('Updated!!')
    console.log(title);
  };
}
```
* Why do we have this state updating function instead of assigning a new value. The reason for this is calling a function doesnot just assign a new value to some variable, but that instead it is a special variable to begin with.
* When we call this state updating function, this special variable will not just receive a new value but the component will be executed again.
* Even if we create a component more than once, the states will be separate.

```js
function ExpenseItem(props) {
  const [title, setTitle] = useState(props.title);
  console.log('ExpenseItem evaluated by React')
  ...
}
```
```
<!-- Output -->
<!-- when we load the page -->
ExpenseItem evaluated by React
ExpenseItem evaluated by React
ExpenseItem evaluated by React
ExpenseItem evaluated by React
<!-- when we click a button -->
Toilet Paper
ExpenseItem evaluated by React
ExpenseItem evaluated by React
```

#### Exercise 1 - Listening to Events:
* Our task is to add an event listener to listen for clicks on the button that's already included in the App component. Upon a button click, the price should change from Rs.100 to Rs.75.
* Add a state value to the existing App component function and make sure the state value is both updated upon button clicks and output as part of the JSX code.
* When using React Hooks like useState(), make sure to use them via `React.useState()`

```js
import React from 'react';
import './styles.css';
export default function App() {
    const [price, setPrice] = React.useState(100);
    const clickHandler = () => {
        setPrice('75')
    }
    return (
        <div>
            <p>${price}</p>
            <button onClick={clickHandler}>Apply Discount</button>
        </div>
    );
}
```
```css
body {
    font-family: sans-serif;
    margin: 0;
    padding: 3rem;
    background-color: #2d2c2c;
    color: #959090;
}
```

#### 5. Adding Form Inputs:
* Creating a new folder in components\Expenses and Adding this to `App.js`
```js
// NewExpense.js
import React from "react";
import ExpenseForm from "./ExpenseForm";
import './NewExpense.css';
const NewExpense = () => {
    return(
        <div className="new-expense">
            <ExpenseForm></ExpenseForm>
        </div>
    )
}
export default NewExpense;

// ExpenseForm.js
import React from "react";
import './ExpenseForm.css';

const ExpenseForm = () => {
    return(
        <form>
            <div className="new-expense-controls">
                <div className="new-expense-control">
                    <label>Title</label>
                    <input type="text"/>
                </div>
                <div className="new-expense-control">
                    <label>Amount</label>
                    <input type="text" min="0.01" step="0.01"/>
                </div>
                <div className="new-expense-control">
                    <label>Date</label>
                    <input type="text" min="2020-01-01" max="2024-12-31"/>
                </div>
            </div>
            <div className="new-expense-actions">
                <button type="submit">Add Expense</button>
            </div>
        </form>
    )
}
export default ExpenseForm;
```
```css
/* NewExpense.css */
.new-expense {
    background-color: #a892ee;
    padding: 1rem;
    margin: 2rem auto;
    width: 50rem;
    max-width: 95%;
    border-radius: 12px;
    text-align: center;
    box-shadow: 0 1px 8px rgba(0, 0, 0, 0.25);
}

.new-expense button {
    font: inherit;
    cursor: pointer;
    padding: 1rem 2rem;
    border: 1px solid #40005d;
    background-color: #40005d;
    color: white;
    border-radius: 12px;
    margin-right: 1rem;
}

.new-expense button:hover,
.new-expense button:active {
    background-color: #510674;
    border-color: #510674;
}

.new-expense button.alternative {
    color: #220131;
    border-color: transparent;
    background-color: transparent;
}

.new-expense button.alternative:hover,
.new-expense button.alternative:active {
    background-color: #ddb3f8;
}

/* ExpenseForm.css */
.new-expense-controls {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    margin-bottom: 1rem;
    text-align: left;
}

.new-expense-control label {
    font-weight: bold;
    margin-bottom: 0.5rem;
    display: block;
}

.new-expense-control input {
    font: inherit;
    padding: 0.5rem;
    border-radius: 6px;
    border: 1px solid #ccc;
    width: 20rem;
    max-width: 100%;
}

.new-expense-actions {
    text-align: right;
}
```

#### 6. Listening to User input:
```js
const ExpenseForm = () => {
    const titleChangeHandler = (event) => {
        console.log(event);                    
    };
}
....
<input type="text" onChange={titleChangeHandler} />
```
* When we start writing in title input, we'll get an object in console.
```js
SyntheticBaseEvent {_reactName: 'onChange', _targetInst: null, type: 'change', nativeEvent: InputEvent, target: input, …}
```

```js
const ExpenseForm = () => {
    const titleChangeHandler = (event) => {
        console.log(event.target.value);                    
    };
}
....
<input type="text" onChange={titleChangeHandler} />

// Output for title input - yam
y
ya
yam
```

#### 7. Working with multiple States:
```js
const [enteredTitle, setEnteredTitle] = useState('');
const titleChangeHandler = (event) => {
    setEnteredTitle(event.target.value);                    
};

const [enteredAmount, setEnteredAmount] = useState('');
const amountChangeHandler = (event) => {
    setEnteredAmount(event.target.value)
};

const [enteredDate, setEnteredDate] = useState('');
const dateChangeHandler = (event) => {
    setEnteredDate(event.target.value)
}
```

#### 8. Using one state instead:
```js
const [userInput, setUserInput] = useState({
    enteredTitle: '',
    enteredAmount: '',
    enteredDate: '',
})
const titleChangeHandler = (event) => {
    // setEnteredTitle(event.target.value);
    setUserInput({
        enteredTitle: event.target.value,
    })               
};
```
* If we set our new userinput state, we would basically dump the other keys.
* Because when we update our state, react will not merge this with old state. It will simply replace the old state with the new one.
* And if our new one is an object with one key value pair, the old state will be replaced and Therefore the other two key value pairs ie., amount and date would be lost.
* For the data not be lost, we should manually need to copy the other values which we're not updating here

```js
const titleChangeHandler = (event) => {
    setUserInput({
        ...userInput,
        enteredTitle: event.target.value,
    })               
};
```

#### 9. Updating State that depends on the Previous State:
* In many cases, both will work fine.
```js
// This will be the safe way
const titleChangeHandler = (event) => {
    setUserInput((prevState) => {
        return { ...prevState, enteredTitle: event.target.value}
    })               
};
// OR
const titleChangeHandler = (event) => {
    setUserInput({
        ...userInput,
        enteredTitle: event.target.value,
    })               
};
```

#### Exercise 2 - Using State with Form Inputs:
* We're working on a text messaging app and our task is to validate the text entered by a user whilst the user is typing.
* If it's at least 3 characters long, the text "Valid message" should be displayed below the input field. If it's invalid (i.e., shorter than 3 characters), the text "Invalid message" should be displayed.

```js
import React from 'react';
import './styles.css';
export default function App() {
    const [msgValidity, setMsgValidity] = React.useState('Invalid');
    const msgChangeHandler = (event) => {
       const value = event.target.value
       if (value.trim().length < 3) {
           setMsgValidity('Invalid')
       } else {
           setMsgValidity('Valid');
       }
    }
    return (
        <form>
            <label>Your message</label>
            <input type="text" onChange={msgChangeHandler}/>
            <p>{msgValidity} message</p>
        </form>
    );
}
```
```css
body {
    font-family: sans-serif;
    margin: 0;
    padding: 3rem;
    background-color: #2d2c2c;
    color: #959090;
}
label {
    display: block;
    margin-bottom: 0.5rem;
}
input {
    font: inherit;
    padding: 0.5rem;
    background-color: #474545;
    border: none;
    border-radius: 4px;
    color: white;
}
```

#### Exercise 3 - Updating State based on Older State:
* 

```js
import React from 'react';
import './styles.css';
export default function App() {
    const [counter, setCounter] = React.useState(0);
    const incrementCounterHandler = () => {
        setCounter(prevCounter => prevCounter + 1);
    }
    return (
      <div>
        <p id="counter">{counter}</p>
        <button onClick={incrementCounterHandler}>Increment</button>
      </div>
    );
}
```
```css
body {
    font-family: sans-serif;
    margin: 0;
    padding: 3rem;
    background-color: #2d2c2c;
    color: #959090;
    text-align: center;
}

#counter {
    color: #d7adff;
    font-size: 3rem;
}
```

#### 10. Handling Form Submission:
```js
const submitHandler = () => {...}
return(
  <form onSubmit={submitHandler}>
  ....
    <button type="submit">Add Expense</button>
  </form>
)
```
* This is a default browser behaviour.
* If we click the button, the page reloads because the browser automatically sends a request when ever a form is submitted to the server which is hosting the webpage.
* This is not what we want, so we'll handle this form submission with JavaScript and manually collect and combine the data.

* We can disable or prevent this default behaviour.
```js
const submitHandler = (event) => {
    event.preventDefault();
}
```
* Getting the data
```js
const submitHandler = (event) => {
    event.preventDefault();
    const expenseData = {
        title: enteredTitle,
        amount: enteredAmount,
        date: new Date(enteredDate)
    };
    console.log(expenseData);
}
// Output
{title: 'testing', amount: '200', date: Sun Jan 02 2022 05:30:00 GMT+0530 (India Standard Time)}
```

#### 11. Adding two-way binding:
* After submitting the input values, we need to clear them.
* For this part, we used state for storing the entered values. 
* We can also create variables globally but we used state and state has advantages, we can now implement two-way binding.
* Two-way binding - We simply just dont listen to changes, but we can also pass a new value back to the input. So we can reset or change the input programatically.

```js
const submitHandler = (event) => {
    event.preventDefault();
    const expenseData = {
        title: enteredTitle,
        amount: enteredAmount,
        date: new Date(enteredDate)
    };
    console.log(expenseData);
    setEnteredTitle('');
    setEnteredAmount('');
    setEnteredDate('');
    // Setting the data to empty
}
return(
    <form onSubmit={submitHandler}>
        <div className="new-expense-controls">
            <div className="new-expense-control">
                <label>Title</label>
                <input 
                    type="text" 
                    value={enteredTitle} 
                    onChange={titleChangeHandler}
                />
            </div>
            <div className="new-expense-control">
                <label>Amount</label>
                <input 
                    type="number"
                    min="0.01"
                    step="0.01"
                    value={enteredAmount}
                    onChange={amountChangeHandler}
                />
            </div>
            <div className="new-expense-control">
                <label>Date</label>
                <input 
                    type="date"
                    min="2020-01-01"
                    max="2024-12-31"
                    value={enteredDate}
                    onChange={dateChangeHandler}
                />
            </div>
        </div>
        <div className="new-expense-actions">
            <button type="submit">Add Expense</button>
        </div>
    </form>
)
```

#### 12. Child-to-Parent Component Communication:
```js
setEnteredTitle('');
setEnteredAmount('');
setEnteredDate('');
```
* The only problem is having this data is nice but we technically dont need it in the expense form component.
* Instead we need it in the NewExpense.js component or App.js component because there we have our expenses array and Ultimately our goal will be to add this new expense.
* So we need to pass the data from ExpenseForm to App
* We can do it from parent to child by using props (i.e., from Expenses to ExpenseItem)

```js
<input 
  type="text" 
  value={enteredTitle} 
  onChange={titleChangeHandler}
/>
```
* We can treat this input as a component aswell. Its not one of our components but it is simply a pre-built component, provided to us by react and translate it to the input DOM element but it has this component character in the end. And we can also set some props on that component like value and onChange.
* We can also replicate this pattern for our own components, We can create our own event props and we can expect functions as values and that would allow us to pass a function from a parent component to a child component and then call that function inside of the child component.
* And when we then call a function, we can ofcourse pass data to that function as a parameter and this is how we can communicate up from child to parent.
* Firstly we need to go to NewExpense and then to App component. we cant skip components in between.

##### step 1 - passing ExpenseData to NewExpense:
```js
// NewExpense.js
import React from "react";
import ExpenseForm from "./ExpenseForm";
import './NewExpense.css';
const NewExpense = () => {
    const saveExpenseDataHandler = (enteredExpenseData) => {
        const expenseData = {
            ...enteredExpenseData,
            id: Math.random().toString()
        };
        console.log(expenseData); // 10th line
    }
    return(
        <div className="new-expense">
            <ExpenseForm onSaveExpenseData={saveExpenseDataHandler}/>
            {/* Passing a pointer at a function */}
        </div>
    )
}
export default NewExpense;
```
##### Step2 - Using `saveExpenseDataHandler` function inside of our custom component
* This is the step we didnt have to do for inputs because these are built in components basically, but there also we pass a function to `onChange` and internally react will add a listener and call this function which we pass in whenever that event occurs.
* Since we're doing this on our own custom component, we aslo have to call the past and function manually

```js
// ExpenseForm.js
const ExpenseForm = (props) => {
  .....
  const submitHandler = (event) => {
      event.preventDefault();
      const expenseData = {
          title: enteredTitle,
          amount: enteredAmount,
          date: new Date(enteredDate)
      };
      props.onSaveExpenseData(expenseData);
      setEnteredTitle('');
      setEnteredAmount('');
      setEnteredDate('');
  }
} 
// Output
NewExpense.js:10 {title: 'test', amount: '200', date: Mon May 03 2021 05:30:00 GMT+0530 (India Standard Time), id: '0.9382838765379367'} 
```

##### Step3 - Communicating from NewExpense to App Component:
```js
// App.js
 const addExpenseHandler = (expense) => {
    console.log('In App.js');
    console.log(expense);
  }
  return (
    <div>
      <NewExpense onAddExpense={addExpenseHandler}/>
      <Expenses items={expenses} />
    </div>
  );

// NewExpense.js
const saveExpenseDataHandler = (enteredExpenseData) => {
    const expenseData = {
        ... enteredExpenseData,
        id: Math.random().toString()
    };
    props.onAddExpense(expenseData);
}

// Output
In App.js
App.js:34 {title: 'test', amount: '200', date: Wed Feb 01 2023 05:30:00 GMT+0530 (India Standard Time), id: '0.8974226397232379'}
```

#### 13. Lifting the stateup:
* We need to hand over the data from NewExpense to Expenses but that doesn't work, because we have no direct connection between two sibling components.
* Instead We can only communicate from parent to child and from child to parent.
* So we the choose the closest parent component which has direct or indirect access to both involved components. Here its `App.js` 

#### Assignment:
* Adding ExpensesFilter component.
* Styling css
```css
/* ExpensesFilter.css */
.expenses-filter {
    color: white;
    padding: 0 1rem;
}
.expenses-filter-control {
    display: flex;
    width: 100%;
    align-items: center;
    justify-content: space-between;
    margin: 1rem 0;
}
.expenses-filter label {
    font-weight: bold;
    margin-bottom: 0.5rem;
}
.expenses-filter select {
    font: inherit;
    padding: 0.5rem 3rem;
    font-weight: bold;
    border-radius: 6px;
}
```

* Step1 -  Basic component
```js
import React, {useState} from "react";
import './ExpensesFilter.css';

const ExpensesFilter = () => {
    const dropdownChangeHandler = (event) => {
        console.log(event.target.value)
    }
    return(
        <div className="expenses-filter">
            <div className="expenses-filter-control">
                <label>Filter by Year</label>
                <select onChange={dropdownChangeHandler}>
                    <option value='2024'>2024</option>
                    <option value='2023'>2023</option>
                    <option value='2022'>2022</option>
                    <option value='2021'>2021</option>
                    <option value='2020'>2020</option>
                </select>
            </div>
        </div>
    )
}
export default ExpensesFilter;
```
* Here by changing the dropdown, we will get the selected year in console.

* Step2 - adding ExpensesFilter component to Expenses component

```js
// Expenses.js
const Expenses = (props) => {
  const filterChangeHandler = (selectedYear) => {
    console.log('Expenses.js');
    console.log(selectedYear);
  }
  return (
    <Card className="expenses">
      <ExpensesFilter
        onChangeFilter = {filterChangeHandler}
      />
      ....
    </Card>
  );
}

// ExpenseFilter.js
const ExpensesFilter = (props) => {
    const dropdownChangeHandler = (event) => {
        props.onChangeFilter(event.target.value)
    }
    ...
}

// Output
Expenses.js
2021
```

```js
// ExpensesFilter.js

import React, {useState} from "react";
import './ExpensesFilter.css';

const ExpensesFilter = (props) => {
    const dropdownChangeHandler = (event) => {
        props.onChangeFilter(event.target.value)
    }
    return(
        <div className="expenses-filter">
            <div className="expenses-filter-control">
                <label>Filter by Year</label>
                <select value={props.selectedYear} onChange={dropdownChangeHandler}>
                    <option value='2024'>2024</option>
                    <option value='2023'>2023</option>
                    <option value='2022'>2022</option>
                    <option value='2021'>2021</option>
                    <option value='2020'>2020</option>
                </select>
            </div>
        </div>
    )
}
export default ExpensesFilter;
 
// Expenses.js
import { useState } from 'react';
import ExpenseItem from './ExpenseItem';
import Card from '../UI/Card';
import './Expenses.css';
import ExpensesFilter from './ExpensesFilter';
 
const Expenses = (props) => {
  const [filteredYear, setFilteredYear] = useState('2020');
  const filterChangeHandler = (selectedYear) => {
    setFilteredYear(selectedYear);
  }
  return (
    <Card className="expenses">
      <ExpensesFilter
        selectedYear = {filteredYear}
        onChangeFilter = {filterChangeHandler}
      />
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
    </Card>
  );
}
export default Expenses;
```

#### Quiz 2:
1. How should we NOT listen to events when working with React?
    * Adding an event listener (eg: via "addEventListener") manually.
    * Instead we listen to events by setting an event handler function via props (eg: onClick={...})

2. Which value should we pass to event listener props like onClick?
    * A pointer at the function that should execute when the event occurs.

3. How can we communicate from one of your components to a parent (i.e. higher level) component?
    * We can accept a function via props and call it from inside the lower level (child) component to then trigger some action in parent component (which passed the function)
    * In JavaScript, functions are just objects (i.e. regular values) and hence we can pass them as values via props to a component. If that component then calls that function, it executes - and that's how you can trigger a function defined in a parent component from inside a child component.

4. How can we change what a component displays on the screen?
    * Create some 'state' value (via useState) which we can then change and output in JSX.

5. Why do we need this extra "state" concept instead of regular JS variables which we change and use?
    * Because standard JS variables doesn't cause react components to be re-evaluated.

6. Which statement about useState is NOT correct?
    a. It receives an (optional) initial state value as an argument.
    b. Calling useState again will update the state value.
    c. It returns an array with exactly two elements.
Ans. b
* This statement is wrong. Calling useState again will simply create a new state.

7. How can we update component state (created via useState)?
    * we can call the state updating function which useState also returned.
    * useState returns an array with exactly two elements - the second element is always a function which you can call to set a new value for your state. Calling that function will then also trigger React to re-evaluate the component.

8. How much state may you manage in one single component?
    * We can have as many state slices as we need/want.
    * There's no restriction.

9. What's wrong about this code snippet?
    ```js
    const [counter, setCounter] = useState(1);
    ...
    setCounter(counter + 1);
    ```
    * If we update state that depends on the previous state, we should use the 'function form' of the state updating function instead.