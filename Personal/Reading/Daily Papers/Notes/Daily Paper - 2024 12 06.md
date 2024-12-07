---
id: 3
Title: 3D Gaussian Splatting for Real-Time Radiance Field Rendering
published-date: 2023-08-08
Author:
  - Bernhard Kerbl
  - Georgios Kopanas
  - Thomas Leimkühler
  - George Drettakis
lecture-date: 2024-12-06
rating: -1
Link: https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/
---
 # Summary:

## Introduction

The usual way to represent 3d scenes used to be with meshes and points. Great for explicit scene description and fast compute.
Neural Radiance Field (NeRF) methods use continuous scene representation by optimising a Multi-Layer-Perceptron (MLP). These are great for scene optimization the rendering process is computationally expensive and noisy.
3DGS offers a solution to improve this whilst maintaining competitive training times.

Based of **three components**:
1. 3D Gaussians as a scene representation.
	- Have a differentiable volumetric representation
	- rasterized efficiently by projecting to 2d and applying $\alpha$-blending
2. 3D Gaussian property optimisation
	- 3D position
	- opacity $\alpha$
	- anisotropic covariance
	- spherical harmonics (SH) coefficients
	- density control (adding / removing gaussians)
3. Real-time rendering solution
	- uses GPU sorting algorithms
	- inspired by tile-based rasterization

## Method
### Overview
**Model Input**: Set of scene images with Sfm calibration and sparse point cloud.
**Initialization**:  Sparse point cloud is used to initialize 3D gaussians defined by a position, covariance matrix, opacity $\alpha$. The color of the radiance field is represented via spherical harmonics following standard practice.
**Training**: Algorithm is trained via many optimisation steps of 3D Gaussian parameters. The tile-based rasterizer allows for a fast backwards pass by tracking accumulated $\alpha$ values, enabling unlimited gaussians to be able to recieve gradients.

### Differentiable 3D Gaussian Splatting

Gaussians are represented by **3D covariance matrix $\Upsigma$** defined in world space centered at point $\mu$ 
$$G(x) = e^{\frac{-1}{2}(x)^{T}\Upsigma^{-1}(x)}$$
$$\text{with }\Upsigma = \begin{bmatrix}
var(x) & cov(x, y) & cov(x,z) \\ cov(y, x) & var(y) & cov(y, z) \\ cov(z, x) & cov(z, y) & var(z)
\end{bmatrix}$$
Gaussians are multiplied by opacity $\alpha$ during blending process.

**Projecting 3D Gaussians to 2D:**

Given viewing transformation W TODO: Define W
