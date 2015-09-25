# CSS Refresher Notes

##Positioning

CSS Position allows up to 5 different values. But essentially only 4 values are commonly used.

```css
div {
  position: static; /* default */
  position: relative;
  position: absolute;
  position: fixed;
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
* You use the positioning attributes top, left, bottom and right to set the location. Remember that these values will be relative to the next parent element with relative (or absolute) positioning. If there is no such parent, it will default all the way back up to the <html> element itself meaning it will be placed relatively to the page itself.
* The trade-off, and most important thing to remember, about absolute positioning is that these elements are removed from the flow of elements on the page. An element with this type of positioning is not affected by other elements and it doesn't affect other elements. 

**Fixed:**
* Similar to position absolute, an element that has fixed position is taken out of the document flow.
* The element is positioned relative to the viewport, or the browser window itself.
* Major difference with position absolute is it always takes its positioning relative to the browser window.
* Since the fixed value behaves similar to the absolute value, we can stretch the width of the element to fit the viewport by setting offset values for left and right to zero.

**Inherits:**
* It works as the name implies: The element inherits the value of its parent element. Typically, position property elements do not naturally inherit their parent’s values—the static value is assigned if no position value is given. Ultimately, you can type inherit or the parent element’s value and get the same result.

**Summary**
* Relative positioning will allow you to tweak an element’s position relative to its normal starting point. 
* Absolute positioning will allow you to tweak an element’s position relative to its first non-statically-positioned ancestor (defaults to page bounds if none is found). 
* Both relatively and absolutely positioned items won’t affect the static and fixed items around them (absolutely positioned items are removed from the flow, relatively positioned items occupy their original position).

**Gotchas:**
* You can’t apply both the position property and the float property to the same element. Both are conflicting instructions for what positioning scheme to use. If you add both to the same element expect the one that appears last in your css code to be the one that’s used.
* Margins don’t collapse on absolutely positioned elements. Suppose you have a paragraph with a margin-bottom of 20px applied. Right below the paragraph is an image with a margin-top of 30px applied. The space between the paragraph and the image will not be 50px (20px + 30px), but rather 30px (30px > 20px). This is known as collapsing margins. The two margins combine (or collapse) to become one margin. Absolutely positioned elements do not have margins that collapse, which might make them act differently than expected.

*More reading:*

https://css-tricks.com/absolute-relative-fixed-positioining-how-do-they-differ/

http://alistapart.com/article/css-positioning-101

http://www.barelyfitz.com/screencast/html-training/css/positioning/

##Display
Every element on a web page is a rectangular box. The display property in CSS determines just how that rectangular box behaves.

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
* Accepts padding and margin, but the element still sits inline. It will only push the elements horizontally away, not vertically.
* Inline element does not accept height and width and will be ignored.

**Inline Block:**
* Very similar to inline in that it will set inline with the natural flow of text.
* Difference is that you will be able to set width and height, which will be respected.

**Block:**
* A number of elements are set to block by the browser US stylesheet. They are usually container elements such as div, section and ul. Also text 'blocks' like p and h1.
* Block level elements do not sit inline, but break past them. By default (without setting a width) they take up as much horizontal space as they can.

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
* The main idea behind the flex layout is to give the container the ability to alter its items' width/height (and order) to best fill the available space (mostly to accomodate to all kind of display devices and screen sizes).
* A flex container expands items to fill available free space, or shrinks them to prevent overflow.
* Most importantly, the flexbox layout is direction-agnostic as opposed to the regular layouts (block which is vertically-based and inline which is horizontally-based).
* Flexbox layout is most appropriate to the components of an application, and small-scale layouts, while the Grid layout is intended for larger scale layouts.

**Grid:**
* The grid property in CSS is the foundation of Grid Layout. It aims at fixing issues with older layout techniques like float and inline-block, which both have issues and weren't really designed for page layout.
* Method of using a grid concept to lay out content, providing a mechanism for authors to divide available space for lay out into columns and rows using a set of predictable sizing behaviors.
* Along with the fact this method fixes the issues we encounter with older layout techniques, its main benefit is you can display a page in a way which can differ from the flow order.
* Not much support. Currently only IE 10+ (no Chrome/Firefox etc.)

*More reading:*

https://css-tricks.com/almanac/properties/d/display/

http://learnlayout.com/display.html

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
* Trick: For clearning floats adding a ‘overflow:auto’ to the parent element also does the trick. 
* Note that overflow: auto doesn't clear floats — it causes the element to contain its floats by way of establishing a new block formatting context for them so that they don't intrude to other elements in the parent context.
That is what forces the parent to stretch to contain them. You can only clear a float if you add a clearing element after the float. A parent element cannot clear its floating children.

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

http://designshack.net/articles/css/everything-you-never-knew-about-css-floats/

http://alistapart.com/article/css-floats-101

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

The tilda (~) symbol allows us to target an attribute which has a spaced-separated list of values.

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

http://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048

## CSS3 Properties

**border-radius**

```css
-webkit-border-radius: 4px;
-moz-border-radius: 4px;
border-radius: 4px;
```

Allows rounded corners in elements. Can also be used to create circles.
Support: IE9+

**box-shadow**

```css
-webkit-box-shadow: 1px 1px 3px #292929;
-moz-box-shadow: 1px 1px 3px #292929;
box-shadow: 1px 1px 3px #292929;
```

box-shadow accepts four parameters: x-offset, y-offset, blur, color of shadow.
text-shadow has the same set of properties as well.

It allows you to immediately apply depth to your elements. We can apply multiple shadows by using comma as a separator.

### Selector efficiency

Below is the order of efficiency for selectors. IDs are the most efficient and pseudo classes and pseudo elements are the least efficient.

* id (#myid)
* class (.myclass)
* tag (div, h1, p)
* adjacent sibling (h1 + p)
* child (ul > li)
* descendent (li a)
* universal (*)
* attribute (a[rel=”external”])
* pseudo-class and pseudo element (a:hover, li:first)

*More Reading*

http://vanseodesign.com/css/css-selector-performance/

### Repaints and Reflows

*Repaint* 

Also known as redraw - is what happens whenever something is made visible when it was not previously visible, or vice versa, without altering the layout of the document. An example would be when adding an outline to an element, changing the background color, or changing the visibility style. Repaint is expensive in terms of performance, as it requires the engine to search through all elements to determine what is visible, and what should be displayed.

*Reflow*

A reflow is a more significant change. This will happen whenever the DOM tree is manipulated, whenever a style is changed that affects the layout, whenever the className property of an element is changed, or whenever the browser window size is changed. The engine must then reflow the relevant element to work out where the various parts of it should now be displayed. Its children will also be reflowed to take the new layout of their parent into account. Elements that appear after the element in the DOM will also be reflowed to calculate their new layout, as they may have been moved by the initial reflows. Ancestor elements will also reflow, to account for the changes in size of their children. Finally, everything is repainted.

Reflows are very expensive in terms of performance, and is one of the main causes of slow DOM scripts, especially on devices with low processing power, such as phones. In many cases, they are equivalent to laying out the entire page again.

*Minimal reflow*

Normal reflows may affect the whole document. The more of the document that is reflowed, the longer the reflow will take. Elements that are positioned absolutely or fixed, do not affect the layout of the main document, so if they reflow, they are the only thing that reflows. The document behind them will need to repaint to allow for any changes, but this is much less of a problem than an entire reflow.

So is an animation does not need to be applied to the whole document, it is better if it can be applied only to a positioned element. For most animations, this is all that is needed anyway.

For a comprehensive list of what forces reflows - check Paul Irish's gist [here](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)

---

*Links*

http://designshack.net/articles/css/5-steps-to-drastically-improve-your-css-knowledge-in-24-hours/
