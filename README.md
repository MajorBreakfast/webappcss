WebAppCSS
=========

WebAppCSS is a micro SCSS framework for web apps that adjust intelligently to the available screen real esate. It is compatible with IE10, Chrome, Firefox, the Android Browser, iOS/Mac Safari and others.

It's not meant to be used exclusively but rather in conjunction with a more ambitous framework (like [Foundation 4][http://foundation.zurb.com/]).

It trys to solve the layout problems that occur when building a web app that runs on mobile devices where screen real estate is very precious. It provides building blocks and estabilshes conventions for your own designs.

## How to use it
``` SCSS
@import "../webappcss/webappcss";
```

## Shorthand notation used in this documentation
```
.a.b[.c]
  div
    {{placeholder}}
  .d margin:10px;
```
means
```html
<div class="a b c">
  <div>{{placeholder}}</div>
  <div class="d" style="margin:10px;"></div>
</div>
```
The square brackets signify that class "c" is optional. Optional classes are used for specifying slight changes in appearance or behaviour.

## Screen size
A screen qualifies as **small** if it's either less than 600px wide or less than 500px tall (or both).

## Design elements
### Overview
WebAppCSS understands these UI element types:

* **Pane**: Parent width & height
* **Block**: Parent width, as high as content requires

### Panes
Panes try to fill their parent element completely. This makes them ideal for building flexible layouts which adjust to the browser window's size.

#### .pane
```
.pane[.clip|.scrollable-x|.scrollable-y|.scrollable-xy][.debug]
  {{content: panes, blocks, inline, inline-blocks, ...}}
```

A simple pane.

It's css implemenation is also very simple:

```css
display: block;
position: absolute;
top: 0; right: 0; bottom: 0; left: 0;
```

`scrollable-...` classes are self-explanatory.
`debug` class draws a white outline.

If you want to add some padding to the pane, you can do so by specifying a margin.
```
.pane margin:10px;
```

You can also limit the elements width or height:
```
.pane width:200px;max-width:100%;height:200px;max-height:100%;
```
The element simply stays centered in it's space, because the margin is set to "auto" by default.
Note: - This centers the pane and not its contents. If you need that take a look at the *vertically-centered-blocks-pane*.
- Just specifying "max-width:200px;max-height:200px;" will yield the wrong result in IE.

#### .header-content-pane
```
.header-content-pane[.responsive][.debug]
  div
    {{header: panes, blocks, inline, inline-blocks, ...}}
  div
    {{content: panes, blocks, inline, inline-blocks, ...}}
```

Pane with 48px header and flexible height content pane underneath with a 10px gap inbetween.
If `responsive` class is set, then the header's height is 48px on small screens and 80px on bigger screens.s
`debug` class draws a white outline.

#### .vertically-centered-blocks-pane
```
.vertically-centered-blocks-pane
  div
    {{blocks, inline, inline-blocks, ...}}
```

#### .stack-pane
```
.stack-pane[.clip|.scrollable-y]
  div
    ...
    .item
      {{blocks, inline, inline-blocks, ...}}
    ...
    .gap
    ...
```

A stack pane is great for creating things like main menus. It arranges its items vertically.

Excess vertical space gets distributed among the gaps.

The height of an item is determinded by its content's height. There are no greedy items.

You can assign a fixed height to gaps like this:
.stack-pane
  div
    ...
    .gap height:20px;
    ...
```
For a minimal height constraint just combine two gaps. (min-height css property doesn't work)

Side note: Internally the stack pane uses a table. Tables have been around for a long time and they have great browser support. Support for greedy items isn't possible though, because IE doesn't understand `position:relative` on table rows/cells which makes it impossible to nest a pane, because of its positioning. (absolute and top,right,bottom,left = 0) To make greedy items useful absolute positioning of its child items is necessary. Otherwise the whole table just grows in height and gets cut off.

#### .rack-block
```
.rack-block
  div
    ...
    .greedy-item
      {{blocks, inline, inline-blocks, ...}}
    ...
    .lazy-item
      {{blocks, inline, inline-blocks, ...}}
    ...
    .gap
    ...
```

A rack block is perfectly suited for creating things like navigation bars and much much more. It arragnes it's items horizontally.

Like all blocks, its width matches the available horizontal space in its parent element. Excess horizontal space gets distributed among the greedy items and the gaps. This is why you should place at least one of either.

The rack block's height depends on the tallest child item.

By default the content of an item is vertically centered. Use `vertical-align` to specify otherwise. Here is how you stick it to the top or the bottom:
```
.rack-block
  div
    .lazy-item vertical-align:top;
      This text sticks to the top
    .greedy-item vertical-align:bottom;
      This text sticks to the bottom
```

You can assign a fixed or minimal width to gaps like this:
```
.rack-block
  div
    ...
    .gap width:20px;
    ...
```
For a minimal height constraint just combine two gaps.

Side note: Internally the rack block uses tables. Tables have been around for a long time and they have great browser support.