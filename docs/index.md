---
title: Sleeptime Markup Language (STML) reference
layout: home
---

<style type="text/css">
	#book-search-input,
	.navigation,
	.pull-right
	{
		display: none;
	}
</style>

STML is an XML-like markup language, influenced by HTML and CSS, developed by Mark LaCroix at Noble Robot. It is used to create the in-game content of the game *Dreamsettler* by Tendershoot and Noble Robot.

Unique features include generic closing tags that prevent interleaved tag pairs, a native "include" solution and a unified syntax for markup and style documents, and a set of conditional elements that allow for fully arbitrary page rendering based on game states and save data values.



## Anatomy of a basic STML page

```
<!DOCTYPE stml>
<stml>

	<head>
		<title>Name of this page</>
		<author>Character id or name</>
		<description>A paragraph or so about this page (user-facing).</>
		<music source=\path\to\musicFile.aud />
	</>

	<body>
		<block>
			<text>
				Text goes <span fashion=bold>here</>.
				Here's a <link to=\page2.stm>link</> to another page.
			</>
		</>

		<block>
			<image source=absolutePath.som\to\image.img />
			<image source=\relativePath\to\image.img />
			<image source=..\parentDirectory\image.img />
		</>

		<rule />
		<text>It's sometimes useful to insert a horizontal rule between sections</>
		<rule />

		<list style=listStyle>
			<item>Item 1</>
			<item>Item 2</>
			<item>Item 3</>
		</>

		<embed source=\path\to\interative\component />

		<include source=\path\to\footer.stinc />

		<< This is a comment! Content here is not rendered on the page. >>

	</>

	<soul />

</>

<sss>

	<< there are two types of styles, document styles... >>
	<style id=listStyle
		pointType=square
		textColor=#333333
	/>

	<< ...and element styles >>
	<body backgroundColor=#FEFEFE />
	<block margin=10 />
	<text font=toronto />

	<< You can also import styles from SSS documents >>
	<include source=\path\to\style.sss />

</>
```





## Elements



### Top-level elements

#### `<!DOCTYPE>`

```
<!DOCTYPE stml>
```

This element, as-is, must appear at the top of every STML file. It is not strictly necessary for parsing the document, but it helps external text editors (such as Visual Studio Code) recognize the filetype, since STML is an XML-like language.



#### `<stml>`

```
<stml>
	...
</>
```

This is a *required* top-level element that contains the page's content. it serves the same function as the `<html>` tag does in an HTML document. It can accept some (mostly background-related) attributes, and has one default attribute value:

- **`backgroundColor`** (default: `#738080`)



#### `<sss>`

```
<sss>
	...
</>
```

This is an *optional* top-level element that contains **document style** and **element style** elements (detailed later in this document). It accepts no attributes itself.

Think of the **sss** element like an embedded stylesheet. In HTML, this is done by including CSS code inside of an HTML `<style>` tag under the HTML`<head>` tag, but here, we repurpose STML syntax instead of using a separate language, and move style information into its own top-level element.





### Constitutional elements

These elements exist under the **`<stml>`** element. All are *required*.

#### `<head>`

```
<stml>
	<head>
		...
	</>
</>
```

As with HTML, this is where user-facing metadata lives (see "Metadata elements"). It accepts no attributes.



#### `<body>`

```
<stml>
	<body>
		...
	</>
</>
```

As with HTML, this is where the actual content of the page lives. This element accepts most but not all attributes that a **`<block>`** element can, and has the following default attribute values:

- **`width`** (*default*: `500`)
- **`align`** (*default*: `center`)
- **`backgroundColor`** (*default*: `#C7DDDD`)
- **`padding`** (*default*: `0`)



#### `<soul>`

```
<stml>
	<soul />
</>
```

*What is this for? Indeed, for what attributes must — or may even — comprise an enumerated soul?*

This is a self-closing element.





### Metadata elements

#### `<title>`

```
<head>
	<title>...</>
</>
```

This *required* element contains the title of the page. In *Dreamsettler*'s Nexus browser, this appears in the program's titlebar. Its contents can be localized (see **`<language>`**). It accepts no attributes.



#### `<author`>

```
<head>
	<author>...</>
</>
```

This *optional* element contains the name/username of the person (in *Dreamsettler*, the *character*) who authored this page. It is not rendered on the page. In *Dreamsettler* it may appear in settlement directories, search results, file info panels, etc. If the content matches an known character or entity id, it can be picked up by *Dreamsettler*'s content indexer. It accepts no attributes.



