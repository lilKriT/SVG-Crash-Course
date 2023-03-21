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
