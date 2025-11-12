# Moodle Tile Image Creator Specification (Original Concept)

Feature: Dynamic Image and Text Overlay Generator

Date: November 10, 2025

Version: 1.0 (Initial Core Feature Set)

Purpose: To enable educators to quickly create visually engaging, high-contrast course tile images using a standard "meme generator" style for immediate recognition and impact within a learning management system (LMS).

## 1. Functional Requirements

The application must provide the following core features:

### 1.1 Image Input

- **F.1.1 Image Upload:** Users must be able to upload a local image file (PNG, JPG) which will be loaded onto the canvas.
    
- **F.1.2 Image Scaling:** The uploaded image must automatically scale and crop to fill the required output canvas dimensions while maintaining its aspect ratio (covering the area).
    

### 1.2 Text Overlay Management

- **F.2.1 Top Text Input:** A text area must be available for users to input the primary (top) line of text.
    
- **F.2.2 Bottom Text Input:** A separate text area must be available for the secondary (bottom) line of text.
    
- **F.2.3 Text Customization:** Users must have controls for:
    
    - Font Size (e.g., slider control from 24pt to 72pt).
        
    - Font Alignment (Center, Left, Right).
        

### 1.3 Canvas & Rendering

- **F.3.1 Real-Time Preview:** The canvas must update instantly (or near-instantly) whenever the image source or text input changes.
    
- **F.3.2 Export:** A dedicated button must allow the user to download the final composite image.
    

## 2. Technical Requirements

The tool will be built as a single, client-side HTML5 application.

|   |   |   |
|---|---|---|
|**Requirement ID**|**Description**|**Details**|
|**T.2.1 Core Technology**|The application must use the HTML5 Canvas API for image manipulation and rendering.|This ensures client-side performance and eliminates server load.|
|**T.2.2 Output Dimensions**|The final exported image must conform to standard LMS tile sizes.|**Fixed Resolution:** 400px x 400px (Square).|
|**T.2.3 Image Handling**|The `getImageData()` method must be accessible for future pixel-level analysis.|Cross-Origin restrictions must be considered if image URLs are used (use only local files or proxy).|
|**T.2.4 Export Format**|The final image must be downloadable.|**Default Format:** PNG (for better text clarity). JPEG option is desirable.|

## 3. User Interface and Design (The "Meme" Aesthetic)

The design prioritizes high contrast and clear readability over potentially busy or detailed background images.

### 3.1 Text Design Principles

|   |   |   |
|---|---|---|
|**Element**|**Specification**|**Purpose**|
|**Font Family**|A bold, sans-serif font (e.g., Impact, or a suitable web-safe alternative like Arial Black).|Emphasizes impact and readability.|
|**Text Color**|Solid White (`#FFFFFF`).|Standard meme aesthetic; maximizes foreground brightness.|
|**Contrast Layer**|A mandatory, thick black outline/stroke (e.g., 3-5px width) around all text characters.|Ensures text remains legible regardless of the background image's color or complexity (high contrast guaranteed).|

### 3.2 Layout and Positioning

- **P.3.2.1 Top Text:** Must be vertically positioned at $Y_{top} \approx 10\%$ of the canvas height.
    
- **P.3.2.2 Bottom Text:** Must be vertically positioned at $Y_{bottom} \approx 90\%$ of the canvas height.
    
- **P.3.2.3 Text Wrapping:** Text should automatically wrap to fit the canvas width. If the text exceeds the canvas height, the font size should be dynamically reduced until it fits (or a warning should be displayed).
    

## 4. Future Enhancements

These items are out of scope for the initial V1.0 but represent the next iteration of the tool.

|   |   |   |
|---|---|---|
|**Enhancement ID**|**Description**|**Specification Reference**|
|**E.4.1 Accessibility Metrics**|Integrate analysis of image complexity and color saturation into the feedback loop.|Refer to **Image Accessibility Analysis Specification** (Metric 1 and Metric 2) for details on $S_{avg}$ and $B$ scores.|
|**E.4.2 Image Filters**|Add simple, non-distracting image filters (e.g., grayscale, subtle blur) to make text pop more effectively.|Users can apply a subtle blur to complex backgrounds.|
|**E.4.3 Template Library**|Provide a pre-selected library of simple, high-contrast background images/icons suitable for tile use.|Reduces reliance on user-uploaded images that might fail a complexity check.|