#### `<details>`

```
<head>
	<details>
		...
	</>
</>
```

This *optional* element contains a short (one-paragraph or less) description of the page. It is not rendered on the page. In *Dreamsettler*, it may appear in settlement directories, search results, file info panels, etc. Its contents can be localized (see **`<language>`**). It accepts no attributes.



#### `<music>`

```
<head>
	<music source=/path/to/file.aud />
</>
```

This is the audio file for the page, which can play when the player loads this page. This element accepts the following three attributes:

- **`source`**: The path to an audio file.
- **`autoplay`**
  - `true`: The file will plays immediately when file page loads.
  - `false`: The file does not play unless until the user manually plays it.
  - `user` (*default*): Plays immediately on page load, unless a user-configured setting instructs it not to.
- **`remember`**
  - `true` (*default*): In *Dreamsettler*, this value will instruct the Nexus browser to remember the last playing state and timecode position of the page's audio file when a user either leaves the page or closes the browser, and will return to that state and position upon re-visiting the page.
  - `false`: Will respect the **`autoplay`** setting and begin at the start of the audio file on every page load.






### Block-level elements



#### `<block>`

```
<body>
	<block>
		<block>
			<block> ... </>
		</>
	</>
</>
```

Similar to HTML's `<div>` tags, STML's **block** is the principal "block-level" element of an STML page. It can accept most attributes, and pass on certain attributes to child elements. It can be nested infinitely, like turtles.

It has the following default attribute values:

- **`margin`** (*default*: `10`)
- **`padding`** (*default*: `5`)



#### `<rule>`

```
<rule />
```

This is a self-closing element. Behind the scenes, A **horizontal rule** is simply an empty **`<block>`** with a height of 0 and a border element enabled. It can accept many attributes, and has the following default attribute values:

- **`align`** (*default*: `center`)
- **`margin`** (*default*: `10`)
- **`borderType`** (*default*: `solid`)
- **`borderColor`** (*default*: `#000000`)
- **`borderThickness`** (*default*: `2`)



#### `<text>`

```
<text>...</>
```

Unlike in HTML, where text content can be placed anywhere inside of the document, in STML, all document text must live inside of a **text** element (or **`<item>`** element).

A **text** element may contain inline (**`<span>`** and **`<link>`**) elements. Its contents can be localized (see **`<language>`**).

It can accept many attributes, and has the following default attribute values:

- **`font`** (*default*: `edita`)
- **`textSize`** (*default*: `16`)
- **`textColor`** (*default*: `#000000`)
- **`margin`** (*default*: `3`)
- **`align`** (*default*: `left`)



#### `<image>`

```
<image source=/path/to/file.img />
```

This is a self-closing element. In *Dreamsettler*, an **image** element without a valid `source` attribute will display a placeholder "broken" image. It can accept many attributes.



#### `<list>`

```
<list>
	<item>...</>
	<list>
		<item>...</>
		<item>...</>
	</>
	<item>...</>
</>
```

A **list** element is similar to HTML's `<ul>` and `<ol>` tags. In STML, it behaves like a **`<block>`** that can only contain **`<item>`**,  **`<image>`**, and nested **`<list>`** elements. Like other block-level elements, it accepts most attributes. **List** elements can be nested inside of each other in order to increase the indent level of list items.

Like other block-level elements, it accepts most attributes. The following attributes are unique to **list** (and **`<item>`**) elements:

- **`listType`** (*default*: `unordered`)
- **`pointType`**  (*default*: `circle`)
- **`orderType`**  (*default*: `1`)
- **`pointColor`** (*default*: undefined)



#### `<item>`

An **item** element can only exist inside of a **`<list>`** element. It is essentially a **`<text>`** element (accepting the same attributes and can contain the same inline elements) with a "point" component prepended to it.

**Item** elements also inherit the unique attributes applied to its parent **`<list>`** element, and may also override any of those attributes. It has the following unique attribute:

- **`indent`** (*default*: `0`): An integer that overrides the item's "level" in the list, *relative to its calculated level*. It has no effect on items above or below it. This value may be positive or negative.





### Inline elements

These elements can only be placed inside of **`<text>`** and **`<item>`** elements, threaded amongst their text content. Inline elements won't accept attributes that are specific to the rendering of block-level elements (such as **`width`**/**`height`**, **`display`**, **`margin`**, etc.), but otherwise accept most attributes.



