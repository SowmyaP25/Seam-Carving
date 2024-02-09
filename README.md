# Seam Carving
Seam Carving for Content Aware Image Resizing and Object Removal

## Introduction
The goal of this project is to perform content-aware image resizing for both reduction and expansion and image object removal with seam carving operator. This allows image to be resized without losing or distorting meaningful content from scaling. The code and technique described below is an implementation of the algorithm presented in [Avidan and Shamir](http://graphics.cs.cmu.edu/courses/15-463/2007_fall/hw/proj2/imret.pdf) 

## Algorithm Overview
### Seam Removal
1. Calculate energy map: 
>Energy is calculated by sum the absolute value of the gradient in both x direction and y direction for all three channel (B, G, R). Energy map is a 2D image with the same dimension as input image.

2. Build accumulated cost matrix using forward energy: 
>This step is implemented with dynamic programming. The value of each pixel is equal to its corresponding value in the energy map added to the minimum new neighbor energy introduced by removing one of its three top neighbors (top-left, top-center, and top-right)

3. Find and remove minimum seam from top to bottom edge: 
>Backtracking from the bottom to the top edge of the accumulated cost matrix to find the minimum seam. All the pixels in each row after the pixel to be removed are shifted over one column to the left if it has index greater than the minimum seam.

4. Repeat step 1 - 3 until achieving targeting width 

5. Rotate image and repeat step 1 - 4 for vertical resizing: 
>Rotating image 90 degree counter-clockwise and repeat the same steps to remove rows.

### Seam Insertion
Seam insertion can be thought of as inversion of seam removal and insert new artificial pixels/seams into the image. We first perform seam removal for n seams on a duplicated input image and record all the coordinates in the same order when removing. Then, we insert new seams to original input image in the same order at the recorded coordinates location. The inserted argificial pixel values are derived from an average of left and right neighbors.

### Seam Removing and Insertion with Mask
When generating energy map, the region protected by mask are weighted with a very high positive value. This guarantees that the minimum seam will NOT be routed through the masked region so that we can provent pixels at this region from removing or distorting because of seam insertion.

2. Seam insertion
>Insering seams to return the image back to it's original dimensions. 

## Result 1
### Original image
![original image](https://github.com/vivianhylee/seam-carving/raw/master/example/image6.jpg)

### Scaling up 
![image size expansion](https://github.com/vivianhylee/seam-carving/raw/master/example/image17_result.png)

## Result 2
### Scaling down
![animation image size reduction](https://github.com/vivianhylee/seam-carving/raw/master/example/image05_video.gif)

### Scaling up 
![animation image size expansion](https://github.com/vivianhylee/seam-carving/raw/master/example/image7_video.gif)

##################################################

# Common using OpenCV

### 1. Key Modules:

- **homotrans**: Provides functions for homography transformation.
- **lookat**: Computes the rotation matrix and translation vector for a given camera position and target.
- **rect2rect_mtx**: Computes the transformation matrix from one rectangle to another.
- **make_cmap**: Creates a colormap for visualization.

### 2. Classes:

- **Bunch**: A simple class for creating objects with arbitrary attributes.
- **Sketcher**: A class for sketching lines on images using OpenCV.

### 3. Functions:

- **splitfn**: Splits a filename into path, name, and extension.
- **anorm**: Computes the Euclidean norm of a vector.
- **clock**: Returns the current time in milliseconds.
- **draw_keypoints**: Draws keypoints on an image.

#######################################################

# Image Masking using OpenCV

This project demonstrates how to mask an object within an image using OpenCV. Masking involves creating a binary image that separates the object of interest from the background, allowing for selective processing or manipulation.

## Overview

The project performs the following steps:
1. Loads an input image.
2. Allows the user to sketch a mask to identify the region of interest.
3. Creates a binary mask based on the sketch.
4. Applies the mask to the original image to extract the colored portion of the object.
5. Combines the colored portion with the grayscale portion of the image outside the mask.
6. Saves the masked output image and displays it.

## Usage

### Dependencies

- OpenCV
- NumPy
- Matplotlib (for visualization)

### Running the Program

1. Ensure you have Python installed along with the required dependencies.
2. Run the program with the command:

```bash
python image_masking.py [input_image]


