#### Contents:
1. Rendering lists of data
2. Using Stateful Lists
3. Understanding Keys
4. Outputting Conditional Content
5. Adding Conditional Return Statements
6. Demo App - Adding a chart
7. Adding Dynamic Styles
8. Wrapup & next steps
`````````````````````````````````````````````````````````````1
#### Rendering lists of data:
`{props.items.map()}`
* map() - creates a new array based on another array, and that transforms every element in that original array.
* map() takes a function which we pass as an argument, and that function is then executed for every item in the array on which we're calling the map and the result of this function is an element which will be added to the newly created array.

```js
// Expenses.js
{props.items.map(expense => 
    <ExpenseItem
        title = {expense.title}
        amount = {expense.amount}
        date = {expense.date}
    />
)}
```

#### 2. Using Stateful Lists:
* This is the complete dynamic method of adding expenses.
```js
// App.js
import Expenses from './components/Expenses/Expenses';
import React, {useState} from 'react';
import NewExpense from './components/Expenses/NewExpense/NewExpense';

const DUMMY_EXPENSES = [
  {
    id: 'e1',
    title: 'Toilet Paper',
    amount: 194.12,
    date: new Date(2020, 7, 14),
  },
  { 
    id: 'e2', 
    title: 'New TV',
    amount: 13799.49,
    date: new Date(2021, 2, 12) 
  },
  {
    id: 'e3',
    title: 'Car Insurance',
    amount: 5294.67,
    date: new Date(2021, 2, 28),
  },
  {
    id: 'e4',
    title: 'New Desk (Wooden)',
    amount: 3450,
    date: new Date(2021, 5, 12),
  },
]; 
function App() {
  const [expenses, setExpenses] = useState(DUMMY_EXPENSES);
  const addExpenseHandler = (expense) => {
    setExpenses(prevExpenses => {
      return [expense, ...prevExpenses]
    })
  }
  return (
    <div>
      <NewExpense onAddExpense={addExpenseHandler}/>
      <Expenses items={expenses}/>
    </div>
  );
}
export default App;

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
      {props.items.map(expense => 
        <ExpenseItem
          title = {expense.title}
          amount = {expense.amount}
          date = {expense.date}
        />
      )}
    </Card>
  );
}
export default Expenses;
```

#### 3. Understanding Keys:
`Warning: Each child in a list should have a unique "key" prop.`
```js
// Expenses.js
{props.items.map(expense => 
    <ExpenseItem
        key = {expense.id}
        title = {expense.title}
        amount = {expense.amount}
        date = {expense.date}
    />
)}
```

#### Working with lists:
* Filtering expenses with Year

```js
// Expenses.js
const filteredExpenses = props.items.filter(expense => {
    return expense.date.getFullYear().toString() === filteredYear;
})
return (
<Card className="expenses">
    <ExpensesFilter
    selectedYear = {filteredYear}
    onChangeFilter = {filterChangeHandler}
    />
    {filteredExpenses.map(expense => 
    <ExpenseItem
        key = {expense.id}
        title = {expense.title}
        amount = {expense.amount}
        date = {expense.date}
    />
    )}
</Card>
);
```

#### 4. Outputting Conditional Content:
* Our filter is working but we can actually select values where we have no data.
* And we might want to show an appropriate message in these cases.

* Conditional Content is about rendering different output under different conditions.

```js
// Expenses.js
{filteredExpenses.length === 0 ? <p>No Expenses Found</p> :    
    (filteredExpenses.map(expense => 
        <ExpenseItem
            key = {expense.id}
            title = {expense.title}
            amount = {expense.amount}
            date = {expense.date}
        />
    ))
}

// OR

{filteredExpenses.length === 0 && <p>No Expenses Found</p>}
{filteredExpenses.length > 0 &&
    (filteredExpenses.map(expense => 
        <ExpenseItem
        key = {expense.id}
        title = {expense.title}
        amount = {expense.amount}
        date = {expense.date}
        />
    ))
}

// OR
const Expenses = (props) => {
  const [filteredYear, setFilteredYear] = useState('2020');
  const filterChangeHandler = (selectedYear) => {
    setFilteredYear(selectedYear);
  }
  const filteredExpenses = props.items.filter(expense => {
    return expense.date.getFullYear().toString() === filteredYear;
  })
  let expensesContent = <p>No Expenses Found</p>;
  if (filteredExpenses.length > 0) {
    expensesContent = filteredExpenses.map(expense => 
      <ExpenseItem
        key = {expense.id}
        title = {expense.title}
        amount = {expense.amount}
        date = {expense.date}
      />
    )
  }
  return (
    <Card className="expenses">
      <ExpensesFilter
        selectedYear = {filteredYear}
        onChangeFilter = {filterChangeHandler}
      />
      {expensesContent}
    </Card>
  );
}
export default Expenses;
```

#### Exercise:
* We're working on a part of a web app that's responsible for showing a warning when a user is about to perform a dangerous action.
* Therefore, our task is to conditionally show a warning box once a user has clicked a specific button. 
* Inside that warning dialog, another button allows users to dismiss the warning (i.e., remove the warning box from the screen).

```js
import React from 'react';