#### `<span>`

```
<text font=edita>
	I will serve this ship as <span fashion=bold>First Officer.</> And in an attack against the Enterprise, <span fashion=italic>I will die with this crew</>. But I will not break my <span font=roma dance=blink>oath of loyalty</> to Starfleet.
</>
```

A **span** element works similarly to the same-named tag in HTML. HTML's classic `<b>` `<i>` and `<u>` tags can also be replicated this way by using the **`fashion`** attribute, or with SSS styles.



#### `<link>`

```
<text>
	Click <link to=/path/to/file.stml>here</> to go away.
</>
```

A **link** element is similar to HTML's anchor (`<a>`) tag. It has the following default attribute values:

- **`textColor`** (*default*: `#1EC16C`)
- **`fashion`** (*default*: `line`)

**Link** elements are unique compared to all other elements in that they also have default attribute *overrides* for thier "hover," "click," and "visited" states.

"Hover" state:

- **`textColor`** (*default*: `#cf1371`)

"Click" state:

- **`textColor`** (*default*: `#ebc20e`)

"Visited" state:

- **`textColor`** (*default*: `#ea7097`)

These values are normally assigned to elements by creating a **`<style>`** element and assigning its **`id`** to the original element's **`hoverStyle`**, **`clickStyle`**, or **`visitedStyle`** attributes (see "Document styles," below).





### External content elements

These are self-closing block-level elements which insert the contents of an external asset or file into the flow of an STML document.



#### `<embed>`

```
<embed source=/path/to/interactive/component />
```

