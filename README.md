# CSS Refresher Notes

##CSS positioning:

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

*More reading:*

http://alistapart.com/article/css-positioning-101

http://www.barelyfitz.com/screencast/html-training/css/positioning/

