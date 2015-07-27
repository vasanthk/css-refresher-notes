# CSS Refresher Notes

##Positioning:

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
* Allows nudging elements in different directions with top, right, bottom and left values.
* When set to position relative, elements take up the same amount of space at the same exact position it was supposed to take as if its position was static.
* It introduces the ability to use z-index on that element, which doesn't really work with statically positioned elements. Even if you don't set a z-index value, this element will now appear on top of any other statically positioned element. 
* It limits the scope of absolutely positioned child elements. Any element that is a child of the relatively positioned element can be absolutely positioned within that block. 

**Absolute:**
* Position absolute takes the element out of the document flow. This means that it no longer takes up any space like what static and relative does. Think of it as an element with a giant strip of velcro on its back. Just tell it where to stick and it sticks. 
* You use the positioning attributes top, left bottom and right to set the location. Remember that these values will be relative to the next parent element with relative (or absolute) positioning. If there is no such parent, it will default all the way back up to the <html> element itself meaning it will be placed relatively to the page itself.
* The trade-off, and most important thing to remember, about absolute positioning is that these elements are removed from the flow of elements on the page. An element with this type of positioning is not affected by other elements and it doesn't affect other elements. 

**Fixed:**
* Similar to position absolute, an element that has fixed position is taken out of the document flow.
* The element is positioned relative to the viewport, or the browser window itself.
* Major difference with position absolute is it always takes its positioning relative to the browser window.

**Inherits:**
* It works as the name implies: The element inherits the value of its parent element. Typically, position property elements do not naturally inherit their parent’s values—the static value is assigned if no position value is given. Ultimately, you can type inherit or the parent element’s value and get the same result.

**Gotchas:**
* You can’t apply both the position property and the float property to the same element. Both are conflicting instructions for what positioning scheme to use. If you add both to the same element expect the one that appears last in your css code to be the one that’s used.
* Margins don’t collapse on absolutely positioned elements. Suppose you have a paragraph with a margin-bottom of 20px applied. Right below the paragraph is an image with a margin-top of 30px applied. The space between the paragraph and the image will not be 50px (20px + 30px), but rather 30px (30px > 20px). This is known as collapsing margins. The two margins combine (or collapse) to become one margin. Absolutely positioned elements do not have margins that collapse, which might make them act differently than expected.

*More reading:*

https://css-tricks.com/absolute-relative-fixed-positioining-how-do-they-differ/

http://alistapart.com/article/css-positioning-101

http://www.barelyfitz.com/screencast/html-training/css/positioning/

##Display
Every element on a web page is a rectangualr box. The display property in CSS determines just how that rectangular box behaves.

```css
div {
	display: inline;		// Default of all elements, unless UA stylesheet overrides
	display: inline-block;	// Characteristics of a block, but sits on a line
	display: block;			// UA stylesheet makes things like <div> and <section> block
	display: run-in;		// Not particularly well supported or common
	display: none;			// Hide
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
* Not much support. Currently only IE 10+ (no chrome/firefox etc.)