An **embed** element works similarly to how an embedded Flash or Java object (or ActiveX control, if you're nasty) worked on the webpages of the early-to-medieval Internet. It provides a "canvas" area on the page for an interactive component to operate within.

In *Dreamsettler*, **embed** elements reference custom-developed "widgets" that are part of the game's internal asset structure and code assemblies. They are arbitrary components with no set format or template, and do not exist outside of the game.

**Embed** elements can receive string values from STML document into thier source components via "queries," which are key/value strings that can be appended to the page's path (usually as part of a link to that page). Queries are passed to the page's **embed** elements' interactive component on page load.



#### `<include>`

```
<stml>
	<body>
		<include source=/header.stinc />
		...
		<include source=/footer.stinc />
	</>
</>
<sss>
	<include source=/style.sss />
	...
</>
```

STML's **include** element takes the contents of an external text file and pastes it into an STML document. This extends STML in a way familiar to web developers, but which requires additional languages or technologies in HTML.

**Include** elements must point to either...

- a `.stinc` file, which is an STML-formatted file that does not contain any top-level elements, or...

- an `.sss` file, which is an STML-formatted file that contains only *document style* or *element style* elements.

An **include** element itself does not "exist" within the rendering of an STML page, so it can accept no attributes other than **`source`**.

##### Example .stinc file

```
<< Page header >>
<block style=header orientation=horizontal>
	<image source=/images/logo.img />
	<list style=menu>
		<item><link to=/main.stm>Home</>
		<item><link to=/blog.stm>Blog</>
		<item><link to=/about.stm>About</>
	</>
</>
```

##### Example .sss file

```
<< Element styles >>
<block margin=12 marginTop=0 padding=8 borderType=solid borderThickness=1 />
<text font=toronto />

<< Document styles >>
<style id=header borderType=dashed borderThickness=4>
<style id=menu fashion=notLine,upper>
```





### Comments

Content within comments are not rendered on the page. They can be placed anywhere in a document.

#### Block comment

```
<< Comment >>

<<
	Multiline
	comment
>>

<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<>>
<<                                             >>
<<             Oh hey, you can hide            >>
<<          messages in code comments!         >>
<<                                             >>
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<>>
```

Like HTML, STML only has "block" comments, not single-line comments. But unlike HTML's comment blocks (`<!--  -->`), they are not irritating to type out.



#### Section comments

```
<body>
	<< section Menu >>
	<block>
		...
	</>
	<</>

	<< section Content >>
	<block> ... </>
	<block> ... </>
	<block> ... </>
	<</>>
</>
```

A comment which begins with the word "section" can be treated specially by a text editor, such as Visual Studio Code, which recognizes "code regions."

When a section comment is encountered, the editor looks for a matching `<</>>` comment. It then defines all the content in-between as a region, which aids in navigating the document by allowing the user to collapse and expand that portion of code.





### Style Elements

Within the **`<sss>`** top-level element, there are two types of acceptable child elements, *document styles*, and *element styles*.

Both types of style elements are self-closing elements.



#### Document styles (`<style>`)

Content STML elements that can accept any attribute can also accept the `style` attribute, which references a **document style**, or **`<style>`** element.

A **document style** can contain *any* attribute, and when referenced by other elements becomes a "reusable collection" of attributes. This works similarly to how CSS classes are applied to HTML tags.

To create a **document style**, place a style tag inside of the document's **`<sss>`** tag, give it an **`id`**, and any number of other attributes:

```
<sss>
	<style id="myStyle"
		font=noble
		fashion=italic
		textColor=#333333
		backgroundColor=#0099CC
	/>
</>
```

To then assign that style to one or more STML elements, set the STML element's **`style`** attribute to match the **`id`** of the chosen **document style**:

```
<block style="myStyle">
	<text>What an interesting look...</>
</>
<block style="myStyle">
	<text>...and it never goes out of style!</>
</>
```

In addition to the **`style`** attribute, a **document style** may also be referenced by the **`hoverStyle`**, **`clickStyle`**, or **`visitedStyle`** attributes on a content element. These styles are applied to an element only when that element is currently in the corresponding state.



#### Element styles

An **element style** works similarly to a document style, but instead of having an **`id`** and using the **`style`** attribute on an content element in order to apply the style's attributes, an **element style** is created using a *self-closing version* of the corresponding element (such as **`<block/>`**, **`<image/>`** or **`<text/>`**).

When placing these elements inside of the **`<sss>`** top-level element, they do not act as content elements, and are not rendered on the page.

Any content element that accepts attributes can also be used as an element style in this way:

```
<stml>
	<body>
		<block>
			<text>This text is blue, set against a dark gray background!</>
		</>
	</>
</>

<sss>
	<block
		padding=10
		backgroundColor=#222222
	/>
	<text
		font="dream"
		color=#0000FF
	/>
</>
```

**Element styles** apply thier attributes to content elements of the same name, unless overridden by a document style or attributes on an individual element.





### Conditional elements

In *Dreamsettler*, certain elements on pages are conditional, based on values stored in a player's save data file, such as a collected item, outcome of a case, or the in-game date. Conditional elements are *non-user-facing*, *non-content* container elements which determine if a part of a document is rendered based on those values.

These elements can be used anywhere inside or outside of any other element.

In *Dreamsettler*, unlike all other STML elements, conditional elements are *non-diegetic*. They do not "exist" in the world of the game, and they are not displayed when a player views the source of a page.



#### `<language>`

```
<head>
	<title>
		<language is="en-US">Page Title</>
		<language is="fr">Titre de la page</>
		<language is="ja">ページタイトル</>
	</>
</>
<body>
	<block>
		<text>
			<language is="en-US"> ... </>
			<language is="fr"> ... </>
			<language is="ja"> ... </>
		</>
		<language is="en-US"><image source=/image-en.img /></>
		<language is="fr"><image source=/image-fr.img /></>
		<language is="ja"><image source=/image-ja.img /></>
	</>
</>
```

**Language** elements are optional, and only should be used for elements which need to be localized. In *Dreamsettler*, any content *not* inside of a **language** element will be rendered as-is, regardless of the game's currently set language.

The **language** element accepts two attributes, **`is`** and **`not`**, which determine if the content is rendered. The only valid values for these attributes are strings, in the form of an *IETF language tag*.



#### `<date>`

Content inside of a **date** element is displayed only if the current in-game date matches one or more of the these attributes:

- **`is`**
- **`not`**
- **`greaterThan`**: After this date.
- **`greaterThanOrIs`**: On or after this date.
- **`lesserThan`**: Before this date.
- **`lesserThanOrIs`**: On or before this date.



These attributes accept a date styled as an number in the format `YYYY`, `YYYYMM`, or `YYYYMMDD`.

```
<date is=2004> ... </>
<date is=200406> ... </>
<date is=20040625> ... </>
```



To create a narrower condition, multiple attributes can be nested:

```
<date greaterThanOrIs=200411>
	<date lessThanOrIs=200603>
		<text>Today is after 2004/10/31 and before 2006/04/01.</>
	</>
</>
```



Multiple values can also be applied to the same attribute as a comma separated list:

```
<date not=2003,2004><text>It's not 2003 or 2004 right now.</></>
<date is=2003,2004><text>It's either 2003 or 2004 right now.</></>
<date lessThan=20041031 greaterThan=20060401>
	<text>It's either before Halloween 2004, or it's after April Fools Day 2006.</>
</>
```



The **`is`** and **`not`** attribute can also accept days of the week or months, spelled out in lowercase:

```
<date is=monday,wednesday> ... </>
<date not=september> ... </>
```





#### `<check>`

#### `<value>`

```
<check id=booleanExample>
	<value is=true> It's real! </>
	<value is=false> It's a faaaaakkke! </>
</>
<check id=ifElseExample>
	<value is=conditionA><text>It's equal to A!</></>	<< if >>
	<value is=conditionB><text>It's equal to B!</></>	<< else if >>
	<value><text>It's neither A nor B!</></>			<< else	>>
</>
<check id=andExample>
	<value greaterThanOrIs=1.5>
		<value lessThanOrIs=4.2><text>The value is between 1.5 and 4.2!</></>
	</>
</>
<check id=orExample>
	<value is=conditionA,conditionB><text>It's equal to either A or  B!</></>
</>
<check id=nestedExampleLevel1>
	<value not=something>
		<text>Now I know level 1 is not "something"!</>
		<check id=nestedExampleLevel2>
			<value is=whatever>
				<text>
					Level 1 is not "something", and level 2 is "whatever"!
				</>
			</>
		</>
	</>
</>
```

A **check** element only has one attribute, `id`, which is a key that references a gameplay variable of some kind.

A **value** element is very similar to the **`<date>`** element, can have one (or more) of the following attributes:

- **`is`**: Number, string, boolean, or a comma-separated list of same.
- **`not`**: Number, string, boolean, or a comma-separated list of same.
- **`greaterThan`**: Number
- **`greaterThanOrIs`**: Number
- **`lesserThan`**: Number
- **`lesserThanOrIs`**: Number

***NOTE**: This is the only place in STML where a non-integer number can be used for any attribute.*

Multiple **value** elements inside of a **check** element creates "if/else if/else" functionality. The parser goes down the list of value elements and only the contents of the first matching tag is displayed, even if subsequent tags would also evaluate as true.

A **value** element with no attributes operates like an `else` block, and displays if no preceding **value** elements match.

AND/`&&` functionality can be achieved by nesting **value** elements.

OR/`||` functionality can be achieved by including multiple values in an attribute on a single **value** element.





## Attributes

Attributes and thier possible values are listed below. Elements which accept STML attributes will accept a subset of the total amount, so not all attributes will be applied to all elements even if assigned.

Attributes are divided into categories ("Layout", "Box", etc.) for convenience, but these categories are merely taxonomic, rather than determinative.

Default values provided here apply to elements that do not have default attribute values specified elsewhere.



### Classification



#### `id`

A unique name for this element.

- undefined (*default*)



#### `style`

The **`id`** of a document style (**`<style>`**).

Any attribute that is set on the referenced document style will be applied to this element, unless overridden by attributes set directly on this element.

- undefined (*default*)



#### `hoverStyle`

#### `clickStyle`

#### `visitedStyle`

The **`id`** of a document style (**`<style>`**).

Any attribute that is set on the referenced document style will be applied to this element *when this element is in the corresponding state*. Similar to a "pseudo-class" in CSS.

- undefined (*default*)





### Layout



#### `display`

Determines how the element will be rendered within the flow of the page.

- `flow` (*default*): The element will stack in the page flow. **`x`**/**`y`** attributes are defined as relative to the point where the element *would* be rendered. Similar to CSS’s `position: relative`.

- `floe`: The element is removed from the page flow, and proceeding elements will fill the space it vacates. **`x`**/**`y`** attributes are defined as relative to the point where the element *would* be rendered. Similar to, but not exactly the same as, CSS’s `position: absolute`.

- `anchored`: The element is removed from the page flow, but unlike `flow`, its `x`/`y` position is relative to the top-left of the content viewport, and will not move when the page scrolls. Similar to CSS's `position: fixed`.

- `buoyed`: Acts like `flow` until the the element would otherwise be scrolled off the screen, then it acts as if it is `anchored` to the top of the content viewport. Similar to CSS’s `position: sticky`. Useful for "top bar" or "menu" elements.

- `none`: Will not render the element.



#### `align`

Aligns the element relative to its container. Also sets the origin point that its **`x`**/**`y`** attributes are calculated from.

- `left` (*default*): Top-left.
- `right`:  Top-right.
- `center`:  Top-center.

The following are available to elements whose **`display`** attribute is `floe` or `anchored`:

- `bottomLeft`
- `bottomRight`
- `bottomCenter`
- `middleLeft`
- `middleRight`
- `middleCenter`



#### `orientation`

Determines the flow of the contents of this element.
- `vertical` (*default*): Child elements flow from top to bottom.
- `horizontal`: Child elements flow from left to right.



#### `x`

Sets the horizontal position of the element, relative to a point depending on its `display` and `align` attributes. Non-zero values may be positive or negative.

- `0` (*default*)



#### `y`

Sets the vertical position of the element, relative to a point depending on its `display` and `align` attributes. Non-zero values may be positive or negative.

- `0` (*default*)



#### `z`

Sets the z-index of an element, *relative to its position in markup order*. Since every element's default '"global" z-index is +1 relative to the element preceding it, this means that if an element's `z` attribute is set to -1, it is drawn behind the element immediately preceding it, but still above the element preceding *that*.  Non-zero values may be positive or negative.

- `0` (*default*)



#### `width`

The width of the element in pixels (`50`) or in some cases percent (`50%`). Non-zero values must be positive. In most contexts, an undefined value sets the element's width to 100% of it's containing element.

- undefined (*default*)



#### `height`

The height of the element in pixels (`50`) or in some cases percent (`50%`). Non-zero values must be positive. In most contexts, an undefined value sets the element's height to the calculated content of the element, including any children.

- undefined (*default*)





### Box



#### `margin`

#### `marginHorizontal`

#### `marginVertical`

#### `marginTop`

#### `marginBottm`

#### `marginLeft`

#### `marginRight`

Sets "outer" spacing for this object, beyond of the element's border and background. More specific attributes will override less specific ones. Non-zero values must be positive.

- `0` (*default*)



#### `padding`

#### `paddingHorizontal`

#### `paddingVertical`

#### `paddingTop`

#### `paddingBottom`

#### `paddingLeft`

#### `paddingRight`

Sets "inner" spacing for this object, within the element's border and background. More specific attributes will override less specific ones. Non-zero values must be positive.

- `0` (*default*)





### Background



#### `backgroundColor`

The background color of the element, including its **`padding`** but excluding its **`margin`**.
- `none` (default)
- A hexadecimal value (i.e.: `#000000`)



#### `backgroundImage`

The path to a background image file. The image will fills the bounds of the element, including its **`padding`** but excluding its **`margin`**.

- undefined (default)



#### `backgroundAlign`

Determines the origin point of the background image, relative to the bounds of the element.

- `left` (*default*): Top-left.
- `right`:  Top-right.
- `center`:  Top-center.

- `bottomLeft`
- `bottomRight`
- `bottomCenter`
- `middleLeft`
- `middleRight`
- `middleCenter`



#### `backgroundRepeat`

How the background image handles an element which is larger/smaller than the dimensions of the image.
- `xy`: (*default*) Tiles the background image both horizontally and vertically.
- `x`: Tiles the background image horizontally only.
- `y`: Tiles the background image vertically only.
- `slice`: converts the image into 9 "slices," based on the the values of the element's **`slice`** attributes.
- `none`: Does not tile or modify the background image.



#### `slice`

#### `sliceTop`

#### `sliceBottom`

#### `sliceLeft`

#### `sliceRight`

Determines the 9-slice values when **`backgroundRepeat`** is set to `slice`.  The more specific attributes (`sliceLeft`, etc.) will override `slice`.

- `0`  (*default*)





### Border



#### `borderType`

When set to a value other than `none`, will display a border around the element.
- `none` (*default*)
- `solid`
- `dashed`
- `dashedTogether`
- `dashedApart`
- `dotted`
- `pilled`
- `caution`:  A dashed line where the dashes are angled.



#### `borderColor`

The color of the border, in hexadecimal format.

- `#000000` (*default*)
- `none`



#### `borderThickness`

In pixels, the thickness of the border.

- `2` (*default*)



#### `borderTop`

#### `borderBottom`

#### `borderLeft`

#### `borderRight`

Boolean values which enable or disable the border around its four sides. These values have no effect when **`borderType`** is set to `none`.

- `true` (*default*)
- `false`





### Shadow



#### `shadowAlpha`

Percentage of shadow opacity on the element. An integer from `0` (transparent) to `100` (solid).

- `0` (default)



#### `shadowColor`

The color of the shadow, in hexadecimal format.

- `#000000` (*default*)
- `none`



#### `shadowBlur`

The width of the drop shadow gradient in pixels. An integer between `0` and `25`.

- `0` (*default*)



#### `shadowX`

The horizontal offset of the drop shadow in pixels. May be a positive or negative integer.

- `4` (*default*)



#### `shadowY`

The vertical offset of the drop shadow in pixels. May be a positive or negative integer.

- `4` (*default*)





### Text



#### `font`

The font used for text. In *Dreamsettler*, valid fonts are pre-configured.

- `edina` (*default*): A humanist serif font.
- `roma`: A neo-grotesque font.
- `dream`: A modernist display font.
- `toronto`: A round geometric font.
- `letter`: A monospace type-style font.
- `loos`: A dense screen font.
- `capital`: A Neoclassical/Art Deco font.
- `cutz`: A casual/humorous font.
- `mister`: A script-style display font.
- `noble`: A humanist pixel font.
- `mystica`: A gothic display font.
- `arkansas`: A slab-serif pixel font.
- `gorton`: a low-contrast geometric font. ***(not currently implemented)***
- `blonk`: A casual/humorous font. ***(not currently implemented)***



#### `textSize`

The point size of the text. Must be greater than zero.

- `12` (*default*)



#### `textColor`

The color of the text.

- A hexadecimal value (i.e.: `#000000`) (*default*)
- `none`



#### `fashion`

The weight, style, and other text modifiers. Multiple values can be applied via a comma separated list (`<text fashion=bold,italic>...</>`).
- undefined (default)
- `bold`
- `italic`
- `line`: "underline"
- `strike`: "strikethrough"
- `lower`: Modifies the text to use all lowercase letters.
- `upper`: Modifies the text to use all uppercase letters.

Since **`fashion`** can hold multiple values, each modifier has a corresponding "not" value, which is used to "cancel out" an inherited value.
- `notBold`
- `notItalic`
- `notLine`
- `notStrike`
- `notLower`
- `notUpper`



### List



#### `listType`

Determines what will be used for the list's/item's point component.
- `unordered` (*default*): uses a graphic for each point item.
- `ordered`: Uses the current font to display sequential values for each item. Items in an ordered list use different **`orderType`** sequences (see below) depending on their nested "level."
- `unenumerated`: Removes points entirely.



#### `pointType`

Selects the point graphic that appears before each item. Only applies when **`listType`** is set to `unordered`.
- `circle` (*default*)
- `circleOpen` (same as`circle` but as an outline rather than a solid graphic)
- `square`
- `squareOpen`
- `triangle`
- `triangleOpen`
- `diamond`
- `diamondOpen`
- `arrow`
- `dash`
- `chevron`
- `slice`



#### `orderType`

Applies a specific sequence to each item in a list. Only applies when **`listType`** is set to `ordered`.
- `1` (*default*): Arabic number (1, 2, 3...)
- `A`: Latin uppercase (A, B, C...)
- `I`: Roman Uppercase (I, II, III...)
- `a`: Latin lowercase (a, b, c...)
- `i`: Roman lowercase (i, ii, iii...)



#### `pointColor`

A hexadecimal color value applied to the list or item point's `unordered` graphic or its `ordered` text, if undefined or `none`, uses the color of the font.
- undefined (default)
- `none`
- A hexadecimal value (i.e.: `#000000`)



#### `indent`

- An integer that overrides an item's "level" in its list, *relative to its calculated level*. It has no effect on items above or below it. This value may be positive or negative. (*default*: `0`)





### Dance



#### `dance`

An animation applied to an element or text characters.

- `none` (*default*)
- `blink`
- `bounce`
- `rock`
- `roll`
- `wiggle`
- `sway`
- `shake`
- `quake`
- `quake2`
- `spin`
- `flip`
- `gyro`
- `pulse`
- `sparkle`
- `hue`
- `marquee`: Only works on **`<text>`** and **`<item>`** elements.



#### `danceTempo`

The speed of the dance. Must be a positive integer between `1` and `100`.

- `10` (*default*)



#### `danceVolume`

The intensity of the dance. Must be a positive integer between `1` and `100`.

- `10` (*default*)



#### `danceVolume`

The starting offset of the dance. Useful when neighboring items share the same dance. Must be a positive integer between `1` and `100`.

- `1` (*default*)







### Paths



#### `source`

For elements which represent an external file, such as **`<image>`** and **`<include>`** , the path to that file.

- undefined (*default*)



#### `to`

A path to another file. When set on an element, that element becomes a link to the path when clicked.

- undefined (*default*)



#### `pop`

When paired with a **`to`** attribute, tells the Nexus browser to open the link in a new pop-up window. Similar to the "target" attribute in HTML.

- `false` (*default*)
- `true`





### Miscellaneous



#### `cursor`

***(NOTE: not currently implemented)***

Determines the image assed used for the mouse cursor when hovering over this element.

- `default` (*default*): The standard "pointer" cursor for the current desktop theme.
- `hand`: The standard "hand" cursor for the current desktop theme.
- `move`: The standard "move" cursor (arrows pointing in the four cardinal directions) for the current desktop theme.
- `resize`: The standard "resize" cursor (a two-way diagonal arrow) for the current desktop theme.
- `bones`: A skeleton hand cursor
- `other1`: *TBD*.
- `other2`: *TBD*.
- `other3`: *TBD*.
- A path to custom image asset.



#### `cursorColor`

Applies a color tint to the current **`cursor`** asset. This is not suitable for all cursor assets and may be disabled for some.

- `none` (*default*)
- A hexadecimal value (i.e.: `#000000`)



#### `zoom`

Applies a scale factor to the element, as a percentage. Valid values are integers between `1` and `500`.

- `100` (*default*)



### Conditional



#### **`is`**

#### **`not`**

#### **`greaterThan`**

#### **`greaterThanOrIs`**

#### **`lesserThan`**

#### **`lesserThanOrIs`**

See **`<date>`** and **`<check>`**/**`<value>`**.





## Appendices



### Structure of Sleepnet

Websites on the "sleeptime Internet" of *Dreamsettler*, called Sleepnet, are based on the existence of a single "webroot" folder in the game's asset structure. The webroot folder is the symbolic root of the entire Sleepnet. Everything exists within it and nothing exists outside of it.

Websites exist inside of "domains," each of which is a subfolder placed in the webroot. Only domain folders, no individual files, are allowed to exist in the webroot.

*NOTE: Some domain folders may be physically stored in a separate location than others (user-generated content compared to in-game content, for example), but for the purposes of Sleepnet, there is only one webroot, shared by all domain folders.*

Within a domain folder, pages and other files can be organized in any way.



#### Top-level domains

In *Dreamsettler*, just like in the real world, sites use one of a number of available "top-level domains" (TLD), but instead of ".com", ".net", ".org", etc. (which may or may not exist in the waketime Internet of the Trennisverse), the valid TLDs seen on Sleepnet most often are (in order of frequency):

`.som`
`.zed`
`.nap`

*Note: There may be others...*

Example domain folders might include:

`circuscitysingers.som`
`oldcartoonsbetterthannewcartoons.zed`
`libraryofapricots.nap`



#### Homepages

When *Dreamsettler*'s Nexus browser encounters a path that ends without specifying a filename, ending instead in a domain folder or a subfolder, it will attempt to find "main.stml," followed by "main.stm," in that folder.

This means that in order to reach those pages, thier names do not need to be added to the end of a path.

This is a domain/folder's "homepage."



#### Paths

STML paths use backslashes (`\`) to separate directories. The STML parser in *Dreamsettler* is capable of and will attempt to interpret paths that use forward slashes, errors are possible.

✔️`<image source=dreamsettler.net\~username\images\logo.img />`

❌`<image source=dreamsettler.net/~username/images/logo.img />`



##### Absolute paths

```
<< This page is: somnius.som\main.stm >>
Click <link to=gracefulbobcat.zed>here</> for "gracefulbobcat.zed\main.stm"
```

Absolute paths begin with the name of the domain folder on Sleepnet.



##### Relative paths

```
<< This page is: gracefulbobcat.zed\main.stm >>
Click <link to=\pages\about.stml>here</> for "gracefulbobcat.zed\pages\about.stml"
```

Most paths take this form, starting with a backslash, to represent the folder where the calling page is located.



##### Parent paths

```
<< This page is: gracefulbobcat.zed\pages\about.stml >>
Click <link to=..\main.stm>here</> for "gracefulbobcat.zed\main.stml"
```