# Image Accessibility Analysis Specification

Feature: Client-Side Image Accessibility (A11y) Feedback

Date: November 10, 2025

Audience: Educators/Content Creators & Developers

Goal: To provide immediate, actionable feedback to users regarding the visual complexity and color saturation of uploaded images (Moodle tiles, course assets, etc.) to ensure content is accessible and non-distracting for students with cognitive, sensory, or visual needs.

## 1. Scope & Technical Approach

The analysis must run entirely client-side, leveraging the HTML5 Canvas API to access raw pixel data. No server-side processing or external libraries will be required for the core analysis.

### 1.1 Implementation Flow

1. **Image Upload/Canvas Draw:** The user uploads an image, which is drawn onto a hidden `<canvas>` element.
    
2. **Data Extraction:** The JavaScript function `context.getImageData()` is used to retrieve the raw pixel data array.
    
3. **Pixel Iteration & Calculation:** The system iterates through the pixel data (Red, Green, Blue, Alpha) to compute the required metrics: Average Saturation and Busyness Score.
    
4. **Feedback Generation:** Based on predefined threshold ranges, the system generates a human-readable feedback message displayed in the UI.
    

## 2. Accessibility Metrics

Two primary metrics will be calculated to determine the visual distraction potential of an image: **Average Saturation** (for color distraction) and a **Busyness Score** (for visual complexity).

### 2.1 Metric 1: Average Saturation (Color Threshold)

This metric assesses the overall intensity of color in the image. Highly saturated images can be overwhelming for some learners.

**Calculation Steps:**

1. **Convert RGB to HSL:** For every pixel $(R, G, B)$ in the image, convert the RGB values (normalized to $0$ to $1$) into the HSL (Hue, Saturation, Luminosity) color model.
    
2. **Extract Saturation (**$S$**):** Isolate the Saturation component $S \in [0, 1]$.
    
3. **Calculate Average Saturation (**$S_{avg}$**):** Compute the mean of all saturation values.
    

$$S_{avg} = \frac{1}{N} \sum_{i=1}^{N} S_i $$*where $N$ is the total number of pixels.* ### 2.2 Metric 2: Busyness Score (Visual Complexity) This score approximates the image's complexity by counting high-contrast transitions or "edges" within the image. A high number of edges indicates a busy, visually demanding image. **Calculation Steps (Edge Detection via Luminosity Change):** 1. **Calculate Luminosity ($L$):** For every pixel $(R, G, B)$, calculate its perceived luminosity using the standard formula:$$