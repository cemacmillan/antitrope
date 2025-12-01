# Sigil Text Path Orientation Specification

## Overview
This document describes the formula used to correctly orient text along circular paths in SVG sigil designs, specifically for the masthead designs where text must be properly centered and visible without truncation.

## Key Principle: Element Rendering Order
In SVG, elements are rendered in document order—later elements appear on top of earlier elements. To ensure the outer ring appears above the inner ring, the outer circle element must be defined **after** the inner circle element in the SVG markup.

## Outer Ring Text Path Formula

### Path Definition
The outer text path uses a **semicircular arc** that spans from west to east along the top half of the circle:

```svg
<path id="outerCircle"
      d="M 30,240
         A 210,210 0 0,1 450,240"
      fill="none" />
```

### Path Breakdown
- **Start Point**: `M 30,240` - West side (left) of the circle at horizontal center
- **Arc Parameters**: `A 210,210 0 0,1 450,240`
  - `210,210` - Radius (x-radius, y-radius) matching the outer circle radius
  - `0` - X-axis rotation (no rotation)
  - `0` - Large-arc-flag (0 = small arc, i.e., semicircle)
  - `1` - Sweep-flag (1 = clockwise direction)
  - `450,240` - End point: East side (right) of the circle at horizontal center

### Text Positioning
```svg
<textPath href="#outerCircle" startOffset="50%" text-anchor="middle">
```

- **startOffset="50%"**: Positions the text starting point at the midpoint of the path (top center)
- **text-anchor="middle"**: Centers the text horizontally at the startOffset point

### Result
This combination ensures:
- The text spans from west (left) to east (right) along the top arc
- "ΥΠΕΡ" appears on the west side
- "ÉDITIONS" appears on the east side
- The text is centered at the top of the circle
- No truncation occurs at the edges

## Inner Ring Text Path Formula

### Path Definition
The inner text path uses a **full circular arc**:

```svg
<path id="innerCircle"
      d="M 240,90
         A 150,150 0 1,1 239.999,90"
      fill="none" />
```

### Path Breakdown
- **Start Point**: `M 240,90` - Top center of the circle
- **Arc Parameters**: `A 150,150 0 1,1 239.999,90`
  - `150,150` - Radius matching the inner circle radius
  - `0` - X-axis rotation
  - `1` - Large-arc-flag (1 = large arc, i.e., full circle)
  - `1` - Sweep-flag (1 = clockwise)
  - `239.999,90` - End point: Nearly identical to start (full circle)

### Text Positioning
```svg
<textPath href="#innerCircle" startOffset="50%" text-anchor="middle">
```

- **startOffset="50%"**: With a full circle, this positions text at the bottom
- **text-anchor="middle"**: Centers the text at that point

## Font Sizing Considerations

### Outer Text
- **Font Size**: 26px (reduced from 30px to prevent truncation)
- **Letter Spacing**: 3px
- The reduced font size ensures the text fits within the semicircular path without losing characters at the edges

### Inner Text
- **Font Size**: 26px
- **Letter Spacing**: 2.5px

## Visual Hierarchy (Rendering Order)

1. Background rectangle
2. Inner circle (rendered first, appears below)
3. Outer circle (rendered second, appears on top)
4. Text paths and text elements
5. Central tri-radial device

This order ensures the outer ring visually appears above the inner ring, creating proper depth perception.

## Summary

The critical formula for correct outer ring orientation:
1. **Semicircular path** from west (30,240) to east (450,240)
2. **startOffset="50%"** to center at top
3. **text-anchor="middle"** to center the text string
4. **Reduced font size** (26px) to prevent truncation
5. **Outer circle element after inner circle** in markup for proper layering

