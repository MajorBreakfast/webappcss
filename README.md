WebAppCSS
=========

Note: I'm activly using this framework in a project of mine and I develop it as I go along.

WebAppCSS is a micro SCSS framework for web apps that adjust intelligently to the available screen real esate. It is compatible with IE10, Chrome, Firefox, the Android Browser, iOS/Mac Safari and others.

You should use it in conjunction with a more ambitious framework (like [Foundation 4](http://foundation.zurb.com/)) or do it all yourself optionally using a mixin library (like [Scut](http://davidtheclark.github.io/scut/)).

It trys to solve the layout problems that occur when building a web app that runs on mobile devices where screen real estate is very precious. It provides building blocks and estabilshes conventions for your own designs.

It solves these layout problems using table styles. Soon flexbox will make hacks like these unnecessary.

## How to use it
``` SCSS
@import "webappcss";
```

## Design elements
### Overview
WebAppCSS understands these UI element types:

* **Panes**: Parent's width & height
  * .pane
  * .stack-pane
* **Blocks**: Parent's width, as high as content requires
  * .rack-block

### Panes
Panes try to fill their parent element completely. This makes them ideal for building flexible layouts which adjust to the browser window's size.

#### .pane
``` html
<div class="pane [debug]">
  {{content: panes, blocks, inline, inline-blocks, ...}}
</div>
```

A simple pane.

It's css implemenation is also very simple:

```css
display: block;
position: absolute;
top: 0; right: 0; bottom: 0; left: 0;
```

- `debug` class draws a white outline.

If you want to add some padding to the pane, you can do so by specifying a margin:
``` html
<div class="pane" style="margin:20px;">
</div>
```

You can also limit the elements width or height:
``` html
<div class="pane" style="width:200px;max-width:100%;height:200px;max-height:100%;">
</div>
```
The element simply stays centered in it's space, because the margin is set to "auto" by default.

Note:
- This centers the pane and not its contents. If you need that take a look at the mixins.
- Just specifying "max-width:200px;max-height:200px;" will yield the wrong result in IE.

#### .stack-pane
``` html
<div class="stack-pane">
  <div>
    ...
    <div class="item">
      {{blocks, inline, inline-blocks, ...}}
    </div>
    ...
    <div class="gap"></div>
    ...
  </div>
</div>
```

A stack pane is great for creating things like main menus. It arranges its items vertically.

Gaps have a flexible height. Excess vertical space gets distributed among the gaps.

The height of an item is determinded by its content's height.

You can assign a fixed height to gaps like this:
``` html
<div class="stack-pane">
  <div>
    <div class="gap" style="height: 20px;"></div>
  </div>
</div>
```

For a minimal height constraint just combine two gaps: One with a fixed height, the other one flexible (min-height css property doesn't work):
``` html
<div class="stack-pane">
  <div>
    <div class="gap"></div>
    <div class="gap" style="height: 20px;"></div>
  </div>
</div>
```

Side note: Internally the stack pane uses a table. Tables have been around for a long time and they have great browser support. Support for greedy items isn't possible though, because IE doesn't understand `position:relative` on table rows/cells which makes it impossible to nest a pane, because of its positioning. (absolute and top,right,bottom,left = 0) To make greedy items useful absolute positioning of its child items is necessary. Otherwise the whole table just grows in height and gets cut off.

#### .rack-block
``` html
<div class="rack-block">
  <div>
    ...
    <div class="greedy-item">
      {{blocks, inline, inline-blocks, ...}}
    </div>
    ...
    <div class="lazy-item">
      {{blocks, inline, inline-blocks, ...}}
    </div>
    ...
    <div class="gap"></div>
    ...
  </div>
</div>
```

A rack block is perfectly suited for creating things like navigation bars. It arranges its items horizontally.

Like all blocks, its width matches the available horizontal space in its parent element. Excess horizontal space gets distributed among the greedy items and the gaps. This is why you should place at least one of either.

The rack block's height depends on the tallest child item.

By default the content of an item is vertically centered. Use `vertical-align` to specify otherwise. Here is how you stick it to the top or the bottom:
``` html
<div class="rack-block">
  <div>
    <div class="greedy-item" style="vertical-align:top;">
      This text sticks to the top
    </div>
    <div class="lazy-item" style="vertical-align:bottom;">
      This text sticks to the bottom
    </div>
  </div>
</div>
```

You can assign a fixed or minimal width to gaps like this:
``` html
<div class="rack-block">
  <div>
    <div class="gap" style="width:20px;"></div>
  </div>
</div>
```

For a minimal height constraint just combine two gaps:
``` html
<div class="rack-block">
  <div>
    <div class="gap"></div>
    <div class="gap" style="width:20px;"></div>
  </div>
</div>
```

Side note: Internally the rack block uses tables. Tables have been around for a long time and they have great browser support.

#### Mixins
- `@mixin scrollable-y` Smooth scrolling on iOS
