# ReSci's React Style Guide.

(Adapted from [Airbnb's mostly reasonable guide][airbnb-react].)


### File structure

* Create a separate file for each React component, with the component name as the filename.
  * Pure components that are closely related to another component may share a file with that
    component.
* Use `PascalCase` for filenames and `.jsx` for extensions.
  * For files used only within a Rails assets pipeline (via the [`react-rails`][react-rails] gem), use
    `snake_case.js.jsx` instead.

### Component names
* In general, use `PascalCase` to refer to components and `mixedCase` to refer to their
  instances.
* Prefer `PascalCase` to refer to instances that behave as singletons, like Flux stores.

### Object literals
* Prefer ES6 shorthand for object literals with matching key-value names.

```
// bad
const firstName = 'Barry', lastName = 'Selmy';
const person = { firstName: firstName, lastName: lastName };

// good
const firstName = 'Ash', lastName = 'Dayne';
const person = { firstName, lastName };
```
* Do not mix ES6 shorthand with full `key: value` syntax within the same object. Use the full syntax
if necessary in such cases, but prefer consistent shorthand.

```
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
* Prefer the object spread operator to `Object.assign` when copying objects or creating new ones.

```
// bad
const obj       = { one: 1, two: 2 };
const newObject = Object.assign({}, obj, { three: 3 });

// good
const obj       = { one: 1, two: 2 };
const newObject = { ...obj, three: 3 }
```
Note that object spread is still a [Stage 2 proposal for ECMAScript][object-spread] and requires a
transformer like [Babel][babel].


### Comments
* Punctuate and appropriately capitalize comments longer than one word.

```
// this is bad comment
const foo = someFunction();

// This is a good comment.
const bar = someOtherFunction();
```
* Place comments on the line immediately preceding the code they refer to. Again, this rule may be
  disregarded for single-word comments.
* Keep existing comments up to date!

### Handling props
* Destructure props whenever possible.

```
// bad
renderHeader() {
  return <div>{this.props.title + ": " + this.props.subtitle}</div>;
}

// good
renderHeader() {
  const { title, subtitle } = this.props;
  return <div>{title + ": " + subtitle}</div>;
}
```


### Alignment and spacing
* Align declarations on the equals sign. Do not make multiple declarations on the same line. If a long list of declarations is required, refactoring is in order.


<!--- Links --->
[airbnb-react]: github.com/airbnb/javascript/tree/master/react
[object-spread]: github.com/sebmarkbage/ecmascript-rest-spread
[react-rails]: github.com/reactjs/react-rails
[babel]: babeljs.io

<!---
working

* prefer [array spread, new element] to array.concat for creating new arrays in pure functions
--->
