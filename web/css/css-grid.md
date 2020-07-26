
# CSS Grid Cheetsheet

## Terminology 

- column/row line: the (virtual) lines starting, separating and ending the columns/rows


## Container

```
.container {
	display: grid;
	grid-gap: 3px; /* gap between cells */
	grid-template-columns: 1fr 1fr; /* two columns, equal width distribution */
	grid-template-rows: 100px auto 50px; /*  three rows, flexible middle row */
}
```


## Columns

### Span

```
.cell {
	grid-column-start: 1;
	grid-column-end: 3; /* for two columns 3 is the last column line */

	grid-column: 1 / 3; /* shorthand */
	grid-column: 1 / -1; /* -1 for very last column line */
	grid-column: 1 / span 2; /* start at column line 1, stretch by two columns */
}
```


## Rows

### Span

```
.cell {
	grid-row-start: 1;
	grid-row-end: 3; /* for two row 3 is the last row line */

	grid-row: 1 / 3; /* shorthand */
	grid-row: 1 / -1; /* -1 for very last row line */
	grid-row: 1 / span 2; /* start at row line 1, stretch by two rows */
}
```


## Template areas

```
.container {
	grid-template-areas: 
		"a a a a a a a"
		"b b c c c c ."
		"b b d d d d d"; /* letters deonote identifiers, . are empty cells */
}

.cell {
	grid-area: "{a|b|c}"; /* identifier letters referenced from the template area */
}
```


## Control the Grid density

```
.container {
	grid-auto-flow: dense;
}
```


## Justifying and aligning content

### The bounding box wrapping the items in the container

```
.container {
	justify-content: {center|space-around|...}
	align-content: {center|space-around|...}
}
```

### All cells at once

```
.container {
	justify-items: {start|center|...}
	align-items: {start|center|...}
}
```

### Individual cells

```
.cell {
	justify-self: {start|center|...}
	align-self: {start|center|...}
}
```

## New units and functions

### `fr` - Fraction

Fraction of the available space.

```
1fr 1fr /* 50% each */
2fr 1fr /* 25% and 75% */
1fr 2fr 1fr /* three (columns), middle is twice as wide as the outer */
```

### `repeat()` - Repeat the expression

`repeat(<repititions>, <unit>)`

```
repeat(3, 1fr) /* equal to */ 1fr 1fr 1fr
repeat(2, 50px) /* equal to */ 50px 50px
repeat(auto-fit, 50px) /* fit as many cells to the row to fill the container */
```

### `minmax()` - Choose the lowest of the specified value

`minmax(<preferred minimum value>, <up to the fraction per cell to stretch a column>)`

```
/* distribute as many cells as possible with least 100px width */
/* but stretch them to up to 1fr if possible */
repeat(auto-fit, minmax(100px, 1fr));
```


## See also

- https://www.youtube.com/watch?v=t6CBKf8K_Ac - freecodecamp.org course
- https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Grids - MDN guide
- https://learncssgrid.com/ - Guide highlighting all aspects
