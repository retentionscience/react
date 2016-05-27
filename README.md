# ReSci's React Style Guide.

(Adapted from [Airbnb's mostly reasonable guide][airbnb-react].)

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

## General Syntax

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
const newObj = { ...obj, three: 3 }
```

* Prefer the array spread operator to `Array.concat` when duplicating arrays or creating new ones.

```javascript
// bad
const arr    = [1, 2, 3];
const newArr = arr.concat([4, 5, 6])

// good
const arr    = [1, 2, 3];
const newArr = [...arr, 4, 5, 6];
```

## React Syntax

### JSX
* Self-close tags with no children. Use a single space before the closing tag.

```javascript
// bad
<NewComponent className="new-component"></NewComponent>

// Bad: no space before closing tag.
<NewComponent className="new-component"/>

// good
<NewComponent className="new-component" />
```
* Always use double quotes for JSX attributes; this usage mirrors the HTML convention. However,
  prefer single quotes for all other Javascript.

```javascript
// Bad: JSX attribute uses single quotes, and JS code uses unnecessary double quotes.
<HighScore className='score-container'>
  {"High Score: " + this.getHighScore()}
</HighScore>

// Good: JSX attribute uses double quotes, and JS code uses single quotes.
<HighScore className="score-container">
  {'High Score: ' + this.getHighScore()}
</HighScore>
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
<Brontosaurus className = "thunder-lizard" key = {idx} />

// good
<Brontosaurus className="thunder-lizard" key={idx} />
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
>
  <ChildComponent />
</AppComponent>
```

### Props
* Destructure props whenever possible, unless only one is being used.

```javascript
// bad
renderHeader() {
  return <div>{this.props.title + ": " + this.props.subtitle}</div>;
}

// good
renderHeader() {
  const { title, subtitle } = this.props;
  return <div>{title + ": " + subtitle}</div>;
}

// Good: no need to destructure when only one prop is used.
renderTitle() {
  return <div>{this.props.title}</div>;
}
```
* Omit the value of a prop if it is explicitly true.

```javascript
// bad
<Winterfell winterIsComing={true} />

// good
<Winterfell winterIsComing />
```
* Used `mixedCase` (or `lowerCamelCase`) for prop names.

```javascript
// Bad: snake case.
<Winterfell winter_is_coming />

// Bad: pascal case.
<Winterfell WinterIsComing />

// good
<Winterfell winterIsComing />
```
### The `key` prop
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
  such objects can be an additional precaution, but if it is truly a constant property, this should
  be unnecessary.

```javascript
constructor(props) {
  // Other constructor things...
  this.NAMES = Object.freeze(['Blackfyre', 'Bloodraven', 'Bittersteel']);
}

// Good: a given index will always refer to the same name.
renderNameList() {
  return this.NAMES.map((name, idx) => {
    return <li key={idx}>{name}</li>;
  })
}
```

### Components

#### Method ordering

1. `constructor`.
  1. Set the initial state here. Do not use `getInitialState`.
  2. Bind event handlers here. Binding in the render call creates a brand new function every time.
2. `componentDidMount`.
3. `componentWillUnmount`.
4. Component update lifecycle methods, including `componentWillReceiveProps`.
5. Event handlers.
  1. Name callbacks triggered by user actions `handle[UserAction]`, like `handleClick` or
  `handleSubmit`.
  2. Name callbacks registered with a dispatcher/store `on[EventName]Event`, like `onChangeEvent`
  or `onLoginEvent`.
6. Helper methods for rendering the component, like `getVisitorCount` or `getTimeOfDay`.
7. Render methods to build subparts of the component, like `renderTitle` or `renderMenuButtons`.
8. `render`.

#### Pure components
* Write pure components as plain Javascript objects. Use an explicit return statement when returning
JSX content, which will likely always be the case for React components.

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
