This guide is aimed to provide a critical overview of two popular front-end web development tools - [React](https://facebook.github.io/react/)  and [Angular 2](https://angular.io/). The tools will be evaluated based on their approaches to building application structure, involvement and adoption by the community, performance and ability to integrate with different platforms. 

The goal of this guide is to delineate core differences and similarities between the two tools in order facilitate decision-making when it comes to choosing which is appropriate for a given project specification.


# Development flow

### Angular 2
Angular 2 feels more like a fully-fledged framework - it comes with many building blocks out of the box that cover most of the common scenarios in developing a web application. There is also a clear separation of the roles of the different elements:

- *Services* - Injectable elements that are used to consume data from an API or share   state between multiple components.
- *Components* - Building blocks of the user interface that consume services. Can be nested inside each other through structural directive selectors.
- *Directives* - Divided into structural and attribute directives. Structural directives (ex. <code>*ngFor</code>)  manipulate the DOM while attribute directives are part of elements and control their style and state.
- *Pipes* - Used to format how data is displayed in the view layer.

### React

React is far less rigid than Angular 2. It is more of a library that provides the most basic tools for building a web applications - a HTTP service and Components.  There is no built-in router or anything that sets a particular convention. It is mostly up to the developer's choice what kind of packages he/she is going to use to shape the applicaiton.

# Application architecture

### Angular 2 

![angular 2 data flow](https://raw.githubusercontent.com/pluralsight/guides/master/images/f5c9fff0-9d0c-4e5c-8cc2-aa646d5c2c8b.png)
*Bi-directional data flow typical for MVC patterns*

Angular 2 is using the standard bi-directional data flow model. Once an action is made, the data goes up to the service layer, then it goes back to the view. This way of manipulating data has been widely criticised because the state of the data can be changed both ways, resulting in inconsistencies.



### React

![react data flow](https://raw.githubusercontent.com/pluralsight/guides/master/images/032be950-9bac-4a65-9bba-0a33089aad37.png)
*Flux data flow*

React uses [flux](https://facebook.github.io/flux/docs/overview.html) as a way of constructing its applications. Each action is sent to a dispatcher, which sends the new information to stores. Stores then update (mutate) the information  and send it down to the views (controller-views), which listen for changes.





### Redux

![Redux](https://raw.githubusercontent.com/pluralsight/guides/master/images/f685aa1c-99d2-44aa-b705-99267c16bd4b.com-optimize)
*Redux data flow*

Redux is the new cool kid on the block //more about redux

Redux was first introduced for React as an alternative to simplify Flux. Because of that, Flux is more widely adopted in the React community.


Recently, Angular 2 has started adopting the [Redux](http://redux.js.org/docs/introduction/ThreePrinciples.html) pattern for data flow by utilizing [rxjs](https://splintercode.github.io/is-angular-2-ready/)'s observables to maintain the state of the data and using pure functions to manipulate it. [Attemps have been made](http://blog.rangle.io/getting-started-with-redux-and-angular-2/) to build unidirectional data flow in Angular 2 applications, but the adoption of this technique is still low.



# Structuring code
Angular 2 is written in TypeScript, a superset language of JavaScript developed by Microsoft. The language introduced types, new data structures and more object-oriented features, making the code easier to read and maintain compared to other alternative.It uses [TSLint](http://palantir.github.io/tslint/) to check the code for issues. Using TypeScript makes setting up an Angular 2 application somehow tediuos, as it introduces extra overhead for configuration. 

React is relying on JSX, a XML-esque syntax extension for rendering JavaScript and HTML. Syntax and strucutre-wise, JSX looks more familiar to JavaScript and apperas to blend into HTML more easily. It uses [ESLint](http://eslint.org/) to track for isses in the code.

To get a clearer comparison between React and Angular 2's code strucures, we are going to compare how a basic TODO application is built (example apps built by [Mark Volkmann (mvolkmann)](https://github.com/mvolkmann/react-examples).

 Both applications use [Webpack](https://webpack.github.io/) for development and deployment.


##### Angular 2


*index.html*

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Angular Todo App</title>
    <link rel="stylesheet" href="todo.css">
  </head>
  <body>
    <todo-list>Loading...</todo-list>
    <script src="bundle.js"></script>
  </body>
</html>

```

##### React

*index.html*

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Angular Todo App</title>
  </head>
  <body>
    <todo-list>Loading...</todo-list>
    <script src="bundle.js"></script>
  </body>
</html>
```


Because both setups use Webpack to handle all the assets, the index.html file is identical.

Application structure-wise, both React and Angular 2 need two files - <code>TodoList</code> that represents the list of tasks and <code>Todo</code> that represents a single task.

I'll analyize the <code>TodoList</code>  component (*todolist.component.ts* for Angular 2 and *todo-list.js* for React, respectively) piece-by-piece, from top to bottom:

#### **Importing** 

##### Angular 2


```ts
import {Component} from '@angular/core';
import {bootstrap} from from '@angular/platform-browser-dynamic';;
import {TodoCmp, ITodo} from "./todoCmp";
```
##### React


```js
import React from 'react';
import ReactDOM from 'react-dom';
import Todo from './todo';
```

#### **Initializing a component**

##### Angular 2

```js
// Used for type of state property in TodoListCmp class.
// Both properties are optional, but preferred as they make 
// the code more maintainable.
interface IState {
  todos?: ITodo[];
  todoText?: string;
}
 
// This syntax is called a Decorator and is similar
// to Annotations in other languages.  Decorators are
// under consideration to be included in ES2016.
@Component({
  // other components used here
  directives: [TodoCmp],
  // TodoListCmp's HTML element selector
  selector: 'todo-list',
  template: // see value to left of
            // React render method below
})

export class TodoListCmp {
  private static lastId: number = 0;
  private state: IState;

  constructor() {
    this.state = {
      todos: [
        TodoListCmp.createTodo('learn Angular', true),
        TodoListCmp.createTodo('build an Angular app')
      ]
    };
  }
```

##### React


```js
let lastId = 0; // no static class properties in ES6

class TodoList extends React.Component {
  constructor() {
    super(); // must call before accessing "this"
    this.state = {
      todos: [
        TodoList.createTodo('learn React', true),
        TodoList.createTodo('build a React app')
      ]
    };
  }
```

#### **Creating an item**

##### Angular 2

```ts
static createTodo(
    text: string, done: boolean = false): ITodo {
    return {id: ++TodoListCmp.lastId, text, done};
}

onAddTodo(): void {
    const newTodo: ITodo =
      TodoListCmp.createTodo(this.state.todoText);
    this.state.todoText = '';
    this.state.todos = this.state.todos.concat(newTodo);
}
```

##### React
```js
static createTodo(text, done = false) {
    return {id: ++TodoList.lastId, text, done};
}

onAddTodo() {
    const newTodo =
      TodoList.createTodo(this.state.todoText);
    this.setState({
      todoText: '',
      todos: this.state.todos.concat(newTodo)
    });
}
```
#### **Detecting item states**

##### Angular 2

```js
get uncompletedCount(): number {
    return this.state.todos.reduce(
      (count: number, todo: ITodo) =>
        todo.done ? count : count + 1, 0);
}
```
##### React 
```js
get uncompletedCount() {
    return this.state.todos.reduce(
      (count, todo) =>
        todo.done ? count : count + 1,
      0);
}

```

####  **Reacting to changes**

##### Angular 2
```js
onArchiveCompleted(): void {
    this.state.todos =
      this.state.todos.filter((t: ITodo) => !t.done);
}

onChange(newText: string): void {
    this.state.todoText = newText;
}

onDeleteTodo(todoId: number): void {
    this.state.todos = this.state.todos.filter(
      (t: ITodo) => t.id !== todoId);
}

onToggleDone(todo: ITodo): void {
    const id: number = todo.id;
    this.state.todos = this.state.todos.map(
      (t: ITodo) => t.id === id ?
        {id, text: todo.text, done: !todo.done} : t);
}
```

##### React
```js
onArchiveCompleted() {
    this.setState({
      todos: this.state.todos.filter(t => !t.done)
    });
}

onChange(name, event) {
    this.setState({[name]: event.target.value});
}

onDeleteTodo(todoId) {
    this.setState({
      todos: this.state.todos.filter(
        t => t.id !== todoId)
    });
}

onToggleDone(todo) {
    const id = todo.id;
    const todos = this.state.todos.map(t =>
      t.id === id ?
        {id, text: todo.text, done: !todo.done} :
        t);
    this.setState({todos});
}
```
#### **Bootstrapping**

##### Angular 2
```
} // end of TodoListCmp class

// Each Angular app needs a bootstrap call
// to explictly specify the root component.
// In larger apps, bootstrapping is usually in
// a separate file with more configuration.
bootstrap(TodoListCmp);
```
#####  React
```
} // end of TodoList class

// This renders a TodoList component
// inside a specified DOM element.
// If TodoList was used in more than one place,
// this would be moved to a different JavaScript file.
ReactDOM.render(
  <TodoList/>,
  document.getElementById('container'));
```
#### **Template syntax**

##### Angular 2
```

  // This is the value of the @Component template
  // property above.  It was moved here for
  // easy comparison with the React render method.
  `<div>
    <h2>To Do List</h2>
    <div>
      {{uncompletedCount}} of
      {{state.todos.length}} remaining
      <!-- Clicking this button invokes the
           component method onArchiveCompleted. -->
      <button (click)="onArchiveCompleted()">
        Archive Completed
      </button>
    </div>
    <br/>
    <form>
      <!-- [ngModel] tells where to get the value. -->
      <!-- (input) tells what to do on value change. -->
      <input type="text" size="30" autoFocus
        placeholder="enter new todo here"
        [ng-model]="state.todoText"
        (input)="onChange($event.target.value)"/>
      <button [disabled]="!state.todoText"
        (click)="onAddTodo()">
        Add
      </button>
    </form>
    <ul>
      <!--
        This uses a for loop to generate TodoCmps.
        #todo defines a variable to use within the loop.
        [todo] sets a property on each TodoCmp.
        (onDeleteTodo) and (onToggleDone) are outputs
        from the TodoCmp, and they call functions
        on TodoListCmp with emitted values.
        deleteTodo receives a type of number.
        toggleDone receives a type of ITodo.
      -->
      <todo *ngFor="let todo of state.todos" [todo]="todo"
        (on-delete-todo)="onDeleteTodo($event)"
        (on-toggle-done)="onToggleDone($event)"></todo>
    </ul>
  </div>
```

##### React
```  
render() {
    // Can assign part of the generated UI
    // to a variable and refer to it later.
    const todos = this.state.todos.map(todo =>
      <Todo key={todo.id} todo={todo}
        onDeleteTodo=
          {this.onDeleteTodo.bind(this, todo.id)}
        onToggleDone=
          {this.onToggleDone.bind(this, todo)}/>);

    return (
      <div>
        <h2>To Do List</h2>
        <div>
          {this.uncompletedCount} of
          {this.state.todos.length} remaining
          <button
            onClick={() => this.onArchiveCompleted()}>
            Archive Completed
          </button>
        </div>
        <br/>
        <form>
          <input type="text" size="30" autoFocus
            placeholder="enter new todo here"
            value={this.state.todoText}
            onChange=
              {e => this.onChange('todoText', e)}/>
          <button disabled={!this.state.todoText}
            onClick={() => this.onAddTodo()}>
            Add
          </button>
        </form>
        <ul className="unstyled">{todos}</ul>
      </div>
    );
 }

```

The component code for Angular 2 is more verbose than React's . TypeScript largely contributes to the this. <code> :void </code> , <code> :ITodo </code> and other types are often seen in the  Style-wise, varaibles, function parameters and function themselves must have a type in TypeScript. React does not have any of that, making the code more convenient for developers who prefer traditional JavaScript. 

Angular 2 uses <code>@Component</code> decorators to specify the different constructs (services, directives, pipes) that it contains and to define its own properties. It also lets the developer specify a <code>selector</code> for the component and put the code for the template using the <code> template </code> property or the <code> templateUrl</code> if a separate file is used. The same applies for styles - there is an option to input styles directly using the <code> styles </code> property or to include an array of files using <code> styleUrls </code>. React does not have an analog for a component decorator and requires the template of the component to be included in the component file itself, thus limiting the options for customization and making the information about a component harder to interpret.

Angular 2 templates come with a specific convention for different elements:
- ```*``` denotes a directive that manipulates the DOM. For example, ```*ngFor``` iterates over an array of values and creates a new element for each. ```*ngIf``` is used to denote whether an element can be seen or not.
- ```[]``` denotes a property binding, indicating that the element receives a value from the component. It is used mostly to pass data to a child component to the parent.
- ```()``` indicates an event binding (such as ```(click)```, which triggers a function in the components.
- ```[()]``` indicates two-way binding, meaning that any changes to the data that are passed will be propagated.

React templates also come with their peculiarities. JSX blurs the line by html and JavaScript, providing its own markup. Even though it seems that the template is written in HTML, the markup is actually XML that is later parsed into HTML.
Because of JSX, standard html property words are camel-cased or have diffeerent names - <code>onclick</code> in JSX is <code>onClick</code>. <code>for</code> (used in labels) becomes <code>htmlFor</code> as <code>for</code> is a reserved word in JavaScript for a loop.


//todo finish this up





# Performance

 Performance is another critical criteria for comparison. It shows how the applications handle under heavy load and gives predictions for future problems as the applications become bulkier and more difficult to optimize.
 
 The following tests have been developed by [Stefan Krauss (krausest)](https://github.com/krausest). The test is carried out with [Selenium](http://docs.seleniumhq.org/docs/03_webdriver.jsp) to emulate user actions and record the results. Here are the results (in miliseconds):

![benchmarks](https://raw.githubusercontent.com/pluralsight/guides/master/images/c5324f3d-e9e4-4b3a-b9aa-760a083fd89a.002)


 It is clear that React (v. 15.3.0) is either on par or faster than Angular 2's release candidate 4. The only action at which React is slower than Angular 2 is in replacing the data in all rows ( 192.23ms for Angular 2 vs 214.07ms for React).  Angular 2 also appears to be better at selecting an element in (3.32ms vs. 6.78ms). But is two times slower at appending elements (611.75ms vs. 293.82ms).
 
 In terms of memory management, React outperforms Angular 2 by using three times less memory after pageload (15.30ms vs. 4.75ms) and two times less memory when adding new elements to the DOM ( 21.10ms vs 10.67ms).
 
 **Even though React is a clear winner, the results cannot be considered absolutely fair**. Angular 2 is still under development and the focus is not on optimizing the speed but rather fixing bugs and polishing features.
 
# Cross-platform integration

The cross-platform development world has made a huge leaps since the massive adoption of mobile devices. For both Angular 2 and React, WebView-based tools for building cross-platform mobile applications are a thing of the past. 

There are two tools that allow building of cross-platform applications with native capabilities - [NativeScript](https://www.nativescript.org/) and [React Native](https://facebook.github.io/react-native/)

NativeScript is built by [Telerik (now Progress)](http://www.telerik.com/) with the intention to enable native mobile applications to be built with Angular 2. Even though it has [8k](https://github.com/NativeScript/NativeScript) starts on Github, NativeScript is mature and comes with great documentation and numerous extensions. It preaches a "write once, run everywhere" methodology of developing cross-platform applications. Even though this is an efficient approach, it does not enable to use many platform-specific UI features.

React Native is developed by Facebook to give React native capabilities. Compared to NativeScript, React Native is still young but quite popular and maturing quickly, boasting over [37k](https://github.com/facebook/react-native) stars on GitHub. React Native uses a "learn once, write anywhere" approach to developing cross-platform applications. Instead of 
attempting to generalize different platforms like NativeScript does, Rect Native embraces their differences, offering different ways to construct UI for the different platforms. Recently, the Angular 2 team has started adopting React Native, [making a renderer for it](https://github.com/angular/react-native-renderer).

# Adoption
[Angular 2](https://github.com/angular/angular) is just shy of 16K stars on Github. There is a [tracker](https://splintercode.github.io/is-angular-2-ready/)  that counts how many issues are left until Angular 2 is ready for release, with hopes this to happen until thethe end of 2016. Even though Angular 2 is relatively young, its community is maturing fast, mostly because many Angular 1 developers are startng to switch to Angular 2. There are already [around 6500](https://github.com/search?l=TypeScript&p=3&q=angular+2&type=Repositories&utf8=%E2%9C%93) repositories on Github that contain "Angular 2" and are written in TypeScript.

[React](https://github.com/facebook/react), on the other hand, is almost at 50K stars on Github. There are [76,122](https://github.com/search?l=JavaScript&p=1&q=react&ref=searchresults&type=Repositories&utf8=%E2%9C%93) repositories  containing the word "react" in them - over ten times more than what Angular 2 has. Still, one of the big reasons for the large number of repositories is that React comes with just a few built-in functionalities and relies on its community to provide it with the needed tooling to develop full-scale applications.
