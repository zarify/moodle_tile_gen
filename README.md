# Moodle Tile Image Creator

A simple web-based tool designed to generate consistent, standardized tile images for Moodle courses.

## Overview

This tool allows users to create 300x200 pixel tile images that align with Moodle's visual style. It provides two main modes of operation:
1.  **Background Image Mode:** Upload a photo to be used as the tile background.
2.  **Icon Mode:** Upload an icon to be placed on a standard background.

## Features

*   **Standardized Output:** Produces 300x200 PNG images ready for Moodle.
*   **colour Branding:** Apply consistent colour strips from a predefined palette (Moodle Blue, Teal, Purple, etc.).
*   **Image Editing:** Zoom and pan controls to position background images that not the same as the output size.
*   *   **Previews:** Shows a preview of the coloured strip while positioning backgrounds.
*   **Automatic Tinting:**
    *   **Icons:** Automatically tints icons to match the selected strip colour.

## Usage

1.  **Select Mode:** Choose between adding a "Background Image" or an "Icon".
2.  **Upload:**
    *   **Backgrounds:** Supports PNG and JPEG.
    *   **Icons:** Supports SVG and PNG.
3.  **Adjust:** If using a background image, use the built-in editor to zoom and position the image within the frame.
4.  **colour:** Select a strip colour from the palette.
5.  **Download:** Click "Download Tile Image" to save your `moodle-tile.png`.

## Important: Icon Requirements

For the **Icon Mode**, the quality of the result depends heavily on the input file.

*   **Format:** SVG or PNG with transparency.
*   **colour:** **White icons are required.**
*   **Why?** The tool tints the icon to match the selected strip colour. It does this by using the brightness of the original pixels. If you upload a coloured icon, the resulting tint will be muddy. A white icon with a transparent background ensures the colour is applied accurately.
