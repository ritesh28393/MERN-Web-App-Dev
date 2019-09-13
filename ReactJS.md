**Contents of the React JS**
  1. [Introduction](#introduction)
  2. [JSX](#jsx)
  3. [Components](#components)
  4. [Props](#props)
  5. [State](#state)
  6. [LifeCycle - Mounting, Updating, UnMounting](#lifecycle)
  7. [Events](#events)
  8. [Forms](#forms)
  9. [CSS](#css)
***  
## Introduction
  * React is a JavaScript library for building UI components.
  * Instead of manipulating the browser's DOM directly, React creates a virtual DOM in memory, where it does all the necessary manipulating, before making the changes in the browser DOM.
  * React finds out what changes have been made, and changes only what needs to be changed.
  * The quickest way start learning React is to first include following 3 scripts and then write React directly in your HTML files. The first two let us write React code in our JavaScripts, and the third, Babel, allows us to write JSX syntax and ES6 in older browsers
    ```js
    <script src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
    ```
  * `ReactDOM.render(HTML_REACT_CODE, HTML_DOM_ELEMENT)`: Using this function, React renders HTML to the web page

## JSX
  * JSX stands for JavaScript XML. JSX allows us to write HTML in React. JSX makes it easier to write and add HTML in React.
  * JSX converts HTML tags into react elements
    * With JSX - `const myelement = <h1>I Love JSX!</h1>;`
    * Without JSX - `const myelement = React.createElement('h1', {}, 'I do not use JSX!');`
  * **Expressions in JSX:** With JSX you can write expressions inside curly braces { }, like `const myelement = <h1>React is {5 + 5} times better with JSX</h1>;`
  * **Inserting Large Block of HTML:** To write HTML on multiple lines, put the HTML inside ()
  * **One Top Level Element:** The HTML code must be wrapped in ONE top level element. So if you like to write two headers, you must put them inside a parent element, like a div element. 
  * **Error thrown in JSX:** JSX will throw an error if the HTML is not correct, or HTML elements are not properly closed, or if the HTML misses a parent element.

## Components
  * > ***In this tutorial we will only concentrate on the Class components, and not on the Function components***
  * Components serve the same purpose as JavaScript functions, but work in isolation and returns HTML via a render function.
  * **Naming Convention:** The component name must start with an upper case letter
  * **Create Component:** Create a component called Car, which returns a `<h2>` element -
    ```js
    class Car extends React.Component {
      render() {
        return <h2>Hi, I am a Car!</h2>;
      }
    }
    ```
  * **Use component:** `ReactDOM.render(<Car />, document.getElementById('root'));`
  * **Components in Components:** Use the **Car** component inside the **Garage** component
    ```js
    class Garage extends React.Component {
      render() {
        return (
          <div>
            <h1>Who lives in my Garage?</h1>
            <Car/>
          </div>
        );
      }
    }
    ReactDOM.render(<Garage />, document.getElementById('root'));
    ```
  * **Components in Files:** 
    * The file must start by importing React (as before), and it has to end with the statement export default Car;
    * Create a new file **App.js**
      ```js
      import React from 'react';
      import ReactDOM from 'react-dom';
      class Car extends React.Component {
        render() {
          return <h2>Hi, I am a Car!</h2>;
        }
      }
      export default Car;
      ```
    * Use the **Car** component after importing the **App.js** file in the application
      ```js
      import React from 'react';
      import ReactDOM from 'react-dom';
      import Car from './App.js';
      ReactDOM.render(<Car />, document.getElementById('root'));
      ```

## Props
  * Props are arguments passed to components via HTML attributes.
  * **Props are read-only!** You will get an error if you try to change their value.
  * 
    ```js
    class Car extends React.Component {
      render() {
        return <h2>I am a {this.props.brand}!</h2>
      }
    }
    const myelement = <Car brand="Ford" />;
    ReactDOM.render(myelement, document.getElementById('root'));
    ```
  * **Pass Data:** Props are also how you pass data from one component to another, as parameters
    ```js
    class Car extends React.Component {
      render() {
        return <h2>I am a {this.props.brand.model}!</h2>;
      }
    }
    class Garage extends React.Component {
      render() {
        const carinfo = {name: "Ford", model: "Mustang"};
        return (
          <div>
            <h1>Who lives in my garage?</h1>
            <Car brand={carinfo} />
          </div>
        );
      }
    }
    ReactDOM.render(<Garage />, document.getElementById('root'));
    ```

## State
  * React components has a built-in state object. The state object is where you store property values that belongs to the component.
  * The state object is initialized in the constructor
  * `setState()`: Use it to change a value in the state object. When the state object changes, the component re-renders.
    ```js
    class Car extends React.Component {
      constructor(props) {
       super(props);
       this.state = {
         brand: "Ford",
         model: "Mustang",
         color: "red",
         year: 1964
       };  
      }
      changeColor = () => {
        this.setState({color: "blue"});
      }
      render() {
        return (
          <div>
            <h1>My {this.state.brand}</h1>
            <p>It is a {this.state.color} {this.state.model} from {this.state.year}.</p>
            <button type="button" onClick={this.changeColor}>Change color</button>
          </div>
        );
      }
    }
    ```

## LifeCycle
  * Each component in React has a lifecycle which you can monitor and manipulate during its three main phases. The three phases are -
    > **Mounting**: constructor(), getDerivedStateFromProps(), render(), componentDidMount()
    > **Updating**:  getDerivedStateFromProps(), shouldComponentUpdate(), render(), getSnapshotBeforeUpdate(), componentDidUpdate()
    > **Unmounting**: componentWillUnmount()
  * **Mounting:** Putting elements into the DOM. React has four built-in methods that gets called, in this order, when mounting a component -
    1. `constructor()`: Natural place to set up the initial state and other initial values. It is called with the props, as argument, and you should always start by calling the `super(props);` before anything else
    2. `getDerivedStateFromProps()`: Natural place to set the state object based on the initial props. It returns an object with changes to the state.
    3. `render()`: **Mandatory**. This outputs HTML code to the DOM
    4. `componentDidMount()`: Place where you run statements after the component is already placed in the DOM.
        ```js
        class Header extends React.Component {
          constructor(props) {
            super(props);
            this.state = {favoritecolor: "red"};
          }
          static getDerivedStateFromProps(props, state) {
            return {favoritecolor: props.favcol };
          }
          render() {
            return (
              <h1>My Favorite Color is {this.state.favoritecolor}</h1>
            );
          }
          componentDidMount() {
            setTimeout(() => {
              this.setState({favoritecolor: "blue"})
            }, 1000);
          }
        }
        ReactDOM.render(<Header favcol="yellow"/>, document.getElementById('root'));
        ```
  * **Updating:** A component is updated whenever there is a change in the component's state or props. React has five built-in methods that gets called, in this order, when a component is updated -
    1. `getDerivedStateFromProps()`: Same as in Mounting phase.
       ```js
       // changeColor() won't have any effect since getDerivedStateFromProps() is called.
       static getDerivedStateFromProps(props, state) {
         return {favoritecolor: props.favcol };
       }
       changeColor = () => {
         this.setState({favoritecolor: "blue"});
       }
       ```
    2. `shouldComponentUpdate()`: Default value is true. You can return a Boolean value that specifies whether React should continue with the rendering or not.
       ```js
       // changeColor() won't have any effect since shouldComponentUpdate() returns false
       shouldComponentUpdate() {
         return false;
       }
       changeColor = () => {
         this.setState({favoritecolor: "blue"});
       }
       ```
    3. `render()`: **Mandatory**. Re-render the HTML to the DOM, with the new changes.
    4. `getSnapshotBeforeUpdate()`: Here you have access to the props and state before the update, meaning that even after the update, you can check what the values were before the update.
       > If the `getSnapshotBeforeUpdate()`is present, you should also include the `componentDidUpdate()`, otherwise you will get an error.
       ```js
       getSnapshotBeforeUpdate(prevProps, prevState) {
         document.getElementById("div1").innerHTML = "Before the update, the favorite was " + prevState.favoritecolor;
       }
       componentDidUpdate() {
         document.getElementById("div2").innerHTML = "The updated favorite is " + this.state.favoritecolor;
       }
       ```
    5. `componentDidUpdate()`: Called after the component is updated in the DOM
  * **Unmounting:** When a component is removed from the DOM. 
    1. `componentWillUnmount()`: React has only one built-in method that gets called when a component is unmounted. This is called before Unmounting the component

## Events
  * React has the same events as HTML: click, change, mouseover etc.
  * **Adding Events**
    * React events are written in camelCase syntax - `onClick` instead of onclick
    * React event handlers are written inside curly braces - `onClick={shoot}`, instead of onClick="shoot()"
  * **Bind this:** For methods in React, the this keyword should represent the component that owns the method. That is why **you should use arrow functions**. With arrow functions, this will always represent the object that defined the arrow function.
    ```js
    class Football extends React.Component {
      shoot = () => {
        alert(this);  // The 'this' keyword refers to the component object
      }
      render() {
        return (
          <button onClick={this.shoot}>Take the shot!</button>
        );
      }
    }
    ```
    
## Forms
  * **Handling Forms**
    * Form data are stored in the component state. You can control changes by adding event handlers in the onChange attribute
    * You must initialize the state in the constructor method before you can use it.
    * 
      ```js
      class MyForm extends React.Component {
        constructor(props) {
          super(props);
          this.state = { username: '', age: null };
        }
        myChangeHandler = (event) => {  
          // use the same event handler function for both input fields. This gives us much cleaner code and is the preferred way in React
          let nam = event.target.name;  // access to the field name
          let val = event.target.value;  // access to the field value
          this.setState({[nam]: val});
        }
        render() {
          return (
            <form>
              <h1>Hello {this.state.username} {this.state.age}</h1>
              <p>Enter your name:</p>
              <input type='text' name='username' onChange={this.myChangeHandler} />
              <p>Enter your age:</p>
              <input type='text' name='age' onChange={this.myChangeHandler} />
            </form>
          );
        }
      }
      ```
  * **Conditional Rendering**
    * If you do not want to display the h1 element until the user has done any input, you can add an if statement.
    * 
      ```js
      // Display the header only if username is defined
      class MyForm extends React.Component {
        constructor(props) {
          super(props);
          this.state = { username: '' };
        }
        myChangeHandler = (event) => {
          this.setState({username: event.target.value});
        }
        render() {
          let header = '';
          if (this.state.username) {
            header = <h1>Hello {this.state.username}</h1>;
          } else {
            header = '';
          }
          return (
            <form>
              {header}
              <p>Enter your name:</p>
              <input type='text' onChange={this.myChangeHandler} />
            </form>
          );
        }
      }
      ```
  * **Submitting Forms**
    * You can control the submit action by adding an event handler in the onSubmit attribute
      ```js
      class MyForm extends React.Component {
        constructor(props) {
          super(props);
          this.state = { username: '' };
        }
        mySubmitHandler = (event) => {
          event.preventDefault();  //we use event.preventDefault() to prevent the form from actually being submitted
          alert("You are submitting " + this.state.username);
        }
        myChangeHandler = (event) => {
          this.setState({username: event.target.value});
        }
        render() {
          return (
            <form onSubmit={this.mySubmitHandler}>
              <h1>Hello {this.state.username}</h1>
              <p>Enter your name, and submit:</p>
              <input type='text' onChange={this.myChangeHandler} />
              <input type='submit' />
            </form>
          );
        }
      }
      ```
  * **Validating Form Input**
    * You can validate form input when the user is typing or you can wait untill the form gets submitted.
    * 
      ```js
      myChangeHandler = (event) => {
        let nam = event.target.name;
        let val = event.target.value;
        let err = '';
        if (nam === "age") {
          if (val !="" && !Number(val)) {
            err = <strong>Your age must be a number</strong>;
          }
        }
        this.setState({errormessage: err});
        this.setState({[nam]: val});
      }
      ```
  * **Textarea**
    * In HTML, the value of a textarea was the text between the start tag `<textarea>` and the end tag `</textarea>`, 
    * BUT in React, the value of a textarea is placed in a value attribute `<textarea value={this.state.description} />`
  * **Select**
    * In HTML, the selected value in the drop down list was defined with the selected attribute
    * BUT in React, the selected value is defined with a value attribute on the select tag
      ```js
      <select value={this.state.mycar}>
        <option value="Ford">Ford</option>
        <option value="Volvo">Volvo</option>
        <option value="Fiat">Fiat</option>
      </select>
      ```

## CSS
  * **Inline Styling**
    * In JSX, JavaScript expressions are written inside curly braces, and since JavaScript objects also use curly braces, the styling in the example above is written inside two sets of curly braces `{{}}` - `<h1 style={{color: "red"}}>Hello Style!</h1>`
    * Properties with two names, like **background-color**, must be written with camel case syntax, like **backgroundColor**
    * **JavaScript Object:** You can also create an object with styling information, and refer to it in the style attribute
      ```js
      const mystyle = {
        color: "white",
        backgroundColor: "DodgerBlue",
        padding: "10px",
        fontFamily: "Arial"
      };
      return <h1 style={mystyle}>Hello Style!</h1>
      ```
  * **CSS Stylesheet**
    * Import the stylesheet in your application
      ```js
      import './App.css';
      class MyHeader extends React.Component {
        render() {
          return (
            <div>
              <h1>Hello Style!</h1>
              <p>Add a little style!.</p>
            </div>
          );
        }
      }
      ```
  * **CSS Modules**
    * The CSS inside a module is available only for the component that imported it, and you do not have to worry about name conflicts.
      ```css
      // Create the CSS module with the ".module.css" extesion, example: mystyle.module.css
      .bigblue {
        color: DodgerBlue;
        padding: 40px;
        font-family: Arial;
        text-align: center;
      }
      ```
      ```js
      // import the stylesheet in your component
      import styles from './mystyle.module.css'; 
      class Car extends React.Component {
        render() {
          return <h1 className={styles.bigblue}>Hello Car!</h1>;
        }
      }
      ```
