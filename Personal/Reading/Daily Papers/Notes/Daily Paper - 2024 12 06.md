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
rating: 5
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

Model is optimized through successive iterations of rasterizing scene from camera positions and comparing to ground truth images.

Stochastic Gradient Descent is used to train the models parameters.
The loss function is a combination of $\mathcal{L}_{1}$ and D-SSIM
$$\mathcal{L} = (1-\lambda)\mathcal{L}_{1} + \lambda \mathcal{L}_\text{D-SSIM}$$ 

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

![[3dgs_optimizer.png|500]]

---


### Fast Differentiable Rasterizer for Gaussians


![[3dgs_rasterizer.png|500]]

### Spherical Harmonics
Functions defined on the surface of a sphere that describe its color dependant on viewing direction.
Allows the model to represent view dependant effects such as specularity

Store a vector of spherical harmonic coefficients for each color channel (RGB).

# Article

## 3D Gaussian Splatting for Real Time Radiance Field Rendering.  
A deep-dive into the paper that revolutionised modern scene reconstruction techniques, yielding high-resolution results and real-time rendering with fast training times.  
A Thread 🧵(1/?):
![[playroom.mp4]]
(2/)
3DGS offers 3 main improvements on previous methods:
- Use of 3D Gaussians for scene representation.
- Optimization of the 3D Gaussians properties such as: position, opacity, covariance and spherical harmonic coefficients.
- Real-time scene rendering.

(3/)
In the spirit of Neural Radiance Fields (NeRFs), 3DGS steps away from traditional scene representation with meshes and points in favour of using a continuous and differentiable scene representation in the form of 3D Gaussians. This allows for the scene to be fitted through optimisation steps yielding high resolution scenes and fast training times.

![[meshvsgauss.mp4]]

(4/)
**What are 3D Gaussians ?**

3D gaussians defined by the following properties:
- A mean or "position" $\mu =$ (x, y, z)
- A 3x3 covariance matrix $\Sigma$
- Opacity $\alpha$
- Spherical Harmonics coefficients that represent r, g, b channels.


![[properties.mp4]]
3D Gaussians are a probabilistic function defined by a 3D covariance matrix $\Upsigma$:
$$G(x) = e^{\frac{-1}{2}(x)^{T}\Upsigma^{-1}(x)}$$
Although they can be hard to visualize, 3D gaussians can be imagined as an ellipsoid and its covariance is analogous to describing ones configuration.
For this reason we can describe sigma as:
$$\Upsigma = RSS^{T}R^{T}$$
$$\text{with } S = \begin{bmatrix}
s_{x} & 0 & 0 \\ 0 & s_{y} & 0 \\ 0 & 0 & s_{z}
\end{bmatrix}$$
$$\text{and } R = \begin{bmatrix}
1 - 2(q_{j}^{2} + q_{k}^{2}) & 2(q_{i}q_{j} - q_{k}q_{r}) & 2(q_{i}q_{k} + q_{j}q_{r}) \\ 2(q_{i}q_{j} + q_{k}q_{r})&  1 - 2(q_{i}^{2} + q_{k}^{2}) & 2(q_{j}q_{k} - q_{i}q_{r}) \\ 2(q_{i}q_{k} - q_{j}q_{r}) & 2(q_{j}q_{k} + q_{i}q_{r}) & 1 - 2(q_{i}^{2} + q_{j}^{2})
\end{bmatrix}$$
$\text{defined by quaternion } q=q_{r}+q_{i}i + q_{j}j + q_{k}k \text{ and vector } s = [s_{x}, s_{y}, s_{z}]$ 

Being volumetric in nature, 3d gaussians are particularly useful as they can be easily projected to 2D image coordinates, which is used during the optimization process.

> [! Reminder] Affine mappings of multi-variate gaussian random variables:  
>Given $X \sim N(\bar{\mu}, \Sigma)$   
>$$y = AX + B$$
>$$cov(Y) = A \Sigma A^{T}$$
>$$ \mu_{Y} = A \mu_{X} + b$$
>If we want to apply a mapping $m$ which is not affine, we can use the affine approximation using the first two terms of the Taylor expansion of $m$ at point $x_{k}$: $$m_{x_{k}}(x) = m(x_{k}) + J_{x_{k}} (x-x_{k})$$ 
 

The 2D Covariance Matrix is derived by applying two affine mappings.
- The extrinsic camera matrix $W = [R | T]$ which maps the covariance matrix from world space to 3D camera space
- The affine approximation of the projective transformation from camera space to image space given by the intrinsic camera matrix $$K = \frac{1}{z}\begin{bmatrix} f_{x} & s & c_{x} \\ 0 & f_{y} & c_{y} \\ 0 & 0 & 1\end{bmatrix}$$
This means $\Sigma'$, the 2D covariance matrix, can be written as:
$$\Sigma' = JW\Sigma W^{T}J^{T} $$
where J is the Jacobian of K.


(6/)
**The Optimization process:**

The 3DGS algorithm is trained through multiple iterations of rendering (or rasterizing) the scene and comparing the output to the training images.

Like in traditional Multi-Layer Perceptrons (MLP's), Stockastic Gradient Descent is used to optimize the model parameters.

In addition to this, adaptive density control is used to control the quantity of gaussians in a scene, adding or removing them where necessary.
Every 100 iterations, the network removes all gaussians with a very low opacity $\alpha$ below a chosen threshold, and applies its densification protocol by detecting under-reconstructed regions and over-reconstructed regions.

When a region's gradient is too high, the densification protocol either splits Gaussians if they are deemed too big, or otherwise clones them.

![[Densification.png]]

In addition to this, to prevent overflow, every 3000 iterations, all gaussians opacity is reset to a very small value close to 0.
Thanks to this optimization will increase $\alpha$ for useful gaussians and useless ones will be culled.

![[3dgs_optimizer.png|500]]

(7/)
**The Rasterization Process**

The rasterization process starts by splitting image space into 16x16 pixel tiles, and culling gaussians that don't have a 99% confidence interval of intersecting with the tile's viewing frustum.
Gaussians too close to near plane or too far outside frustum are also culled.
![[Viewing frustum.png]]
Gaussians are then duplicated as many times as tiles they overlap and are each assigned a 64 bit key where view space depth is the lower 32 bits and tile ID is the higher 32 bits.
This enables the model to sort gaussians once, selecting them by tile id in key.
Pixels then each traverse the tiles gaussians from closest to furthest, adding $\alpha$ blended colors until target saturation is reached.

![[3dgs_rasterizer.png|500]]

(8/8)
**Spherical Harmonics**
Spherical harmonics are functions defined on the surface of a sphere that are used, in this case, to describe gaussians color dependant on viewing direction.
![[Spherical Harmonics function.png]]
This allows the trained model to represent view dependant effects such as reflections and specularity.

![[Spherical Harmonics.png|500]]



In the 3DGS algorithm, spherical harmonics are used by performing a weighted sum of the function for all degrees l and values m up to l = 2
A vector of these coefficients coefficients is stored for each color channel (RGB).
This enables to model to accurately optimize view dependent color for any non transparent object whilst storing only 27 different coefficient values per gaussian.

**Good References:**
Original paper: https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/
Good ressource: https://towardsdatascience.com/a-comprehensive-overview-of-gaussian-splatting-e7d570081362
Good implementation: https://github.com/nerfstudio-project/nerfstudio/