// don't change the Component name "App"
export default function App() {
    const [isDeleting, setIsDeleting] = React.useState(false);
    const deleteHandler = () => {
        setIsDeleting(true);
    }
    const proceedHandler = () => {
        setIsDeleting(false);
    }
    let alertHandler;
    if (isDeleting) {
        alertHandler = (
        <div id="alert">
          <h2>Are you sure?</h2>
          <p>These changes can't be reverted!</p>
          <button onClick={proceedHandler}>Proceed</button>
        </div>)
    }
    return (
        <div>
            {alertHandler}
            <button onClick={deleteHandler}>Delete</button>
        </div>    
    );
}
```

#### 5. Adding Conditional Return Statements:
```js
// NewExpense.js
import React, {useState} from "react";
import ExpenseForm from "./ExpenseForm";
import './NewExpense.css';
const NewExpense = (props) => {
    const [newExpense, setNewExpense] = useState(false);
    const addExpenseHandler = () => {
        setNewExpense(true);
    }
    const cancelHandler = () => {
        setNewExpense(false);
    }
    const saveExpenseDataHandler = (enteredExpenseData) => {
        const expenseData = {
            ... enteredExpenseData,
            id: Math.random().toString()
        };
        props.onAddExpense(expenseData);
        setNewExpense(false);
    }
    return(
        <div className="new-expense">
            {!newExpense && <button onClick={addExpenseHandler}>Add New Expense </button>}
            {newExpense && 
                <ExpenseForm 
                    onSaveExpenseData={saveExpenseDataHandler} 
                    onCancel={cancelHandler}
                />
            }
        </div>
    )
}
export default NewExpense;

// ExpenseForm.js
import React, {useState} from "react";
import './ExpenseForm.css';

const ExpenseForm = (props) => {
    const [enteredTitle, setEnteredTitle] = useState('');
    const [enteredAmount, setEnteredAmount] = useState('');
    const [enteredDate, setEnteredDate] = useState('');

    // const [userInput, setUserInput] = useState({
    //     enteredTitle: '',
    //     enteredAmount: '',
    //     enteredDate: '',
    // })

    const titleChangeHandler = (event) => {
        // setUserInput({
        //     ...userInput,
        //     enteredTitle: event.target.value,
        // })

        // setUserInput((prevState) => {
        //     return { ...prevState, enteredTitle: event.target.value}
        // })

        setEnteredTitle(event.target.value)              
    };

    const amountChangeHandler = (event) => {
        setEnteredAmount(event.target.value)
    };
    const dateChangeHandler = (event) => {
        setEnteredDate(event.target.value)
    }
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
                <button type="button" onClick={props.onCancel}>Cancel</button>
                <button type="submit" >Add Expense</button>
            </div>
        </form>
    )
}
export default ExpenseForm;
```

#### 6. Demo App - adding a chart:
```js
// Chart.js
import React from "react";
import './Chart.css';
import ChartBar from "./ChartBar";

const Chart = () => {
    return(
        <div className="chart">
            {props.dataPoints.map(dataPoint => (
                <ChartBar
                    key={dataPoint.label}
                    value = {dataPoint.value}
                    maxValue = {null}
                    label = {dataPoint.label}
                />
            ))}
        </div>
    )
};
export default Chart;
```

#### 7. Adding Dynamic Styles:
```js
// ChartBar.js
import React from "react";
import './ChartBar.css';
const ChartBar = (props) => {
    let barFillHeight = '0%';
    if (props.max > 0) {
        barFillHeight = Math.round((props.value / props.max) * 100) + '%'
    }
    return(
        <div className="chart-bar">
            <div className="chart-bar-inner">
                <div 
                    className="chart-bar-fill" 
                    style={{height: barFillHeight}}
                ></div>
                <div className="chart-bar-label"></div>
            </div>
        </div>
    )
};
export default ChartBar;
```

#### 8. Wrapup & nextsteps:
```js
// ExpenseChart.js
import React from "react";
import Chart from "../Chart/chart";

const ExpenseChart = () => {
    const chartDataPoints = [
        {label: 'Jan', value: 0},
        {label: 'Feb', value: 0},
        {label: 'Mar', value: 0},
        {label: 'Apr', value: 0},
        {label: 'May', value: 0},
        {label: 'Jun', value: 0},
        {label: 'Jul', value: 0},
        {label: 'Aug', value: 0},
        {label: 'Sep', value: 0},
        {label: 'Oct', value: 0},
        {label: 'Nov', value: 0},
        {label: 'Dec', value: 0},
    ];
    for (const expense of props.expenses) {
        const expenseMonth = expense.date.getMonth(); 
        // Starting at 0 => January = 0
        chartDataPoints[expenseMonth].value += expense.amount;
    }
    return <Chart dataPoints={chartDataPoints}/>
};
export default ExpenseChart;

// Chart.js
import React from "react";
import './Chart.css';
import ChartBar from "./ChartBar";

const Chart = () => {
    const dataPointValues = props.dataPoints.map(dataPoint => dataPoint.value);
    // Changing the dataPoint array to a number
    // map returns a new array of numbers

    const totalMaximum = Math.max(...dataPointValues);
    // dataPointValues is still an array
    // We can use the spread operator to pull out all the array elements
    // and add them as stand alone arguments
    // Now, max method recieves 12 arguments
    return(
        <div className="chart">
            {props.dataPoints.map(dataPoint => (
                <ChartBar
                    key={dataPoint.label}
                    value = {dataPoint.value}
                    maxValue = {null}
                    label = {dataPoint.label}
                />
            ))}
        </div>
    )
};
export default Chart;

// Expenses.js
<Card className="expenses">
    <ExpensesFilter
    selectedYear = {filteredYear}
    onChangeFilter = {filterChangeHandler}
    />
    <ExpenseChart expenses={filteredExpenses}/>
    <ExpensesList items={filteredExpenses}/>
</Card>
```
