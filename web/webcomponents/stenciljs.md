
# Stencil

## What it is and what it not is

- No framework
- Is is a collection of tools, an SDK


## Start a project

Select a type of project to initialize

```
$ npm init stencil
```


## Components

```
import { Component, Prop, State, Method } from "@stencil/core";

@Component({
	tag: "<TAG NAME>",
	styles: "<CSS SELECTORS>",
	styleUrl: "<RELATIVE .css FILE>",
	shadow: true,
	/* - Emulates shadow DOM by generating scoped CSS selectors
	 * - Does not use shadow DOM
	 */
	scoped: true
})
class MyComponent {
	@Prop({
		/* Update this attribute when you change
		 * <foo property="42 => 47">
		 */
		reflect: true
	})
	property: string;

	@State
	state: boolean;

	@Method
	interact() {
	}

	render() {
		return (
			<html via tsx>
			<button onClick={this.handler.bind(this)}></button>
			<slot></slot>
			{this.referenceToProperty}
		);
	}
}
```


## Unidirectional dataflow by default

```
class MyComponent {
	/* Can't modify this property from the component by default.
	 */
	@Prop({
		// To change it in methods
		mutable: true,
		// To change the DOM attribute too
		reflectToAttr: true
	})
	property: string;

	someHandler() {
		// Not working unless mutable: true is used
		this.property = "47";
	}
```


## Watch property changes

```
class MyComponent {
	@Prop({
		mutable: true
	})
	property: string;

	/* Watch the specified @Prop() annotated property for changes.
	 */
	@Watch("property")
	propertyUpdated(newValue: string, oldValue: string) {
		// ...
	}

```


## Returned markup

```
render() {
	return (
		<only-one-root-element />
	);
}

render() {
	// Return an array of multiple elements
	return [
		<one-element />
		<another-element />
	];
}
```


## Event emitter

```
@Event({ bubbles: true, composed: true })
someEvent: EventEmitter<string>;

someMethod() {
	this.someEvent.emit();
}
```


## Event listener

```
/* event-name: an event inside the shadow DOM
 * body:event-name: an event name bubbled up to the host's body node
 */
@Listen('event-name')
eventHandler(event: CustomEvent) {
	// ...
}
```


## Referencing elements

### The host element

```
class MyComponent {
	/* Reference to the host element ("this" in WebComponent context)
	 */
	@Element()
	el: HTMLELement;
}
```

### Any element

```
class MyComponent {
	el: HTMLELement;
	render() {
		return (
			<div ... ref={(el) => this.el = el}></div>
		);
	}
}
```


## Two way binding

```
class MyComponent {
	@State()
	value: string;
	render() {
		return (
			<input ... value={this.value}/>
		);
	}
}
```

## Lifecycle hooks

```
componentWillLoad() {
	// Before inital rendering & mounting
	// Changes to stateful properties will be reflected by the next render,
	// though changes won't cause an actual render
}

componentDidLoad() {
	// After initial rendering, mounting
}

componentWillUpdate() {
	// Run right before a render
}

componentDidUpdate() {
	// Run right after an update to a @Prop()
}

componentDidUnload() {
	// Run right after being removed from the DOM
}
```


## Setting host element properties

```
class MyComponent {
	/* Define DOM node properties on the host element.
	 * Return an object of DOM properties like "class" or "lang".
	 * This method name is reserved, will run on state updates.
	 */
	hostData() {
		return { class: "...", lang: "...", /* ... */ };
	}
}
```



# Deploying Stencil components

## Loading bundles

Include the `<namespace>.js` file.

```
+- dist
   +- collection
   +- esm
  ...
   +- <namespace>.js
  ...
```


## Publishing to NPM

```
$ npm publish [--access public]
```


## Using NPM modules

### Angular

1. Define the custom elements schema to `app.module.ts`
2. `import { defineCustomElements } from "<component-name>/dist/loader"` and call with window in `main.ts`

### React

`import { defineCustomElements } from "<component-name>/dist/loader"` and call with window in `index.js`

### Vue.js

`import { defineCustomElements } from "<component-name>/dist/loader"` and call with window in `main.js`

