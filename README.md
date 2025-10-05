#  Panorama Stitching

This is my submission for the Assignment - 2 : Panorama Stitching, for the course - Digital Image Processing. I have implemented automated image stitching using both custom and an opencv implementation.

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Methodology](#methodology)
- [Complexity and Optimization](#complexity-and-optimization)
- [Results](#results)
- [Future Enhancements](#future-enhancements)
- [License](#license)

## Overview

This project implements an automated panorama stitching system that combines multiple overlapping images into a single seamless panoramic image. The implementation includes both a custom algorithm built from scratch and an OpenCV-based solution for comparison.

## Features

- **Dual Implementation**: Custom implementation and OpenCV-based solution
- **SIFT Feature Detection**: Scale-Invariant Feature Transform for robust feature matching
- **RANSAC-based Homography**: Robust estimation of transformation matrices
- **Advanced Image Warping**: Both forward and inverse projection techniques
- **Pyramid-based Blending**: Laplacian and Gaussian pyramids for seamless image blending
- **Multiple Scene Support**: Tested on 6 different scene datasets

## Requirements

- Python 3.7+
- OpenCV (cv2)
- NumPy
- tqdm (for progress bars)
- Jupyter Notebook

## Installation

1. Clone the repository:
```bash
git clone https://github.com/agarwal-mihir/Panorama-Stitching.git
cd Panorama-Stitching
```

2. Install required packages:
```bash
pip install opencv-python opencv-contrib-python numpy tqdm jupyter
```

## Usage

1. Open the Jupyter notebook:
```bash
jupyter notebook assignment_2.ipynb
```

2. Run all cells in the notebook to process all scene datasets

3. The notebook will:
   - Process images from the `dataset/` directory
   - Generate intermediate warped images
   - Create final blended panoramas
   - Save results in the `outputs/` directory

### Input Structure

Place your images in the following structure:
```
dataset/
├── scene1/
│   ├── I11.JPG
│   ├── I12.JPG
│   └── ...
├── scene2/
│   └── ...
```

### Output Structure

Results are saved in:
```
outputs/
├── scene1/
│   ├── custom/
│   │   ├── warped_0.png
│   │   ├── warped_1.png
│   │   └── blended_image.png
│   └── opencv/
│       └── ...
```

## Methodology

The algorithmic approach to solving the problem is organized into four main components:

### Feature Matching
- SIFT (Scale-Invariant Feature Transform) is used for feature extraction, offering strong invariance to rotation, scale, and partially to affine distortions.
- For identifying correspondences between features across two images, Brute-Force matching is employed based on a distance criterion.

```python
# Feature matching using SIFT and BFMatcher
```

### Homography Estimation
- A robust method for estimating the Homography matrix is carried out using the RANSAC (Random Sample Consensus) algorithm. The process involves 10,000 trials, picking four random matches in each run to determine the best-fit homography.

```python
# Robust Homography estimation using RANSAC
```

### Image Warping
- There are two types of warping techniques utilized:
    - **Forward Projection**: Directly maps source pixels to destination. While straightforward, it can result in 'holes'.
    - **Inverse Sampling**: Leverages the inverse of the homography matrix to fill each pixel in the destination image, thereby eliminating 'holes'.

- Optimization strategies involve transforming a computationally expensive \(O(n^2)\) loop into a matrix multiplication operation.
- For handling multiple images:
    1. The first image remains as-is.
    2. Each following image is warped in a cumulative manner, updating the homography matrix incrementally.

```python
# Efficient Image warping techniques
```

### Image Blending
- Both Laplacian and Gaussian pyramids are constructed to facilitate effective blending.
    - The Laplacian pyramid contains the granular details of the image.
    - The Gaussian pyramid serves as the blending mask.

- Blending strategies include:
    - **Vertical Stitching**: A basic left-right division is applied at each pyramid level.
    - **Diagonal Stitching**: This method uses a diagonal in the overlapping regions, enabling a more natural blend and better preservation of image content.

```python
# Advanced Image blending using pyramidal structures
```

## Complexity and Optimization

### Time Complexity
- **Feature matching and Homography estimation**: \(O(N \log N)\)
  - SIFT feature detection and matching
  - RANSAC algorithm with multiple iterations
- **Warping and blending**: \(O(N)\)
  - Matrix-based image transformation
  - Pyramid construction and blending

### Optimization Techniques
- Vectorized operations using NumPy for efficient matrix computations
- Inverse warping to eliminate holes in the destination image
- Efficient memory management for large panoramic images
- Progressive image stitching with cumulative homography updates

## Results

The implementation successfully stitches panoramas from 6 different scene datasets:
- **Scenes 1-3**: 4 images per scene (indoor and outdoor environments)
- **Scenes 4-6**: 2 images per scene (various perspectives)

Both custom and OpenCV implementations produce comparable results, with:
- Accurate feature matching and alignment
- Smooth blending without visible seams
- Preservation of image quality and details

Results can be found in the `outputs/` directory, organized by scene and implementation method.

## Future Enhancements

- **Advanced Blending**: Multi-band blending for smoother and more visually appealing results
- **Cylindrical/Spherical Projection**: Support for 360-degree panoramas
- **Automatic Image Ordering**: Intelligent detection of image sequence
- **Real-time Processing**: Optimization for video-based panorama creation
- **Bundle Adjustment**: Global optimization for multiple image stitching
- **GPU Acceleration**: CUDA-based implementation for faster processing

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

**Mihir Agarwal**

## Acknowledgments

- Assignment for the Digital Image Processing course
- OpenCV library for computer vision functionalities
- SIFT algorithm for robust feature detection

