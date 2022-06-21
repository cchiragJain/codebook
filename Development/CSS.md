<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [CSS Selectors](#css-selectors)
  - [Tag, Class & Id Selectors](#tag-class--id-selectors)
  - [Pseudo Selectors](#pseudo-selectors)
- [FlexBox](#flexbox)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# CSS Selectors

- Used [web dev simplified](https:www.youtube.com/watch?v=l1mER1bV0N0&t=22s) video on css selectors

- [CSS Selectors](#css-selectors)
  - [Tag, Class & Id Selectors](#tag-class--id-selectors)
  - [Pseudo Selectors](#pseudo-selectors)
- [FlexBox](#flexbox)

## Tag, Class & Id Selectors

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

## Pseudo Selectors

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

# FlexBox

- [Refer this](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
