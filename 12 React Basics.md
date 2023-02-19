### Contents:
1. React
2. Components 

#### React:
* React is a javascript library for building user interfaces.
* HTML, CSS and JS are about building user interfaces as well.
* React makes building complex, interactive and reactive user interfaces simpler.
* React is all about 'Components'.
* Because all user interfaces in the end are made up of components.

#### Components:
* Components are reusable building blocks in our user interface.
* They are combination of HTML code, CSS code for styling and JS code for logic.

##### Why Components?
1. Reusability (Don't repeat ourselves)
2. Separation of Concerns (Don't do too many things in one and the same place (function))

#### How is a Component built?
* React allows us to create re-usable and reactive components consisting HTML and JavaScript (and CSS)
* React uses a declarative approach for building these components
* Define the desired target state and let React figure out the actual JavaScript DOM Instructions.
* With React, we build a component tree with one root component that's mounted into a DOM node.

```js
import './App.css';
function App() {
  const para = document.createElement('p');
  para.textContent = 'This is also Visible!';
  document.getElementById('root').append(para);
//   this is how do u in a regular javascript
  return (
    <div className="App">
      <h2>Lets get started!</h2>
      <p>This is also visible</p>
    </div>
    // this is how reacts deals with the elements
  );
}
export default App;
```
* The above is following an imperative approach.
* Because, we are giving clear step by step instructions, what javascript and browser should be doing.


---

#### JSX
Its a special, non standard syntax which is enabled in React projects

---

```js
return React.createElement(
    'div',
    {},
    React.createElement('h2',{},"Let's get started!"),
    React.createElement(Expenses, {items: expenses})
  )
// OR
return (
  <div>
    <h2>Let's get started!</h2>
    <Expenses items={expenses} />
  </div>
);
```