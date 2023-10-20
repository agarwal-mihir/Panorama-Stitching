#  Panorama Stitching

This is my submission for the Assignment - 2 : Panorama Stitching, for the course - Digital Image Processing. I have implemented automated image stiching using both custom and an opencv implementation. 

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
- Feature matching and Homography estimation: \(O(N \log N)\)
- Warping and blending: \(O(N)\)

## Future Enhancements
- More advanced blending methods, such as multi-band blending, can be explored for smoother and more visually appealing results.

