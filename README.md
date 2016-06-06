# ReSci's React Style Guide.

(Adapted from [Airbnb's mostly reasonable guide][airbnb-react].)

## Table of Contents

1. [Organization](#organization)
  1. [File structure](#file-structure)
  1. [Component names](#component-names)
1. [General JS Syntax](#general-js-syntax)
  1. [ECMAScript 6](#ecmascript-6)
  1. [Comments](#comments)
  1. [Alignment and spacing](#alignment-and-spacing)
  1. [Object and array literals](#object-and-array-literals)
  1. [String literals and templates](#string-literals-and-templates)
  1. [Arrow functions](#arrow-functions)
1. [React Syntax](#react-syntax)
  1. [JSX tags](#jsx-tags)
  1. [Returning JSX](#returning-jsx)
  1. [Props](#props)
    1. [The `key` prop](#the-key-prop)
  1. [Event handlers](#event-handlers)
  1. [Components](#components)
    1. [Method ordering](#method-ordering)
    1. [Pure components](#pure-components)


## Organization

### File structure

* Create a separate file for each React component, with the component name as the filename.
  * Pure components that are closely related to another component may share a file with that
    component.
* Use `PascalCase` for filenames and `.jsx` for extensions.
  * For files used only within a Rails assets pipeline (via the [`react-rails`][react-rails] gem,
    for instance), use `snake_case.js.jsx` instead.

### Component names
* In general, use `PascalCase` to refer to components and `mixedCase` to refer to their
  instances.
* Prefer `PascalCase` to refer to instances that behave as singletons, like Flux stores.

## General JS Syntax

### ECMAScript 6
* This style guide assumes (and encourages) the use of ECMAScript 6 (ES6) for all JavaScript code,
  whether React or otherwise.

### Comments
* Punctuate and appropriately capitalize comments longer than one word.

```javascript
// heres a bad comment
const foo = someFunction();

// Here's a much better comment.
const bar = someOtherFunction();
```
* Place comments on the line immediately preceding the code they refer to. Again, this rule may be
  disregarded for single-word comments.
* Keep existing comments up to date! Err on the side of fewer comments but make sure to write
  self-documenting code.

### Alignment and spacing
* Limit lines to 100 characters.
* Align declarations within the same 'paragraph' on the equals sign.

```javascript
// bad
const firstName = 'Arwen';
const age = 2776;

// good
const firstName = 'Arwen';
const age       = 2776;
```
* Do not make multiple declarations on the same line. If a long list of declarations is required,
  refactoring (or destructuring) is in order.

```javascript
// bad
const a = 1, b = 2, c = 3, d = 4;

// Okay, but not ideal.
const a = 1;
const b = 2;
const c = 3;
const d = 4;

// Better, probably.
const [a, b, c, d] = [1, 2, 3, 4];
```

### Object and array literals
* Prefer ES6 shorthand for object literals with matching key-value names.

```javascript
// bad
const firstName = 'Barry';
const lastName  = 'Selmy';

const person = { firstName: firstName, lastName: lastName };

// good
const firstName = 'Ash';
const lastName  = 'Dayne';

const person = { firstName, lastName };
```
* Do not mix ES6 shorthand with full `key: value` syntax within the same object. Use the full syntax
  if necessary in such cases, but prefer consistent shorthand.

```javascript
// bad
const apple      = 'green';
const strawberry = 'red';

const fruit = { apple, strawberry, banana: 'yellow' };

// good
const apple      = 'green';
const strawberry = 'red';

const fruit = { apple: apple, strawberry: strawberry, banana: 'yellow' };

// better
const apple      = 'green';
const strawberry = 'red';
const banana     = 'yellow';

const fruit = { apple, strawberry, banana };
```
* Prefer the object spread operator to `Object.assign` when duplicating objects or creating new
ones. Note that object spread is still a [Stage 2 proposal for ECMAScript][object-spread] and
requires a transformer like [Babel][babel].

```javascript
// bad
const obj    = { one: 1, two: 2 };
const newObj = Object.assign({}, obj, { three: 3 });

// good
const obj    = { one: 1, two: 2 };
const newObj = { ...obj, three: 3 };
```

* Prefer the array spread operator to `Array.concat` when duplicating arrays or creating new ones.

```javascript
// bad
const arr    = [1, 2, 3];
const newArr = arr.concat([4, 5, 6]);

// good
const arr    = [1, 2, 3];
const newArr = [...arr, 4, 5, 6];
```

### String literals and templates
* Use single quotes for string literals.

```javascript
// bad
const pub = "The Three Broomsticks";

// good
const pub = 'The Three Broomsticks';
```
* Use double quotes rather than escape characters for strings containing `'`.

```javascript
// bad
const pub = 'The Hog\'s Head';

// good
const pub = "The Hog's Head";

```
* Use templates with interpolation instead of string concatenation. Do not pad interpolation curly
  braces with spaces.

```javascript
// bad
const name   = 'Odysseus';
const source = 'Ithaca';

const title = name + ' of ' + source;

// Good. Note: no spaces within curly braces in template.
const name   = 'Ajax';
const source = 'Telamon';

const title = `${name} of ${source}`;
```

### Arrow functions
* Prefer concise arrow functions (with implied return statements) to block functions (which require
  explicit return statements), except when [returning JSX content](#returning-jsx).

```javascript
const numbers = [1, 2, 3, 4];

// bad
const squares = numbers.map(n => {
  return n * n;
});

// bad
const squares = numbers.map(n => { return n * n; });

// good
const squares = numbers.map(n => n * n);
```

## React Syntax

### JSX tags
* Self-close tags with no children. Use a single space before the closing tag.

```javascript
// bad
<NewComponent className="new-component"></NewComponent>

// Bad: no space before closing tag.
<NewComponent className="new-component"/>

// good
<NewComponent className="new-component" />
```
* Always use double quotes for JSX attributes; this usage mirrors the HTML convention. (Note:
  continue to use single quotes for all JavaScript statements within JSX elements.)

```javascript
// bad
<HighScore className='score-container' />

// good
<HighScore className="score-container" />
```
* Unlike object literals, do not pad JSX curly braces with spaces.

```javascript
// bad
<Brontosaurus>{ this.getDinosaur() }</Brontosaurus>

// good
<Brontosaurus>{this.getDinosaur()}</Brontosaurus>
```
* Unlike ordinary JS expressions, do not pad JSX attribute assignments with spaces.

```javascript
// bad
<Brontosaurus className = "thunder-lizard" period = "Jurassic" />

// good
<Brontosaurus className="thunder-lizard" period="Jurassic" />
```
* For multi-line JSX tags, put each attribute on its own indented line.

```javascript
// bad
<AppComponent attributeOne="attribute-one"
              attributeTwo="attribute-two"
              attributeThree="attribute-three" />

// good
<AppComponent
  attributeOne="attribute-one"
  attributeTwo="attribute-two"
  attributeThree="attribute-three"
/>

// For components with children:
<AppComponent
  attributeOne="attribute-one"
  attributeTwo="attribute-two"
  attributeThree="attribute-three"
>
  <ChildComponent />
</AppComponent>
```

### Returning JSX
* Always use a block with an explicit return statement for arrow functions that return JSX content.
  Put the return statement on its own line.

```javascript
// bad
renderList() {
  return this.props.items.map(item => <ListItem key={item.id}>{item.name}</ListItem>);
}

// bad
renderList() {
  return this.props.items.map(item => { return <ListItem key={item.id}>{item.name}</ListItem>; });
}

// good
renderList() {
  return this.props.items.map(item => {
    return <ListItem key={item.id}>{item.name}</ListItem>;
  });
}
```

* Use parentheses when returning multi-line JSX content. Do not put any JSX on the same lines as the
  parentheses.

```javascript
// bad
render() {
  return <ListComponent>
           <ul>{this.getListItems()}</ul>
         </ListComponent>;
}

// bad
render() {
  return (<ListComponent>
            <ul>{this.getListItems()}</ul>
          </ListComponent>);
}

// good
render() {
  return (
    <ListComponent>
      <ul>{this.getListItems()}</ul>
    </ListComponent>
  );
}
```

### Props
* Destructure props whenever possible. If only one prop is used, destructuring may not be necessary.

```javascript
// bad
renderName() {
  return <div>{`My name is ${this.props.firstName} ${this.props.lastName}.`}</div>;
}

// good
renderName() {
  const { firstName, lastName } = this.props;
  return <div>{`My name is ${firstName} ${lastName}.`}</div>;
}

// Good: no need to destructure when a single prop is used once.
renderName() {
  return <div>{`The name's ${this.props.name}.`}</div>;
}

// Good: do destructure when a single prop is called multiple times.
renderName() {
  const { name } = this.props;
  return <div>{`O ${name}, ${name}! wherefore art thou ${name}?`}</div>;
}
```
* Omit the value of a prop if it is explicitly true.

```javascript
// bad
<Winterfell winterIsComing={true} />

// good
<Winterfell winterIsComing />
```
* Use `mixedCase` (or `lowerCamelCase`) for prop names.

```javascript
// Bad: snake case.
<Winterfell winter_is_coming />

// Bad: pascal case.
<Winterfell WinterIsComing />

// good
<Winterfell winterIsComing />
```
#### The `key` prop
* Do not use an array index as the `key` prop when creating lists of JSX elements.
  [This is an anti-pattern][key-prop-index].

```javascript
// Bad: array index as key prop.
renderListItems() {
  return this.getListItems().map((item, idx) => {
    return <li key={idx}>{item.name}</li>;
  });
}

// Good: unique ID as key prop.
renderListItems() {
  return this.getListItems().map(item => {
    return <li key={item.id}>{item.name}</li>;
  });
}
```
* An exception to this rule can be made for iterables that behave as constant properties. Freezing
  such objects can be an additional precaution, but if they are truly constant properties, that
  should be unnecessary.

```javascript
constructor(props) {
  // Other constructor things...
  this.NAMES = Object.freeze(['Blackfyre', 'Bloodraven', 'Bittersteel']);
}

// Good: a given index will always refer to the same list element.
renderNameList() {
  return this.NAMES.map((name, idx) => {
    return <li key={idx}>{name}</li>;
  })
}
```

### Event handlers
* Use `e` to denote the event variable within an event handler. Naming this variable `event` will
  cause the global `event` object to be shadowed within the scope of the event handler.

```javascript
// bad
handleContentChange(event) {
  this.setState({ content: event.target.value });
}

// good
handleContentChange(e) {
  this.setState({ content: e.target.value });
}
```
* If an event handler requires additional parameters at the time of invocation, use an arrow
  function to pass the relevant event (if any) as the handler's first argument and the additional
  parameters as subsequent arguments.

```javascript
handleFieldChange(e, field) {
  this.setState({ [field]: e.target.value });
}

handleClick(field) {
  console.log(`${field} clicked!`);
}

renderSomeField() {
  return (
    <input
      value={this.state.someField}
      onChange={e => handleFieldChange(e, 'someField')}
      onClick={() => handleClick('someField')}
    />
  );
}
```


### Components

#### Method ordering

1. `constructor`.
  1. Set the initial state here. Do not use `getInitialState`.
  1. Bind event handlers here. Binding in the render call creates a brand new function every time.
1. `componentDidMount`.
1. `componentWillUnmount`.
1. Component update lifecycle methods, including `componentWillReceiveProps`.
1. Event handlers.
  1. Name callbacks triggered by user actions `handle[UserAction]`, like `handleClick` or
     `handleSubmit`.
  1. Name callbacks registered with a dispatcher/store `on[EventName]Event`, like `onChangeEvent`
     or `onLoginEvent`.
1. Helper methods for rendering the component, like `getVisitorCount` or `getTimeOfDay`.
1. Render methods to build smaller parts of the component, named `render[PartName]`, like
   `renderTitle` or `renderMenuButtons`.
1. `render`.

#### Pure components
* Write pure components as plain JS objects.

```javascript
// bad
class PureComponent extends React.Component {
  render() {
    const { title, content, handleClick } = this.props;
    return (
      <div className="pure-component" onClick={handleClick}>
        <h4>{title}</h4>
        {content}
      </div>
    );
  }
}

// good
const PureComponent = ({ title, content, handleClick }) => {
  return (
    <div className="pure-component" onClick={handleClick}>
      <h4>{title}</h4>
      {content}
    </div>
  );
}
```

<!--- Links --->
[airbnb-react]: https://github.com/airbnb/javascript/tree/master/react
[react-rails]: https://github.com/reactjs/react-rails
[object-spread]: https://github.com/sebmarkbage/ecmascript-rest-spread
[babel]: https://babeljs.io/
[key-prop-index]: https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318
