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
For optimisation, $\Upsigma$ is defined like an ellipsoid, with a scaling matrix $S$ and a rotation matrix $R$.
$$\Upsigma = RSS^{T}R^{T}$$
$$\text{with } S = \begin{bmatrix}
s_{x} & 0 & 0 \\ 0 & s_{y} & 0 \\ 0 & 0 & s_{z}
\end{bmatrix}$$
$$\text{and } R = \begin{bmatrix}
1 - 2(q_{y}^{2} + q_{z}^{2}) & 2(q_{x}q_{y} - q_{z}q_{r}) & 2(q_{x}q_{z} + q_{y}q_{r}) \\ 2(q_{x}q_{y} + q_{z}q_{r})&  1 - 2(q_{x}^{2} + q_{z}^{2}) & 2(q_{y}q_{z} - q_{x}q_{r}) \\ 2(q_{x}q_{z} - q_{y}q_{r}) & 2(q_{y}q_{z} + q_{x}q_{r}) & 1 - 2(q_{x}^{2} + q_{y}^{2})
\end{bmatrix}$$
$\text{defined by } q=q_{r}+q_{x}x + q_{y}y + q_{z}z \text{ and } s = [s_{x}, s_{y}, s_{z}]$  

**Projecting 3D Gaussians to 2D:**

Given viewing transformation W defined by camera's extrinsic matrix:
$$ W = \begin{bmatrix}
R & T
\end{bmatrix}$$$$\text{with } R = \begin{bmatrix}
r_{1, 1} & r_{1, 2} & r_{1, 2} \\ r_{2, 1} & r_{2, 2} & r_{2, 3} \\ r_{3, 1} & r_{3, 2} & r_{3, 3}
\end{bmatrix} \text{the camera rotation matrix}$$$$\text{and } T = \begin{bmatrix}
t_{1} \\ t_{2} \\ t_{3}
\end{bmatrix} \text{the camera position in world coordinates}$$
The 2d covariance matrix $\Upsigma'$ in camera coordinates is defined by $$\Upsigma' = JW \Upsigma W^{T}J^{T}$$ with $J$ being the Jacobian of the affine approximation of projective transformation from 3D to 2D
The projection is defined as: $$\begin{bmatrix}
x' \\ y' \\ z'
\end{bmatrix} = \frac{1}{z}\begin{bmatrix}
f_{x} & s & c_{x} \\ 0 & f_{y} & c_{y} \\ 0 & 0 & 1
\end{bmatrix} \begin{bmatrix}
X \\ Y \\ Z
\end{bmatrix}$$
with $f$ the focal length, $s$ the skew and $(c_{x}, c_{y})$ the principal point coordinates.
$$J \approx \begin{bmatrix}
\frac{\partial x'}{dx} & \frac{\partial x'}{dy} & \frac{\partial x'}{dz} \\ \frac{\partial y'}{dx} & \frac{\partial y'}{dy} & \frac{\partial y'}{dz} \\ \frac{\partial z'}{dx} & \frac{\partial z'}{dy} & \frac{\partial z'}{dz}
\end{bmatrix} = \begin{bmatrix}
\frac{f_{x}}{z} & \frac{s}{z} & -\frac{f_{x}X}{z^{2}}-\frac{sY}{z^{2}}  \\ 0 & \frac{f_{y}}{z} & -\frac{f_{y}X}{z^{2}}  \\ 0 & 0 & 0
\end{bmatrix}$$
### Optimization with adaptive density control of 3D Gaussians
#### Optimization


#### Adaptive Control of Gaussians
Densify every 100 iterations and remove any transparent Gaussians, where $\alpha < \epsilon_{\alpha}$.

Needs to populate empty areas, regions with missing geometric features ("under-reconstruction") and regions where Gaussians cover large scene areas ("over-reconstruction").

**under-reconstruction:**
- We want to cover new geometry that must be created
- Clone existing gaussian and move in direction of positional gradient
**over-reconstruction:**
- We want to add more detail whilst maintaining volume
- Split large gaussian into two smaller ones (scale divided by $\phi = 1.6$) and initialize their position using Probability Density Function of original gaussian

**Preventing Gaussian Population Overflow**
Every 3000 iterations, set all Gaussians $\alpha \approx 0$. This allows optimisation to increase $\alpha$ for useful Gaussians and useless gaussians will be culled as $\alpha \lt \epsilon_{\alpha}$.
Periodically remove gaussians that are too large in world space or view space.


### Fast Differentiable Rasterizer for Gaussians
