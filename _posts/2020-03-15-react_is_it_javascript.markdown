---
layout: post
title:      "React is it Javascript???"
date:       2020-03-15 23:01:26 +0000
permalink:  react_is_it_javascript
---


## What is React?
React is a framework made by facebook that allows a user to very quickly and efficiently build a frontend(client side/in browser). It is one of the most powerful frameworks currently released. 
- It uses powerful built in tools to change the way code is written
- It has a built in architecture that forces developers to change their mindset.
- it scale well to very large applications
lets talk about it!


###  It uses powerful built in tools to change the way code is written
React abstracts alot of the grunt work out of JavaScript. It uses a transcompiler called Bable. Keep in mind this is only one use for Bable. The main use for Bable is to convert ecmaScript6 (ES6) or newer versions of JavaScript into a backwards compatible version that all browers can read. In React its used to read JSX and compile it into JavaScript code. This allows react to take simple and writeable code like this

```
import React from 'react'

export default class Intoduction extends React.component{
  render(){
    return(
      <h1>Hello World</h1>
      )
  }
}
```

and convert it into code that all browers could read

```
"use strict";

Object.defineProperty(exports, "__esModule", {
  value: true
});
exports.default = void 0;

var _react = _interopRequireDefault(require("react"));

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

function _instanceof(left, right) { if (right != null && typeof Symbol !== "undefined" && right[Symbol.hasInstance]) { return !!right[Symbol.hasInstance](left); } else { return left instanceof right; } }

function _typeof(obj) { "@babel/helpers - typeof"; if (typeof Symbol === "function" && typeof Symbol.iterator === "symbol") { _typeof = function _typeof(obj) { return typeof obj; }; } else { _typeof = function _typeof(obj) { return obj && typeof Symbol === "function" && obj.constructor === Symbol && obj !== Symbol.prototype ? "symbol" : typeof obj; }; } return _typeof(obj); }

function _classCallCheck(instance, Constructor) { if (!_instanceof(instance, Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

function _defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } }

function _createClass(Constructor, protoProps, staticProps) { if (protoProps) _defineProperties(Constructor.prototype, protoProps); if (staticProps) _defineProperties(Constructor, staticProps); return Constructor; }

function _possibleConstructorReturn(self, call) { if (call && (_typeof(call) === "object" || typeof call === "function")) { return call; } return _assertThisInitialized(self); }

function _assertThisInitialized(self) { if (self === void 0) { throw new ReferenceError("this hasn't been initialised - super() hasn't been called"); } return self; }

function _getPrototypeOf(o) { _getPrototypeOf = Object.setPrototypeOf ? Object.getPrototypeOf : function _getPrototypeOf(o) { return o.__proto__ || Object.getPrototypeOf(o); }; return _getPrototypeOf(o); }

function _inherits(subClass, superClass) { if (typeof superClass !== "function" && superClass !== null) { throw new TypeError("Super expression must either be null or a function"); } subClass.prototype = Object.create(superClass && superClass.prototype, { constructor: { value: subClass, writable: true, configurable: true } }); if (superClass) _setPrototypeOf(subClass, superClass); }

function _setPrototypeOf(o, p) { _setPrototypeOf = Object.setPrototypeOf || function _setPrototypeOf(o, p) { o.__proto__ = p; return o; }; return _setPrototypeOf(o, p); }

var Intoduction = /*#__PURE__*/function (_React$component) {
  _inherits(Intoduction, _React$component);

  function Intoduction() {
    _classCallCheck(this, Intoduction);

    return _possibleConstructorReturn(this, _getPrototypeOf(Intoduction).apply(this, arguments));
  }

  _createClass(Intoduction, [{
    key: "render",
    value: function render() {
      return _react.default.createElement("h1", null, "Hello World");
    }
  }]);

  return Intoduction;
}(_react.default.component);

exports.default = Intoduction;
```

thats alot of nonesense!

please keep in mind this is using no html and in a very early form of JavaScript. It would look this bad if written in ES6 however Bable would still be reading and converting ES6 code too.

React also has a built in virtual DOM. This dom updates and reloads every time you save your code so no more need to keep hard relloading. It also allows developers to write to the virtual DOM then React updates the Browers DOM only change what has been updated. Becasue of this when you have multipil children rendered they need a unique key that React can keep track of this way it knows what to render. Since React is only updating what has been changed there is no more need to reload the who web page making apps faster.

### It has a built in architecture that forces developers to change their mindset.
React has a component hierarchy that changes the way applications are developed. It takes practice to start thinking in React however once you get the hang of it it is very fun and powerful. A React page is made up of tons of components that can do many different things. They can be both functional 

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

or class components

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

these are some of the simplist components call presentational conponents that have one job, render some html to a page. Typically these components are child components to some form of a container components that would handle the logic and pass data down to the child components as props. as seen above these props can be accessed with this.props*whatever the prop was named*.  These container components will usually have some form of state. This is data stored in the components that can change based on interaction. This state will change how the child components are endered/what info is passed down though props.

```
export default class Equipment extends React.Component{
    constructor(props){
        super(props)
        this.state = {
            name: this.props.name,
            description: this.props.description,
            clicked: false
        }
    }

    renderData = () => {
        if (this.state.clicked) {
          return <div>
            <label>Name: </label>
            <input onChange={this.handleNameChange} value={this.state.name}/>
            <label>Description: </label>
            <textarea onChange={this.handleDescriptionChange} value={this.state.description || ""}/>
            <button onClick={this.handleSave}>Save Equipment</button>
          </div>
        }else{
           return <div onClick={e => this.makeEditable()} >
               <EquipmentInfo name={this.props.name} description={this.props.description} />
            </div>
        }
      }

      handleSave = (e) => {
        e.preventDefault()
        const data = {
            name: this.state.name,
            description: this.state.description, 
            id: this.props.id
        }
        this.setState((state, props) => ({
          clicked: false
        }), this.props.updateCallback(data))
    }

      makeEditable = (e) => {
        this.setState({
          clicked: true
        })
      }

      handleNameChange = (e) => {
        this.setState({
          name: e.target.value
        })
      }

      handleDescriptionChange = (e) => {
        this.setState({
          description: e.target.value
        })
      }

    render(){
        return(
            <div>
                {this.renderData()}
            </div>
        )
    }
}
```

This component has a state that holds a plain old JavaScript object that looks like 
```
{
    name: this.props.name,
    description: this.props.description,
    clicked: false
        }
```
When the components div tag is clicked calls a function that changes the components state to clicked: true. Then because state changed the virtual dom rerenders with the updated state. It will see that Clicked is true and render the input tags rather then another component.


### React is very scalable
When you pair these things together like done in React a developer can quickly cleanly and effeciently create very interactive *views*. This can make building large scale applications much simpler with less bugs and more UI/UX options.
