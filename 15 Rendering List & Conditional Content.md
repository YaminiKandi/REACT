#### Contents:
1. Rendering lists of data
2. Using Stateful Lists

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


