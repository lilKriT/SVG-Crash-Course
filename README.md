# SVG Crash Course

SVG stands for Scalable Vector Graphics. Since HTML5 it can be used inside HTML markup.
It's very useful for creating simple (and complex) graphics.

# Basics

## Viewport

Every svg file starts like:

```
<svg width="100" height="100" viewBox="-100 -100 200 200">

</svg>
```

Width and height are the HTML dimensions (how big the image will be in your browser).
Viewbox is the "inside" dimensions, the viewport.

## Example shapes

```
<circle cx="0" cy="20" r="70" fill="#D1495B" />
<rect x="-17.5" y="-65" width="35" height="20" fill="#F79257" />
<polygon points="0,0 80,120 -80,120" fill="#234236" />
<text id="sometext" x="0" y="0">Write here</text>
```

## Common properties

- fill
- stroke
- stroke-width
- stroke-linecap
- rx - same as border radius
- cx, cy - circle position
- r - circle radius
- x, y, width, height - rectangle dimensions
- points - points in a polygon

## Groups

```
<g></g>
```

You can put elements in a group to transform them together.

## Transforms

```
transform="translate (0 5)"
transform="rotate (72)"
```

## Reusing code

```
<defs>
    <path id="reuseThis" d="..." stroke="..." />
</defs>

// Later
<use href="#reuseThis" transform="rotate(60)">
<use href="#reuseThis" transform="rotate(120)">
<use href="#reuseThis" transform="rotate(180)">
```

## Paths

```
<path
      id="branch"
      d="
        M 0 0 L 0 -90
        M 0 -20 L 20 -34
        M 0 -20 L -20 -34
        M 0 -40 L 20 -54
        M 0 -40 L -20 -54
        M 0 -60 L 20 -74
        M 0 -60 L -20 -74"
      stroke="#E5C39C"
      stroke-width="5"
    />
```

d - stands for draw
each command starts with a letter that defines command type, and then coordinates.
M - move to
L - line to

They can use Bezier curves too.

```
<path
    id="squiggly"
    d="
    M 0 0
    Q 5 -75 0 -70
    Q -10 -65 0 -60
    ...
    "
    stroke="black"
    stroke-width="2"
/>
```

Command - control coords - end coords
Q - quadratic bezier
C - cubic bezier (same but has 2 control points instead of 1)
`C 40 -60 -40 -60 -40 10`

## Strokes

- stroke-dasharray (if you set one number, it will make a dashed stroke of equal lenghts. Two numbers - dash and gap length. Or you can even use more numbers, then they will alternate)
- stroke-dashoffset - offsets where the drawing of first element starts.
- pathLength - it creates a virtual circumference of the path (like viewport for dimensions)

## CSS attributes

In general if something defines the style - you can use CSS. If it defines shape - you can't use CSS.

# Using JS with SVG

Assign an ID to element.
Then just use JS:

```js
hoursElement.setAttribute("transform", `rotate(${(360 / 12) * hour})`);
```

And then animate it:

```js
function animate() {
  const date = new Date();

  const hour = date.getHours() % 12;
  const minute = date.getMinutes();
  const second = date.getSeconds();

  hoursElement.setAttribute("transform", `rotate(${(360 / 12) * hour})`);
  minutesElement.setAttribute("transform", `rotate(${(360 / 60) * minute})`);
  secondsElement.setAttribute("transform", `rotate(${(360 / 60) * second})`);

  requestAnimationFrame(animate);
}

requestAnimationFrame(animate);
```

You can add event handlers too.

# Animation

Very simple example:

```
<svg viewBox="0 0 10 10" xmlns="http://www.w3.org/2000/svg">
 <rect width="10" height="10">
   <animate
     attributeName="rx"
     values="0;5;0"
     dur="10s"
     repeatCount="indefinite" />
 </rect>
</svg>
```

Some attributes:
Animation timing:

- begin
- dur
- end
- min
- max
- restart
- repeatCount
- repeatDur
- fill

Value:

- calcMode
- values
- keyTimes
- keySplines
- from
- to
- by

Other:

- attributeName
- additive
- accumulate

Events:

- onbegin
- onend
- onrepeat

# Gradients

## Linear

Put them in <defs>

```
<linearGradient id="Gradient1">
    <stop class="stop1" offset="0%" />
    <stop class="stop2" offset="50%" />
    <stop class="stop3" offset="100%" />
</linearGradient>
```

Then style:

```
<style>
    <![CDATA[
            #rect1 { fill: url(#Gradient1); }
            .stop1 { stop-color: red; }
            .stop2 { stop-color: black; stop-opacity: 0; }
            .stop3 { stop-color: blue; }
        ]]>
</style>
```

And insert as fill:
`fill="url(#Gradient2)"`

## Radial

```
<radialGradient id="RadialGradient1">
    <stop offset="0%" stop-color="red" />
    <stop offset="100%" stop-color="blue" />
</radialGradient>
```

They can have separate center and focal point, spread etc...

### Example of advanced gradients:

```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1920 1080">
   <defs>
      <linearGradient id='a' gradientUnits='objectBoundingBox' x1='0' y1='0' x2='1' y2='1'>
         <stop offset='0' stop-color='red'>
            <animate attributeName="stop-color"
               values="red;purple;blue;green;yellow;orange;red;" dur="20s" repeatCount="indefinite">
            </animate>
         </stop>
         <stop offset='.5' stop-color='purple'>
            <animate attributeName="stop-color"
               values="purple;blue;green;yellow;orange;red;purple;" dur="20s" repeatCount="indefinite">
            </animate>
         </stop>
         <stop offset='1' stop-color='blue'>
            <animate attributeName="stop-color"
               values="blue;green;yellow;orange;red;purple;blue;" dur="20s" repeatCount="indefinite">
            </animate>
         </stop>
         <animateTransform attributeName="gradientTransform" type="rotate" from="0 .5 .5" to="360 .5 .5"
            dur="20s" repeatCount="indefinite" />
      </linearGradient>
      <linearGradient id='b' gradientUnits='objectBoundingBox' x1='0' y1='1' x2='1' y2='1'>
         <stop offset='0' stop-color='red'>
            <animate attributeName="stop-color"
               values="red;purple;blue;green;yellow;orange;red;" dur="20s" repeatCount="indefinite">
            </animate>
         </stop>
         <stop offset='1' stop-color='purple' stop-opacity="0">
            <animate attributeName="stop-color"
               values="purple;blue;green;yellow;orange;red;purple;" dur="20s" repeatCount="indefinite">
            </animate>
         </stop>
         <animateTransform attributeName="gradientTransform" type="rotate" values="360 .5 .5;0 .5 .5" class="ignore"
            dur="10s" repeatCount="indefinite" />
      </linearGradient>
   </defs>
   <rect fill='url(#a)' width='100%' height='100%' />
   <rect fill='url(#b)' width='100%' height='100%' />
</svg>
```
