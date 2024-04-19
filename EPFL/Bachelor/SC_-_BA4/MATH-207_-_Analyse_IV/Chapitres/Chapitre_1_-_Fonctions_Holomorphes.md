## Fonction complexe

Une fonction complexe est notée:    $\begin{cases} f \colon \mathbb{C}  & \longrightarrow \mathbb{C} \\  z & \longmapsto f(z)\end{cases}$ 
On écrira:
$$f(z) = f(x + iy) = \mathbb{R}e(f(x + iy)) + i \mathbb{I}m(f(x + iy)) = u(x,y) + iv(x,y)$$
avec $u$, $v$ :  $\mathbb{R}^{2} \rightarrow \mathbb{R}$

On peut assimiler $\mathbb{C} = \mathbb{R}^{2}$ par abus de language car il peut être associé par un unique point en $\mathbb{R}^2$ et inversement

## Fonction holomorphe

Soit $\Omega \subset \mathbb{C}$ ouvert et $z_{0}\in \mathbb{C}$ 
$f \colon \Omega \longmapsto \mathbb{C}$ est holomorphe en $z_{0}$ si la limite suivante existe: 
$$\lim_{h \longrightarrow0} \frac{f(z_{0} + h) -f(z_{0})}{h}$$
Si cette limite existe alors nous la notons $f'(z_{0})$ 
On dit que f est holomorphe sur $\Omega$ si f est holomorphe $\forall z_{0}\in \Omega$ 

## Théorème de Cauchy Riemann

Soit $\Omega \subset \mathbb{C}$ ouvert, $f=u + iv \, \colon \, \Omega \longmapsto \mathbb{C}$ 
$f$ est holomorphe sur $\Omega$ ssi les equations de Caucy Riemann sont satisfaites:
$$\begin{cases}
u,v = C^{1}(\Omega) \\
\frac{\partial u}{\partial x}= \frac{\partial v}{\partial y} \Longleftrightarrow u_{x}= v_{y} \\
\frac{\partial v}{\partial x}= -\frac{\partial u}{\partial y} \Longleftrightarrow v_{x}= -u_{y}
\end{cases}$$
De plus nous avons: $$f'(z) = u_{x} + iv_{x} = u_{y}- iv_y$$ 