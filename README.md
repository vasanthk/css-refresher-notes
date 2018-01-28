# CSS Refresher Notes

This is a quick refresher of CSS concepts compiled from various articles online. Contributions are always welcome :)

I have linked to most of the articles used, sorry if I missed any. Huge thanks to the community!

**Table of Contents**

- [Positioning](#positioning)
- [Display](#display)
- [Floats](#floats)
- [CSS Selectors](#css-selectors)
- [Selector efficiency](#selector-efficiency)
- [Repaints and Reflows](#repaints-and-reflows)
- [CSS3 Properties](#css3-properties)
- [CSS3 Media queries](#css3-media-queries)
- [Responsive Web Design](#responsive-web-design)
- [CSS3 Transitions](#css3-transitions)
- [CSS Animations](#css-animations)
- [Scalable Vector Graphics (SVG)](#scalable-vector-graphics-svg)
- [CSS Sprites](#css-sprites)
- [Vertical Alignment](#vertical-alignment)
- [Known Issues](#known-issues)

## Positioning

CSS Position allows up to 5 different values. But essentially only 4 values are commonly used.

```css
div {
  position: static; /* default */
  position: relative;
  position: absolute;
  position: fixed;
  position: sticky;
  position: inherit; /* Not very common */
}
```

**Static (default):**
* The only reason you would ever set an element to position: static is to forcefully-remove some positioning that got applied to an element outside of your control. This is fairly rare, as positioning doesn't cascade.

**Relative:**
* Allows nudging elements in different directions with top, right, bottom and left values. Its starting point is where it normally lies in the flow of the document, not the top left of the page.
* When set to position relative, elements take up the same amount of space at the same exact position it was supposed to take as if its position was static.
* It introduces the ability to use z-index on that element, which doesn't really work with statically positioned elements. Even if you don't set a z-index value, this element will now appear on top of any other statically positioned element.
* It limits the scope of absolutely positioned child elements. Any element that is a child of the relatively positioned element can be absolutely positioned within that block.

**Absolute:**
* Position absolute takes the element out of the document flow. This means that it no longer takes up any space like what static and relative does. Think of it as an element with a giant strip of velcro on its back. Just tell it where to stick and it sticks.
* You use the positioning attributes top, left, bottom and right to set the location. Remember that these values will be relative to the next parent element with relative (or absolute) positioning. If there is no such parent, it will default all the way back up to the html element itself meaning it will be placed relatively to the page itself.
* The trade-off, and most important thing to remember, about absolute positioning is that these elements are removed from the flow of elements on the page. An element with this type of positioning is not affected by other elements and it doesn't affect other elements.

**Fixed:**
* Similar to position absolute, an element that has fixed position is taken out of the document flow.
* The element is positioned relative to the viewport, or the browser window itself.
* Major difference with position absolute is it always takes its positioning relative to the browser window.
* Since the fixed value behaves similar to the absolute value, we can stretch the width of the element to fit the viewport by setting offset values for left and right to zero.

**Sticky:**
* This is a hybrid of relative and fixed positioning.
* The element is treated as relative positioned until it crosses a specified threshold, at which point it is treated as fixed positioning.
* For more info, find the demo from the MDN (https://codepen.io/simevidas/pen/JbdJRZ)

**Inherits:**
* It works as the name implies: The element inherits the value of its parent element. Typically, position property elements do not naturally inherit their parent’s values—the static value is assigned if no position value is given. Ultimately, you can type inherit or the parent element’s value and get the same result.

**Summary**
* Relative positioning will allow you to tweak an element’s position relative to its normal starting point.
* Absolute positioning will allow you to tweak an element’s position relative to its first non-statically-positioned ancestor (defaults to page bounds if none is found).
* Both relatively and absolutely positioned items won’t affect the static and fixed items around them (absolutely positioned items are removed from the flow, relatively positioned items occupy their original position).

**Gotchas:**
* You can’t apply both the position property and the float property to the same element. Both are conflicting instructions for what positioning scheme to use. If an element has both float and position:absolute or fixed, the float property won't apply.
* Margins don’t [collapse](http://www.sitepoint.com/web-foundations/collapsing-margins/) on absolutely positioned elements. Suppose you have a paragraph with a margin-bottom of 20px applied. Right below the paragraph is an image with a margin-top of 30px applied.
The space between the paragraph and the image will not be 50px (20px + 30px), but rather 30px (30px > 20px). This is known as collapsing margins. The simplest way to stop the margin collapse from occurring is to add padding or borders to each element.
The two margins combine (or collapse) to become one margin. Absolutely positioned elements do not have margins that collapse, which might make them act differently than expected.

*More reading:*

[Absolute, Relative, Fixed Positioning: How Do They Differ?](https://css-tricks.com/absolute-relative-fixed-positioining-how-do-they-differ/)

[CSS Positioning 101](http://alistapart.com/article/css-positioning-101)

[Learn CSS Positioning in Ten Steps](http://www.barelyfitz.com/screencast/html-training/css/positioning/)

## Display
Every element on a web page is a rectangular [box](http://www.sitepoint.com/web-foundations/css-box-model/). The display property in CSS determines just how that rectangular box behaves.

```css
div {
	display: inline;
	display: inline-block;
	display: block;
	display: run-in;
	display: none;
}

```
* Default value of all elements is inline. Most "User Agent stylesheets" (the default styles the browser applies to all sites) reset many elements to "block"

**Inline:**
* Default value for elements. Think of elements like span, b or em
* Will ignore top and bottom margin/padding settings, but will apply left and right margin/padding. Only moves horizontally, not vertically.
* Is subject to vertical-align property.
* Will ignore width and height properties.
* If floated left or right, will automatically become a block-level element, subject to all block characteristics.

**Inline Block:**
* Very similar to inline to its parents in that it will set inline with the natural flow of text.
* Very similar to block to its children in that it can have a width and height and its content can move vertically.
* Think of a display:block element that has been rendered (or converted to an image) and then placed in the document inline
* Margin and paddings work properly.
* Width and height will be respected.
* There is a "bug" which causes extra margin in inline block elements. See on [Known Issues](#extra-margin-on-inline-block-elements)

**Block:**
* A number of elements are set to block by the browser UA stylesheet. They are usually container elements such as div, section and ul. Also text 'blocks' like p and h1.
* Block level elements do not sit inline, but break past them. If no width is set, will expand naturally to fill its parent container.
* Can have margins and/or padding.
* If no height is set, will expand naturally to fit its child elements (assuming they are not floated or positioned). So, for a block element, it’s not necessary to give it a set width or to give it a width of 100%.
* Ignores the vertical-align property.

**Run-in:**
* Not supported in Firefox + spec not well defined yet.
* To begin to understand it though, it's like if you want a header element to sit inline with the text below it.

**None:**
* Totally removes the element from the page.
* While the element is still in the DOM, it is removed visually and in any other conceivable way (you can't tab to it or it's children , it is ignored by screen readers etc.)

**Table values:**
* There is a whole set of display values that force non-table elements to behave like table-elements.
* It's rare-ish, but it sometimes allows you to be "more semantic" with your code while utilizing the unique positioning powers of tables.

```css
div {
  display: table;
  display: inline-table; /* like a table inside an inline-block */
  display: table-cell;
  display: table-column;
  display: table-colgroup;
  display: table-header-group;
  display: table-row-group;
  display: table-footer-group;
  display: table-row;
  display: table-caption;
}
```

* To use, just mimic normal table structure. Simple example:
```html
<div style="display: table;">
  <div style="display: table-row;">
    <div style="display: table-cell;">
      Gross but sometimes useful.
    </div>
  </div>
</div>
```

**Flexbox:**
* Aims at providing a more efficient way to lay out, align and distribute space among items in a container, even when their size is unknown and/or dynamic (thus the word 'flex')
* The main idea behind the flex layout is to give the container the ability to alter its items' width/height (and order) to best fill the available space (mostly to accommodate to all kind of display devices and screen sizes).
* A flex container expands items to fill available free space, or shrinks them to prevent overflow.
* Most importantly, the flexbox layout is direction-agnostic as opposed to the regular layouts (block which is vertically-based and inline which is horizontally-based).
* Flexbox layout is most appropriate to the components of an application, and small-scale layouts, while the Grid layout is intended for larger scale layouts.
* As with inline-block and inline-table, there is also inline-flex.

*More reading:*

[Flexbox Froggy](http://flexboxfroggy.com/)

[Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

[Flexbox reference](http://tympanus.net/codrops/css_reference/flexbox/)

**Grid:**
* The grid property in CSS is the foundation of Grid Layout. It aims at fixing issues with older layout techniques like float and inline-block, which both have issues and weren't really designed for page layout.
* Method of using a grid concept to lay out content, providing a mechanism for authors to divide available space for lay out into columns and rows using a set of predictable sizing behaviors.
* Along with the fact this method fixes the issues we encounter with older layout techniques, its main benefit is you can display a page in a way which can differ from the flow order.
* An older deprecated version of the spec is implemented in IE10/11. The current version of the spec is present in all other modern browsers (Chrome, Safari, Firefox, and Edge 16+).
* As with inline-block and inline-table, there is also inline-grid.

*More reading:*

[A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)

[Grid Garden](http://cssgridgarden.com/)

[CSS Almanac: Display](https://css-tricks.com/almanac/properties/d/display/)

[The Difference Between “Block” and “Inline”](http://www.impressivewebs.com/difference-block-inline-css/)

[Learn CSS Layout: The "display" property](http://learnlayout.com/display.html)

## Floats
* Floated elements are first laid out according to the normal flow, then taken out of the normal flow and sent as far to the right or left (depending on which value is applied) of the parent element.
In other words, they go from stacking on top of each other to sitting next to each other, given that there is enough room in the parent element for each floated element to sit.
* Generally, a floated element should have an explicitly set width (unless it is a replaced element, like an image). This ensures that the float behaves as expected and helps to avoid issues in certain browsers.
* Non-positioned, non-floated, block-level elements act as if the floated element is not there, since the floated element is out of flow in relation to other block elements. When we use a floated image in a paragraph - the text in the paragraph is inline, so it flows around the floated element.
* The clear property has five values available: left, right, both, inherit, and none. Assigning a value of left says the top edge of this element must sit below any element that has the float: left property applied to it.
* A rule that I have found that works nicely for my layouts is float first. That is, in my HTML, I almost always place my floated elements first in the markup, and before any non-floated elements that my float will interact with,
such as the paragraph in the example above. Most of the time, this gives the desired result.
* Collapsing is when a parent element that contains any number of floated elements doesn’t expand to completely surround those elements in the way it would if the elements were not floated.
* There is a method that allows a parent element to clear itself of any floated elements inside it. It uses a CSS property called overflow with a value of hidden.
Note that the overflow property was not intended for this type of use, and could cause some issues such as hiding content or causing unwanted scrollbars to appear.
* **Trick: For clearing floats adding a ‘overflow:auto’ to the parent element also does the trick.**
* Note that overflow: auto doesn't clear floats — it causes the element to contain its floats by way of establishing a new block formatting context for them so that they don't intrude to other elements in the parent context.
That is what forces the parent to stretch to contain them. You can only clear a float if you add a clearing element after the float. A parent element cannot clear its floating children.
* A better way to clear floats is to add a `clearfix` class to the container element with the following styles
  ```css
  .clearfix:after {
    content: "";
    display: table;
    clear: both;
  }
  ```

**9 Rules governing Floats**

1. Floated elements are pushed to the edge of their containers, no further.

2. Any floated element will either appear next to or below a previous floated element. If the elements are floated left, the second element will appear to the right of the first. If they’re floated right, the second element will appear to the left of the first.

3. A left-floating box can't be further right than a right-floating box.

4. Floated elements can't go higher than their container’s top edge (this gets more complicated when collapsing margins are involved, see original rule).

5. A floated element can't be higher than a previous block level or floated element.

6. A floated element can't be higher than a previous line of inline elements.

7. One floated element next to another floated element can’t stick out past the edge of its container.

8. A floating box must be placed as high as possible.

9. A left-floating box must be put as far to the left as possible, a right-floating box as far to the right as possible. A higher position is preferred over one that is further to the left/right.

**Gotchas**
```html
<img src="http://lorempixum.com/200/200/">
<p>Lorem ipsum...</p>
```
```css
img {
  float: right;
  margin: 20px;
}
```

Any margin that we add to the paragraph is being applied way off to the right of the image. This is because the image is actually inside of the paragraph’s box! This is why it doesn’t increase the space between the image and the paragraph.

*More reading:*

[Everything You Never Knew About CSS Floats](http://designshack.net/articles/css/everything-you-never-knew-about-css-floats/)

[CSS Floats 101](http://alistapart.com/article/css-floats-101)

[Clearing Floats](http://www.sitepoint.com/clearing-floats-overview-different-clearfix-methods/)

## CSS Selectors

```css
div#container > ul {
  border: 1px solid black;
}
```

The difference between the standard X Y and X > Y is that the latter will only select direct children.

```css
ul ~ p {
   color: red;
}
```

This sibling combinator is similar to X + Y, however, it's less strict. While an adjacent selector (ul + p) will only select the first element that is immediately preceded by the former selector, this one is more generalized.
It will select, referring to our example above, any p elements, as long as they follow a ul.

```css
a[href*="google"] {
  color: #1f6053;
}
```

The star designates that the proceeding value must appear somewhere in the attribute's value.

```css
a[href^="http"] {
   background: url(path/to/external/icon.png) no-repeat;
   padding-left: 10px;
}
```

If we want to target all anchor tags that have a href which begins with http, we could use a selector similar to the snippet shown above.

```css
a[href$=".jpg"] {
   color: red;
}
```

Again, we use a regular expressions symbol, $, to refer to the end of a string. In this case, we're searching for all anchors which link to an image -- or at least a url that ends with .jpg

```css
a[data-info~="image"] {
   border: 1px solid black;
}
```

The tilde (~) symbol allows us to target an attribute which has a spaced-separated list of values.

```css
div:not(#container) {
   color: blue;
}
```

This selects all divs, except for the one which has an id of container.

```css
p::first-line {
   font-weight: bold;
   font-size: 1.2em;
}
```

We can use pseudo elements (designated by ::) to style fragments of an element, such as the first line, or the first letter. This works only for **block level elements**.

```css
li:nth-child(3) {
   color: red;
}
```

nth child pseudo classes target specific elements in the stack. It accepts an integer as a parameter, however, this is not zero-based. If you wish to target the second list item, use li:nth-child(2).
We can even use this to select a variable set of children. For example, we could do li:nth-child(4n) to select every fourth list item.

```css
li:nth-last-child(2) {
   color: red;
}
```

What if you had a huge list of items in a ul, and only needed to access, say, the 2nd to the last item out of 10? Rather than doing li:nth-child(8), you could instead use the nth-last-child pseudo class.

```css
li:first-child {
    border-top: none;
}

li:last-child {
   border-bottom: none;
}
```

This is particularly useful when assigning border and padding/margin styles for a table list.

*More reading:*

[The 30 CSS Selectors you Must Memorize](http://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048)

## Selector efficiency

The following list of selectors is in decreasing order of specificity:
* ID selectors (e.g., #example).
* Class selectors (e.g., .example), attributes selectors (e.g., [type="radio"]) and pseudo-classes (e.g., :hover).
* Type selectors (e.g., h1) and pseudo-elements (e.g., :before).
* Universal selector (*), combinators (+, >, ~, ' ') and negation pseudo-class (:not()) have no effect on specificity. (The selectors declared inside :not() do, however.)

Inline styles added to an element (e.g., style="font-weight:bold") always overwrite any styles in external stylesheets and thus can be thought of as having the highest specificity.

Below is a longer list:
* id (#myid)
* class (.myclass)
* tag (div, h1, p)
* adjacent sibling (h1 + p)
* child (ul > li)
* descendant (li a)
* universal (*)
* attribute (a[rel=”external”])
* pseudo-class and pseudo element (a:hover, li:first)

*More Reading*

[CSS Selectors: Should You Optimize Them To Perform Better?](http://vanseodesign.com/css/css-selector-performance/)

## Repaints and Reflows

**Repaint**

Also known as redraw - is what happens whenever something is made visible when it was not previously visible, or vice versa, without altering the layout of the document. An example would be when adding an outline to an element, changing the background color, or changing the visibility style. Repaint is expensive in terms of performance, as it requires the engine to search through all elements to determine what is visible, and what should be displayed.

**Reflow**

A reflow is a more significant change. This will happen whenever the DOM tree is manipulated, whenever a style is changed that affects the layout, whenever the className property of an element is changed, or whenever the browser window size is changed. The engine must then reflow the relevant element to work out where the various parts of it should now be displayed. Its children will also be reflowed to take the new layout of their parent into account. Elements that appear after the element in the DOM will also be reflowed to calculate their new layout, as they may have been moved by the initial reflows. Ancestor elements will also reflow, to account for the changes in size of their children. Finally, everything is repainted.

Reflows are very expensive in terms of performance, and is one of the main causes of slow DOM scripts, especially on devices with low processing power, such as phones. In many cases, they are equivalent to laying out the entire page again.

**Minimal reflow**

Normal reflows may affect the whole document. The more of the document that is reflowed, the longer the reflow will take. Elements that are positioned absolutely or fixed, do not affect the layout of the main document, so if they reflow, they are the only thing that reflows. The document behind them will need to repaint to allow for any changes, but this is much less of a problem than an entire reflow.

So if an animation does not need to be applied to the whole document, it is better if it can be applied only to a positioned element. For most animations, this is all that is needed anyway.

*What causes a reflow*

* Resizing a window.
* Changing the font.
* Adding or removing a stylesheet.
* Content changes, such as a user typing text in an input box.
* Activation of CSS pseudo classes such as :hover (In IE the activation of the pseudo class of a sibling).
* Manipulating the class attribute.
* A script manipulating the DOM
* Calculating offsetWidth and offsetHeight
* Setting a property of the style attribute.

For a comprehensive list of what forces reflows - check Paul Irish's gist [here](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)

**How to minimize the effect of reflows on performance**

* Change classes on the element you wish to style (as low in the DOM tree as possible).
* Avoid setting multiple inline styles.
* Apply animations to elements that are position fixed or absolute.
* Trade smoothness for speed.
* Avoid tables for layout. Even a minor change to a cell will cause reflows on all other nodes of the table.
* Avoid JavaScript expressions in the CSS (IE only).

**Note**

* Sweating over the selectors used in modern browsers is futile. Most selection methods are now so fast it’s really not worth spending much time over. Furthermore, there is disparity across browsers of what the slowest selectors are anyway. Look here last to speed up your CSS.
* Excessive unused styles are likely to cost more, performance wise, than any selectors you chose so look to tidy up there second. 3000 lines that are unused or surplus on a page are not even that uncommon. While it’s common to bunch all the styles up into a great big single styles.css, if different areas of your site/web application can have different (additional) stylesheets added (dependency graph style), that may be the better option.

*More Reading*

[Reflows & Repaints: CSS performance making your JavaScript slow?](http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow/)

[Writing efficient CSS selectors](http://csswizardry.com/2011/09/writing-efficient-css-selectors/)

[Why do browsers match CSS selectors from right to left?](http://stackoverflow.com/questions/5797014/why-do-browsers-match-css-selectors-from-right-to-left)

[CSS performance revisited: selectors, bloat and expensive styles](http://benfrain.com/css-performance-revisited-selectors-bloat-expensive-styles/)

## CSS3 Properties

**border-radius**

```css
-webkit-border-radius: 4px;
-moz-border-radius: 4px;
border-radius: 4px;
```

Allows rounded corners in elements. Can also be used to create circles.

*Support: IE9+*

**box-shadow**

```css
-webkit-box-shadow: 1px 1px 3px #292929;
-moz-box-shadow: 1px 1px 3px #292929;
box-shadow: 1px 1px 3px #292929;
```

The box-shadow property allows us to easily implement multiple drop shadows (outer or inner) on box elements, specifying values for color, size, blur and offset.

box-shadow accepts four parameters: x-offset, y-offset, blur, color of shadow.

* The first value defines the horizontal offset of the shadow, with a positive value offsetting the shadow to the right of the element, and a negative value to the left.
* The second value defines the vertical offset of the shadow, with a positive value offsetting the shadow from the bottom of the element, and a negative value from the top.
* If supplied, an optional third value defines the blur distance of the shadow. Only positive values are allowed, and the larger the value, the more the shadow’s edge is blurred.
* An optional fourth value can be supplied to define the spread distance of the shadow. A positive value will cause the shadow shape to expand in all directions, while a negative value will cause the shadow shape to contract.

An optional ‘inset’ keyword can be supplied, preceding the length and color values. If present, this keyword causes the shadow to be drawn inside the element.

```css
-webkit-box-shadow: 0 0 20px #333 inset;
-moz-box-shadow: 0 0 20px #333 inset;
box-shadow: 0 0 20px #333 inset;
```

text-shadow has the same set of properties as well.

It allows you to immediately apply depth to your elements. We can apply multiple shadows by using comma as a separator.

*Support: IE9+*

*More reading:*

[Box-shadow, one of CSS3’s best new features](http://www.css3.info/preview/box-shadow/)

[CSS Almanac: box-shadow](https://css-tricks.com/almanac/properties/b/box-shadow/)

**text-shadow**

```css
color: #fff /* text color to white */
text-shadow: 0 0 50px #333;
```

Text shadows are like box shadows except that they are shadows for text rather than the whole element. Luckily, there is no vendor prefix necessary for text shadow.

The options for text shadow are the same as for box-shadow except that there is *no inset* text shadow support.

As with box shadows, it is possible to have multiple text shadows just by separating them with commas. Here is an example that creates a flaming text effect.

```css
text-shadow: 0 0 4px #ccc,
             0 -5px 4px #ff3,
             2px -10px 6px #fd3,
             -2px -15px 11px #f80,
             2px -18px 18px #f20;
```

*Support: IE10+*

**Gradients**

Just as you can declare the background of an element to be a solid color in CSS, you can also declare that background to be a gradient. Using gradients declared in CSS, rather using an actual image file, is better for control and performance.

Gradients are typically one color that fades into another, but in CSS you can control every aspect of how that happens, from the direction to the colors (as many as you want) to where those color changes happen.

```css
.gradient {
  /* can be treated like a fallback */
  background-color: red;

  /* will be "on top", if browser supports it */
  background-image: linear-gradient(red, orange);

  /* these will reset other properties, like background-position, but it does know what you mean */
  background: red;
  background: linear-gradient(red, orange);
}
```

*Support: IE10+*

***Linear Gradient***

Perhaps the most common and useful type of gradient is the linear-gradient(). The gradients "axis" can go from left-to-right, top-to-bottom, or at any angle you chose. Not declaring an angle will assume top-to-bottom.
To make it left-to-right, you pass an additional parameter at the beginning of the linear-gradient() function starting with the word "to", indicating the direction, like "to right". This "to" syntax works for corners as well.
You aren't limited to just two colors either. In fact you can have as many comma-separated colors as you want.
You can also declare where you want any particular color to "start". Those are called "color-stops". Say you wanted yellow to take up the majority of the space, but red only a little bit in the beginning, you could make the yellow color-stop pretty early:

```css
.gradient {
  height: 100px;
  background-image:
    linear-gradient(
      to right,
      red,
      yellow 10%
    );
}
```

***Gotchas***

* We tend to think of gradients as fading colors, but if you have two color stops that are the same, [you can make a solid color instantly change to another solid color](http://codepen.io/chriscoyier/pen/csgoD). This can be useful for declaring a full-height background that simulates columns.

* There are three different syntaxes that browsers have supported:

  - Old: original WebKit-only way, with stuff like from() and color-stop()

  - Tweener: old angle system, e.g. "left"

  - New: new angle system, e.g. "to right"

The way degrees work in the OLD vs NEW syntax is a bit different. The OLD (and TWEENER - usually prefixed) way defines 0deg and left-to-right and proceeds counter-clockwise, while the NEW (usually unprefixed) way defines 0deg as bottom-to-top and proceeds clockwise.

OLD (or TWEENER) = (450 - new) % 360

or even simpler:

NEW = 90 - OLD
OLD = 90 - NEW

like:

OLD linear-gradient(135deg, red, blue)
NEW linear-gradient(315deg, red, blue)

***Radial Gradients***

Radial gradient differ from linear in that they start at a single point and emanate outwards. Gradients are often used to simulate a lighting, which as we know isn't always straight, so they can be useful to make a gradient seem even more natural.

The default is for the first color to start in the (center center) of the element and fade to the end color toward the edge of the element. The fade happens at an equal rate no matter which direction. The default gradient shape is an ellipse.

The possible values for fade  are: closest-corner, closest-side, farthest-corner, farthest-side. You can think of it like: "I want this radial gradient to fade from the center point to the __________, and everywhere else fills in to accommodate that."
A radial gradient doesn't have to start at the default center either, you can specify a certain point by using "at ______" as part of the first parameter.

***Gotchas***

* There are again three different syntaxes that browsers have supported:

  - Old: Prefixed with -webkit-, stuff like from() and color-stop()

  - Tweener: First param was location of center. That will completely break now in browsers that support new syntax unprefixed, so make sure any tweener syntax is prefixed.

  - New: Verbose first param, like "circle closest-corner at top right"

* It is recommended to used autoprefixers like [postcss](https://github.com/postcss/autoprefixer) to handle vendor prefixes to make it work across different browsers consistently.

***Repeating Gradients***

The size of the gradient is determined by the final color stop. If that's at 20px, the size of the gradient (which then repeats) is a 20px by 20px square.

```css
.repeat {
  background-image:
    repeating-linear-gradient(
      45deg,
      yellow,
      yellow 10px,
      red 10px,
      red 20px /* determines size */
    );
}
```

They can be used with both linear and radial varieties.

There is a trick, with non-repeating gradients, to create the gradient in such a way that if it was a little tiny rectangle, it would line up with other little tiny rectangle versions of itself to create a repeating pattern. So essentially create that gradient and set the background-size to make that little tiny rectangle. That made it easy to make stripes, which you could then rotate or whatever.

*More reading:*

[CSS Gradients](https://css-tricks.com/css3-gradients/)

## CSS3 Media queries

CSS Media Queries are a feature in CSS3 which allows you to specify when certain CSS rules should be applied. This allows you to apply a special CSS for mobile, or adjust a layout for print.

There are three ways to invoke media-query-dependent styles.

* First of all, as stylesheets in the link element of HTML or XHTML:

```html
<link rel="stylesheet" type="text/css" media="all and (color)" href="/style.css">
```

Secondly, in XML:

```html
<?xml-stylesheet media="all and (color)" rel="stylesheet" href="/style.css" ?>
```

And finally, in CSS stylesheets using @import rules:

```css
@import url("/style.css") all and (color);
```

Or using @media rules:

```css
@media all and (color) { /* one or more rule sets... */ }
```

There are currently 13 media features catered for in the specification: width, height, device-width, device-height, orientation, aspect-ratio,device-aspect-ratio, color, color-index, monochrome, resolution, scan, and grid. All but orientation, scan, and grid can accept min- and max- prefixes as well.

## Responsive Web Design

***Setting the viewport***

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

A meta viewport element gives the browser instructions on how to control the page's dimensions and scaling.

The width=device-width part sets the width of the page to follow the screen-width of the device (which will vary depending on the device).

The initial-scale=1.0 part sets the initial zoom level when the page is first loaded by the browser.

***Gotchas***

1. Do NOT use large fixed width elements - If an image is displayed at a width wider than the viewport it can cause the viewport to scroll horizontally.

2. Do NOT let the content rely on a particular viewport width to render well - Since screen dimensions and width in CSS pixels vary widely between devices, content should not rely on a particular viewport width to render well.

3. Use CSS media queries to apply different styling for small and large screens - Setting large absolute CSS widths for page elements, will cause the element to be too wide for the viewport on a smaller device. Instead, consider using relative width values, such as width: 100%. Also, be careful of using large absolute positioning values. It may cause the element to fall outside the viewport on small devices.

***Building a Responsive Grid-View***

A responsive grid-view often has 12 columns, and has a total width of 100%, and will shrink and expand as you resize the browser window.

First ensure that all HTML elements have the [box-sizing](https://css-tricks.com/box-sizing/) property set to border-box. This makes sure that the padding and border are included in the total width and height of the elements.

Add the following code in your CSS:

```css
* {
    box-sizing: border-box;
}
```

***Responsive Images***

Images will be responsive and scale up and down if the width property is set to 100%.
**A better option would be to set max-width property to 100% since the image will scale down if it has to, but never scale up to be larger than its original size.**

Background images can also respond to resizing and scaling.

* If the background-size property is set to "contain", the background image will scale, and try to fit the content area. However, the image will keep its aspect ratio
* If the background-size property is set to "100% 100%", the background image will stretch to cover the entire content area.
* If the background-size property is set to "cover", the background image will scale to cover the entire content area. The "cover" value keeps the aspect ratio, and some part of the background image may be clipped

A large image can be perfect on a big computer screen, but useless on a small device. To reduce the load, you can use media queries to display different images on different devices.

You can use the media query min-device-width, instead of min-width, which checks the device width, instead of the browser width. Then the image will not change when you resize the browser window:

```css
/* For devices smaller than 400px: */
body {
    background-image: url('img_smallflower.jpg');
}

/* For devices 400px and larger: */
@media only screen and (min-device-width: 400px) {
    body {
        background-image: url('img_flowers.jpg');
    }
}
```

HTML5 introduced the picture element, which lets you define more than one image. (No IE support :(  .. only Edge 13+)

The picture element works similar to the video and audio elements. You set up different sources, and the first source that fits the preferences is the one being used:

```css
<picture>
  <source srcset="img_smallflower.jpg" media="(max-width: 400px)">
  <source srcset="img_flowers.jpg">
  <img src="img_flowers.jpg" alt="Flowers">
</picture>
```

The srcset attribute is required, and defines the source of the image. The media attribute is optional, and accepts the media queries you find in CSS @media rule. You should also define an img element for browsers that do not support the picture element (good fallback option).

***Responsive Videos***

* Using The width Property

  - If the width property is set to 100%, the video player will be responsive and scale up and down. However, it can be scaled up to be larger than its original size. A better solution, in many cases, will be to use the max-width property instead.

  ```css
  video {
      max-width: 100%;
      height: auto;
  }
  ```

*More reading:*

[w3schools - Responsive Web Design](http://www.w3schools.com/css/css_rwd_intro.asp)

[9 basic principles of responsive web design](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)

## CSS3 Transitions

A CSS transition allows you to change the property of an element over a given duration that you set.

* transition-property: The property to be transitioned (in this case, the background property)
* transition-duration: How long the transition should last (0.3 seconds)
* transition-timing-function: How fast the transition happens over time (ease)

The timing function value allows the speed of the transition to change over time by defining one of six possibilities: *ease, linear, ease-in, ease-out, ease-in-out, and cubic-bezier* (which allows you to define your own timing curve).

You may want the transition to also happen on the :focus or :active pseudo-classes of the link as well. Instead of having to add the transition property stack to each of those declarations, the transition instructions are attached to the normal state and therefore declared only once.

```css
a.foo {
  padding: 5px 10px;
  background: #9c3;
  -webkit-transition: background .3s ease,
    color 0.2s linear;
  -moz-transition: background .3s ease,
    color 0.2s linear;
  -o-transition: background .3s ease, color 0.2s linear;
  transition: background .3s ease, color 0.2s linear;
}
a.foo:hover,
a.foo:focus {
  color: #030;
  background: #690;
}
```

Let’s say that along with the background color, we also want to change the link’s text color and transition that as well. We can do that by stringing multiple transitions together, separated by a comma. Each can have their varying duration and timing functions. An alternative to listing multiple properties is using the all value. This will transition all available properties.

Another basic use of changing states is to change the background of an input box on focus.

```css
input.ourInputBox:focus{
 -webkit-transition:background-color 0.5s linear;
 background:#CCC;
}
```

This time, we put the transition declaration into the hover state, so that we aren’t adding additional unnecessary classes to the CSS. We apply the transition once the input box acquires focus.

*More reading:*

[Understanding CSS3 Transitions](http://alistapart.com/article/understanding-css3-transitions)

[CSS Fundamentals: CSS3 Transitions](http://code.tutsplus.com/tutorials/css-fundamentals-css3-transitions--pre-10922)

## CSS Animations

While CSS transitions are all about altering element properties as they move from state to state, CSS animations are dependent on keyframes and animation properties.

* keyframes: Used to define the styles an element will have at various times.
* animation properties: Used to assign @keyframes to a specific element and determine how it is animated.

***Keyframes***

Keyframes are the foundation of CSS animations. They define what the animation looks like at each stage of the animation timeline. Each @keyframes is composed of:

- Name of the animation: A name that describes the animation, for example, bounceIn.

- Stages of the animation: Each stage of the animation is represented as a percentage. 0% represents the beginning state of the animation. 100% represents the ending state of the animation. Multiple intermediate states can be added in between.

- CSS Properties: The CSS properties defined for each stage of the animation timeline.

Let’s take a look at a simple @keyframes I’ve named “bounceIn”. This @keyframes has three stages. At the first stage (0%), the element is at opacity 0 and scaled down to 10 percent of its default size, using CSS transform scale. At the second stage (60%) the element fades in to full opacity and grows to 120 percent of its default size. At the final stage (100%), it scales down slightly and returns to its default size.

The @keyframes are added to your main CSS file.

```css
@keyframes bounceIn {
  0% {
    transform: scale(0.1);
    opacity: 0;
  }
  60% {
    transform: scale(1.2);
    opacity: 1;
  }
  100% {
    transform: scale(1);
  }
}
```

***Animation Properties***

Once the @keyframes are defined, the animation properties must be added in order for your animation to function.

Animation properties do two things:

* They assign the @keyframes to the elements you want to animate.
* They define how it is animated.

The animation properties are added to the CSS selectors(or elements) that you want to animate, You must add the following two animation properties for the animation to take effect.

* animation-name: The name of the animation defined in the @keyframes.
* animation-duration: The duration of the animations, in seconds (eg. 5s) or milliseconds (eg. 200ms).

Continuing with the above bounceIn example, we’ll add animation-name and animation-duration to the div that we want to animate.

```css
div {
  animation-duration: 2s;
  animation-name: bounceIn;
}
```

Shorthand syntax:
```css
div {
  animation: bounceIn 2s;
}
```

By adding both the @keyframes and the animation properties, we have a simple animation!

In addition to the required animation-name and animation-duration properties, you can further customize and create complex animations using the following properties:

* animation-timing-function – specifies the speed curve of the animation.
* animation-delay – specifies a delay for the start of an animation.
* animation-iteration-count – specifies the number of times an animation should be played.
* animation-direction – specifies whether an animation should play in reverse direction or alternate cycles.
* animation-fill-mode – specifies a style for the element when the animation is not playing. Such as when it is finished or when it has a delay.
* animation-play-state – specifies whether the animation is running or paused.

Order used in shorthand syntax:

animation: [animation-name] [animation-duration] [animation-timing-function]
[animation-delay] [animation-iteration-count] [animation-direction]
[animation-fill-mode] [animation-play-state];

To add multiple animations to a selector, you simply separate the values with a comma. Here’s an example:

```css
.div {
  animation: slideIn 2s, rotate 1.75s;
}
```

*More reading:*

[An Introduction to CSS Transitions & Animations](https://www.elegantthemes.com/blog/tips-tricks/an-introduction-to-css-transitions-animations)

[CSS Animation for Beginners](https://robots.thoughtbot.com/css-animation-for-beginners)

[Simple CSS3 Transitions, Transforms, & Animations Compilation](http://callmenick.com/post/simple-css3-transitions-transforms-animations-compilation)

[Learn to Code Advanced HTML & CSS](http://learn.shayhowe.com/advanced-html-css/performance-organization/)

## Scalable Vector Graphics (SVG)

SVG stands for scalable vector graphics. It is an XML-based format that supports animation and interactivity.
In other words, SVG is a technology that allows us to create graphics by writing a code. Moreover, it is scalable. While non-vector graphics like PNG and JPEG have fixed sizes and cannot be scaled without loosing quality, SVG can be mathematically scaled to any size.

We can use ".svg" file in our code by setting it as an image source just like any other image "< img src='say_hello.svg'>". But it's not so interesting.
One of the greatest things about SVG is that it is actually text file in XML format. So we can open it, read it and interact with it. We can take our SVG code, put in DOM and play with it - change elements parameters like position, background color or font family using JavaScript and CSS. Moreover, each element of SVG can be animated. And it's really awesome.

So, basically, we should always use SVG graphics instead of PNG or JPEG, when we are talking about basic shapes, logos and vector graphic.

Here's a simple red circle SVG
```css
<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" fill="red" />
</svg>
```

***Common element parameters***

Usually graphs starts (x,y = 0) from bottom left corner. In SVG start point is top left corner.

Each SVG shape (element) has some basic parameters:
* X - X coordinate of top left corner of the element
* Y - Y coordinate of top left corner of the element
* Fill - element background color.
* Fill-opacity - opacity of background color
* Stroke - element border color.
* Stroke-width - element border size.

***Pros:***
  - Resolution independent: Scalability without changing the image quality. It is widely used for devices with screens Retina and those close to them.
  - Small size. SVG image elements take up much less space than their twins created in raster format.
  - Flexibility. With CSS, you can quickly change the graphics settings on the site, such as background color or the position of the logo on the page. To do this, you can edit the file in any text editor.
  - It’s possible to view the contents of the SVG file in any browser (IE, Chrome, Opera, FireFox, Safari, etc.).
  - No unnecessary HTTP requests, unlike using an img tag
  - SEO friendly: text labels, descriptions can be searched by search engines.
  - We have scripting control for custom interactive events and animation.

***Cons:***
  - The file size is growing very fast, if the object consists of a large number of small elements.
  - It’s impossible to read a part of the graphic object, only the entire object and it slows you down.

*Support: IE9+*

*More reading:*

[Introduction to SVG](http://tonyfreed.com/blog/introduction-to-svg)

[An introduction to SVG](http://engageinteractive.co.uk/blog/an-introduction-to-svg)

[The Simple Intro to SVG Animation](http://davidwalsh.name/svg-animation)

[Icon fonts vs SVG - which is the best option?](http://www.creativebloq.com/web-design/icon-fonts-vs-svg-101413211)

*Videos:*

[Using SVG - Intro](https://www.youtube.com/watch?v=vvuH6qS2M5Q)

[SVG Tutorials - Playlist](https://www.youtube.com/watch?v=PQxtlY19kto&list=PLL8woMHwr36F2tCFnWTbVBQAGQ6nTcXOO)

[Working with SVG, A Primer - Sara Soueidan](https://www.youtube.com/watch?v=uKNX23lvnPo)

[You Don't Know SVG - Dmitry Baranovskiy](https://www.youtube.com/watch?v=SeLOt_BRAqc)

[SVG is for Everybody - Chris Coyier](https://www.youtube.com/watch?v=w83XRCkMtHQ)

[Building Better Interfaces With SVG - Sara Soueidan](https://www.youtube.com/watch?v=lMFfTRiipOQ)

[SVG Lessons I Learned The Hard Way - Sara Soueidan](https://www.youtube.com/watch?v=NkLDuPf5P0A)

## CSS Sprites

​A CSS sprite is a performance optimization technique that combines multiple images into a single image called a sprite sheet or tile set. Sprites reduce network congestion by reducing the number of downloads needed to render a web page.

While combining several images into one with sprites, all images are loaded with a single HTTP request. Browsers limit the number of concurrent requests a site can make and HTTP requests require a bit of handshaking. Thus, sprites are important for the same reasons that minifying and concatenating CSS and JavaScript are important.

**Using Sprites in CSS**

You set the same background-image on several CSS classes and set the background position and dimensions of the individual classes to display a single portion of the sprite.

```css
.flags-canada, .flags-mexico, .flags-usa {
  background-image: url('../images/flags.png');
  background-repeat: no-repeat;
}

.flags-canada {
  height: 128px;
  background-position: -5px -5px;
}

.flags-usa {
  height: 135px;
  background-position: -5px -143px;
}

.flags-mexico {
  height: 147px;
  background-position: -5px -288px;
}
```
**Benefits**

​CSS sprites reduce overall page load time while giving enterprises more control over the performance of their website.

* Users experience faster page load times since images appear as soon as the sprite sheet is downloaded.
* Enterprises see greater throughput and lower resource usage as fewer HTTP requests are made, reducing the amount of network congestion.

*More reading:*

[CSS Sprites: What They Are, Why They’re Cool, and How To Use Them](https://css-tricks.com/css-sprites/)

[What are CSS Sprites](https://www.maxcdn.com/one/visual-glossary/css-sprites/)

[w3schools - Image Sprites](http://www.w3schools.com/css/css_image_sprites.asp)

## Vertical Alignment

Vertical alignment is one of the main reasons some people think CSS is confusing. You wonder, I guess, why is it so *hard* to align content vertically with CSS? Why don't they create a property that does it automatically? _Why does `vertical-align` not work for me?!_

About the `vertical-align` question: this property is only for aligning inline or table-cell elements, hope it ends your doubt about it.

And about being hard, well, turns out it is not that hard, but there are some questions you have to do to yourself when wanting do align something vertically:

* Is it a inline or block-level element?
* If it's a text, is it single or multi-line?
* Do you always know the height of the content?
* Can you use CSS3?

**The line-height method:**

This method can be used when you're vertically aligning a single-line text, single-line inline-level element or a image.

Let's say you have the following HTML:

```html
  <div class="parent">
    <span class="child">Hello!</span>
  </div>
```

You'll then align the `.child` content with:

```css
  .parent {
    height: 150px;
  }

  .child {
    line-height: 150px;
  }
```

But if the content of `.child` is directly inside `.parent` like this:

```html
  <div class="parent">
    Hello!
  </div>
```

You then can put the `line-height` property on it:

```css
  .parent {
    height: 150px;
    line-height: 150px;
  }
```

If, instead of a `<span>` you have a `<img>`:

```html
  <div class="parent">
    <img src="image.jpg" class="child-image"/>
  </div>
```

You got to change to:

```css
  .parent {
    height: 150px;
    line-height: 150px;
  }

  .child-image {
    vertical-align: middle; /* Aha! Here it does work :) */
  }
```

**The CSS table method:**

This method consists in simulating a table with CSS. Let's say you have the given HTML:

```html
  <div class="parent">
    <div class="child">
      Hello, CSS table method!
    </div>
  </div>
```

You'll align `.child` with:

```css
  .parent {
    display: table;
  }

  .child {
    display: table-cell;
    vertical-align: middle; /* Yep, it works here too! */
  }
```

This way, it doesn't matter what's the height of `.parent`, `.child` will always be vertically aligned.

Sadly this method does not work with older versions of Internet Explorer.

**The absolute position method:**

This method has two ways to be implemented:

1 - If you know the height of the element you want to align

2 - If you don't know this height (only works if you can use CSS3)

Let's say you have the following HTML:

```html
  <div class="parent">
    <div class="child">
      Hello, absolute position method!
    </div>
  </div>
```

With the _first_ way, you can align `.child`with:

```css
  .parent {
    position: relative;
    height: 300px; /* It is important that parent have a height different from `auto` */
  }

  .child {
    position: absolute;
    top: 50%;
    height: 50px;
    margin-top: -25px;
  }
```

And with the _second_ way, you should do this:

```css
  .parent {
    position: relative;
    height: 300px; /* It is important that parent have a height different from `auto` */
  }

  .child {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
  }
```

There are the main ways to do it, but you can still vertically align using flexbox, padding, stretching, etc.

*More reading:*

[Centering in CSS: A Complete Guide](https://css-tricks.com/centering-css-complete-guide/)

[6 Methods For Vertical Centering With CSS](http://vanseodesign.com/css/vertical-centering/)

[Vertical align anything with just 3 lines of CSS](http://zerosixthree.se/vertical-align-anything-with-just-3-lines-of-css/)

[How to center in CSS](http://howtocenterincss.com/)

---

## Known Issues

### Extra margin on inline-block elements
Let's suppose you need to create a list and the items should be side by side horizontally,
without any spacing between them.

The code could be something like that:

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>
```

```css
li {
  display: inline-block;
  background: red;
}
```

Then, you see on the browser. Yeah, it seems to work properly, except by the extra spacing between the items.
You don't ask for that, and, even adding `margin: 0` in the code, the problem is still there.

The problem occurs because when you use display inline-block, the whitespace in the HTML becomes
a visual space on the browser.

So, let's take a look in some ways to solve that:

#### No spaces in the HTML
So, if the whitespace is the problem, let's remove it.

```html
  <ul>
    <li>Item 1</li><li>Item 2</li><li>Item 3</li>
  </ul>
```
#### Comments in the HTML
It works like the previous solution, but instead you literally remove the whitespaces,
you use comments to do that.

```html
<ul>
   <li>Item content</li><!--
--><li>Item content</li><!--
--><li>Item content</li>
</ul>
```

#### font-size: 0 on parent element
You just add font-size: 0, on their parent which will remove the whitespaces,
and then you set the right font size on inline block element.

```css
 ul {
   font-size: 0;
 }

 li {
   display: inline-block;
   background: red;
   font-size: 14px;
 }
```

#### Negative margin on inline block elements
Pretty clear, the extra margin rendered is 4px, so, let's add a 4px of negative margin in the element.

```css
li {
  display: inline-block;
  margin: 0 -4px;
}
```

*More reading:*

[Fighting the Space Between Inline Block Elements](https://css-tricks.com/fighting-the-space-between-inline-block-elements/)

[Remove Whitespace Between Inline-Block Elements](http://davidwalsh.name/remove-whitespace-inline-block)

[CSS display inline-block Extra Margin/Space](https://tylercipriani.com/2012/08/01/display-inline-block-extra-margin.html)

---

*CSS Links*

[CSS for Devs - Slides](http://elijahmanor.com/talks/css-for-devs/#/)

[5 Steps to Drastically Improve Your CSS Knowledge in 24 Hours](http://designshack.net/articles/css/5-steps-to-drastically-improve-your-css-knowledge-in-24-hours/)

[CSS3 Mastery](http://code.tutsplus.com/series/css3-mastery--net-18126)

[Getting to Work with CSS3 Power Tools](http://code.tutsplus.com/tutorials/getting-to-work-with-css3-power-tools--net-13894)

[CSS pro tips](https://github.com/AllThingsSmitty/css-protips)
