# Moodle Tile Generator - Modifications Specification

Date: November 13, 2025

Version: 1.1 (Modified Feature Set)

Purpose: Detailed implementation plan for modifications to the original tile generator specification.

---

## 1. Remove Top Text Functionality

### 1.1 Scope
- **Remove:** Top text input field (F.2.1 from original spec)
- **Remove:** All UI controls and rendering logic associated with top text
- **Keep:** Bottom text input and all bottom text functionality

### 1.2 Implementation Notes
- Clean up any references to "top text" in code comments and variable names
- Update positioning logic to focus solely on bottom text at Y_bottom ≈ 90%

---

## 2. Color Strip Behind Bottom Text

### 2.1 Visual Design
- **Positioning:** Behind the bottom text, in front of the background image
- **Width:** Full canvas width (400px)
- **Opacity:** Fully opaque (100%)
- **Height:** Dynamic, based on actual text height to accommodate content
  - Calculate based on rendered text metrics
  - Provide padding above and below text for visual breathing room

### 2.2 Rendering Order
1. Background image (scaled/cropped to fill canvas)
2. Color strip (opaque rectangle)
3. Bottom text (white, without black outline)

### 2.3 Technical Requirements
- Strip must be rendered before text to act as background
- Strip height should be calculated dynamically based on text metrics
- Strip should respond to changes in font size and text content

---

## 3. Configurable Color Palette

### 3.1 Color Palette Definition
Define a palette based on typical Moodle brand colors:

```javascript
// COLOR_PALETTE - Easily modifiable color definitions
const COLOR_PALETTE = [
    { name: 'Moodle Orange', value: '#F98012' },
    { name: 'Dark Blue', value: '#1F3A68' },
    { name: 'Light Blue', value: '#5A9FD4' },
    { name: 'Green', value: '#7FA04F' },
    { name: 'Purple', value: '#8E44AD' },
    { name: 'Red', value: '#C0392B' },
    { name: 'Dark Grey', value: '#333333' },
    { name: 'Black', value: '#000000' }
];
```

**Note:** This palette is defined in an easily accessible location for future modification.

### 3.2 UI Controls
- Provide a color picker/selector UI element (e.g., radio buttons, color swatches, or dropdown)
- Selected color applies to the strip background
- Default selection: Moodle Orange (#F98012)

---

## 4. Icon Overlay Feature

### 4.1 Icon Specifications
- **Size:** 64x64 pixels (default, user-adjustable)
- **Position:** Top-right corner with 15px margin from both top and right edges
- **Format:** PNG files (with transparency support). SVG would be ideal but PNG provides better availability.
- **Upload:** User can upload their own icon file

### 4.2 Icon Scaling
- Uploaded icons automatically scale to fit within the 64x64px default size
- Maintain aspect ratio during scaling
- Provide a size slider control (range: 32px - 128px suggested)
  - **Implementation note:** Slider should be easily disabled or removed in future iterations
  - Use a configuration constant or feature flag to control slider visibility

### 4.3 Technical Implementation
```javascript
// ICON_CONFIG - Easily modifiable icon settings
const ICON_CONFIG = {
    defaultSize: 64,
    minSize: 32,
    maxSize: 128,
    margin: 15,
    enableSizeSlider: true  // Set to false to disable slider
};
```

---

## 5. Icon Colorization

### 5.1 Colorization Method
Use **Canvas Composite Operations** method for PNG colorization:

**Recommended Approach:**
1. Draw the icon to a temporary canvas
2. Use `globalCompositeOperation = 'source-in'` to apply color
3. Draw the colored result back to the main canvas

**Algorithm:**
```javascript
// Pseudo-code for icon colorization
function colorizeIcon(iconImage, color) {
    // Create temporary canvas
    tempCanvas = createCanvas(iconSize, iconSize);
    tempCtx = tempCanvas.getContext('2d');
    
    // Draw original icon
    tempCtx.drawImage(iconImage, 0, 0, iconSize, iconSize);
    
    // Apply color using composite operation
    tempCtx.globalCompositeOperation = 'source-in';
    tempCtx.fillStyle = color;
    tempCtx.fillRect(0, 0, iconSize, iconSize);
    
    // Return colorized canvas
    return tempCanvas;
}
```

### 5.2 Color Palette Integration
- Icon colorization uses the same COLOR_PALETTE as the color strip
- Provide separate UI control for icon color selection
- Default: Match the strip color (or allow independent selection)

### 5.3 Transparency Handling
- Preserve alpha channel from original PNG
- Only recolor non-transparent pixels
- Transparent areas remain transparent after colorization

---

## 6. Text Styling Changes

### 6.1 Remove Black Outline
- **Remove:** The 3-5px black stroke/outline specified in original 3.1
- **Rationale:** Color strip provides sufficient contrast; outline is redundant

### 6.2 Text Color
- **Keep:** Solid white (#FFFFFF) text color
- Text will render on top of the opaque color strip, ensuring readability

### 6.3 No Dynamic Font Scaling
- **Remove:** Automatic font size reduction logic (P.3.2.3 from original spec)
- **Behavior:** Users are responsible for choosing appropriate text length and font size
- If text overflows, it will clip or overflow naturally (no automatic resizing)
- Consider adding a visual warning if text exceeds strip boundaries (optional enhancement)

---

## 7. Updated Rendering Pipeline

### 7.1 Render Order
1. Clear canvas
2. Draw background image (scaled to cover 400x400px)
3. Calculate text metrics and strip height
4. Draw color strip (full width, dynamic height, at Y_bottom ≈ 90%)
5. Draw bottom text (white, centered, no outline)
6. If icon is uploaded:
   - Colorize icon using selected color
   - Draw icon at top-right (15px margins)

### 7.2 Real-Time Updates
All changes (text, colors, icon, icon size) should update the canvas in real-time.

---

## 8. Configuration Summary

All easily modifiable constants should be grouped at the top of the code:

```javascript
// CONFIGURATION SECTION - Modify these values as needed

const COLOR_PALETTE = [ /* defined in section 3.1 */ ];

const ICON_CONFIG = {
    defaultSize: 64,
    minSize: 32,
    maxSize: 128,
    margin: 15,
    enableSizeSlider: true  // Feature flag
};

const TEXT_CONFIG = {
    color: '#FFFFFF',
    enableOutline: false,  // Set to true to re-enable black outline
    enableDynamicScaling: false  // Set to true to re-enable auto-scaling
};

const CANVAS_CONFIG = {
    width: 400,
    height: 400,
    stripPosition: 0.90  // 90% from top
};
```

---

## 9. Implementation Checklist

- [ ] Remove top text input UI and logic
- [ ] Implement opaque color strip rendering with dynamic height
- [ ] Add COLOR_PALETTE constant and color picker UI
- [ ] Remove black text outline from rendering
- [ ] Disable dynamic font scaling behavior
- [ ] Add icon upload functionality
- [ ] Implement icon scaling to 64x64px default
- [ ] Add icon size slider (with easy disable mechanism)
- [ ] Implement PNG colorization using canvas composite operations
- [ ] Add icon color picker (using same COLOR_PALETTE)
- [ ] Update render pipeline to correct z-order
- [ ] Test with various icon sizes and colors
- [ ] Test with varying text lengths
- [ ] Document all configuration constants

---

## 10. Future Considerations

- May lock icon size to fixed 64x64px (remove slider)
- May lock font size to fixed value (making strip height fixed)
- Consider SVG icon support in future iterations for better colorization
- Consider adding visual warnings for text overflow