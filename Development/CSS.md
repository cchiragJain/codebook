# CSS

## CSS Selectors

### Tag, Class & Id Selectors

```css
p {
	/* selects all elements with p tag */
}

.error {
	/* select all with class error
	 <element class="error"></element> */
}

#id-name {
	/* select element with that id */
}

p.error {
	/* selects all p tags with error class inside it
	 <p class = "error toggle">some text</p> */
}

p.error.toggle {
	/* selects all p tags with error class inside it
	ex.
	<p class = "error toggle">some text</p> */
}

p .error {
	/* will select those p which have a element with error class
	 inside them
	 ex.
		<p>
    ... (could be nested inside it as well)
			<element class = "error"></element>
		</p> */
}

h1,
p {
	/* select both h1 and p elements */
}

div a {
	/* select all children of the first element */
}

div > a {
	/* select only direct children of the first element */
}

div ~ a {
	/* Selects all anchors that are siblings(on the same level) of a div and come after the div */
}

div + a {
	/* Select elements that are siblings of the first element and come directly after the first element */
}
```

### Pseudo Selectors

```css
button:hover {
	/* select buttons that are being hovered
   can be used for other tags as well */
}

input:focus {
	/* select input which is currently clicked on */
}

/*
  - required
  - checked
  - disabled
*/

div::before {
	/* creates an empty element directly before the children of the
   selected element
   ex. */
	content: "before current";
}

div::after {
	/* creates after the children of the selected element */
}
```

## Units

## Box model

## Positions

## Media Queries

## Flexbox

## Grid
