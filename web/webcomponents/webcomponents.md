
# Web Components

## Basics

To be written ...

See: https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements


## Attributes

```
class FooElement extends HTMLElement {
	// Listen to attribute changes
	attributeChangedCallback(name, oldValue, newValue) {

	}

	// Declare what attributes the component uses
	static get observedAttributes() {
		return ["attribute-name", "another-attribute"];
	}
}
```


## Methods

```
class FooElement extends HTMLElement {
	method() { ... }	
}
```

## Dispatching events

### Dispatching on the component itself

```
let event = new Event("customevent", { /* optionally configure the event */ });
this.dispatchEvent(event);
```

### Dispatching on elements inside the shadow DOM

If you want to dispatch events on an element inside the shadow DOM, you've to configure the event to be `composed` and to `bubble`.

```
let event = new Event("customevent", {
	// The event shall bubble up the DOM
	bubbles: true,
	// The event shall leave the component's shadow DOM
	composed: true
});
someElementOfTheShadowDom.dispatchEvent(event);
```


## Slots

Slots allow document fragments to be inserted into a custom element.

### One slot

```
// Component template
<slot><!-- optional default content --></slot>


<!-- Usage -->
<my-component>
	<p>inserted into the slot</p>
</my-component>
```

### Multiple, named slots

```
// Component template
<slot name="slot-name"><!-- named slot; optional default content --></slot>
<slot><!-- default slot; optional default content --></slot>

<my-component>
	<p slot="slot-name">inserted into the named slot</p>
	<p>inserted into the default slot</p>
</my-component>
```

### Access the slot

Slots can be selected like normal elements:

```
this.shadowRoot.querySelectorAll("slot");
```

### Listen to slot changes

```
const slot = this.shadowRoot.querySelector("slot");

slot.addEventListener("slotchange", (e) => {
	
});

```

### Slot attributes

```
// assignedNodes() returns a NodeList of the elements inserted into the slo
const nodes = slot.assignedNodes();
```


## Styling

### Styling slotted content

Component styles do not apply to elements in the slot unless one selects them using a `::slotted` pseudo selector. 

```
::slotted(<selector>) { ... }

::slotted(span) { margin: 5px; }
```


### Styling the host component

#### The component itself

The component, as viewed from the containing document, can be styled too:

```
:host { ... }
:host(<selector on the host element>) { ... }

:host { background: rgb(200, 200, 200); }
:host([border]) { border: 1px; }
```

The style applied to the whole element.

#### Depending on the context

A Component can be styled depending on how it's located in the DOM.

With this selector one can apply styles to the host, when it is a decendant of the specified selector:

```
:host-context(<selector on the host element>) { ... }

:host-context(p) { font-weight: bold; }
```

```
<p>
	<web-component></web-component> <!-- above style applies only in this case-->
</p>
```


### CSS custom properties

Define and use custom properties:

```
.selector {
	background-color: var(--foocomponent-primary-color, rgb(200, 200, 200)); /* using a default value */
}
```

From outside define it:

```
:root {
	--foocomponent-primary-color: lightblue;
}
```
