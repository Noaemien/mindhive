## Fonction analytique
Une fonction définie sur $I$ est analytique si pour tout point de $I$ il existe un sous interval centré sur ce point ou la fonction est égale à sa propre série de Taylor.

## Taylor en $\mathbf{x_{0}}$ 
$$T_{x_{0}}(x) = \sum\limits^{\infty}_{n=0}\frac{f^{(n)}(x_{0})}{n!}(x-x_{0})^{n}$$
## Théorème
Soit $\Omega \subset \mathbb{C}$ ouvert, $f : \Omega \mapsto \mathbb{C}$ holomorphe, $z_{0} \in \Omega$ et $\epsilon > 0$ tel que:
$$B_{\epsilon}(z_{0}) = \set{z \in \mathbb{C} \, |\: |z-z_{0}| < \epsilon} \subset \Omega$$
Alors f est analytique dans cette boule, i.e:
$$f(z) = \sum\limits^{\infty}_{n=0}c_{n}(z-z_{0})^{n}, \: \forall z \in B_{\epsilon}(z_{0})$$
Avec les coefficients donnés par:
$$c_{n} = \frac{f^{(n)}(z_{0})}{n!}$$
Les fonctions holomorphes sont donc automatiquement analytiques.
La réciproque est vraie.

## Rayon de convergence
Le rayon de convergence est le plus grand nombre $R > 0$ ou f reste holomorphe dans $B_{R}(z_{0})$ 

## Règle de l'Hospital
Soit $\Omega \subset \mathbb{C}$ un ouvert, $z_{0} \in \Omega$, et $f$,$g : \Omega \mapsto \mathbb{C}$ des fonctions holomorphes telles que:
$$f(z_{0}) = g(z_{0}) = 0, \: g'(z_{0})\ne 0$$
alors nous avons:
$$\lim_{z \rightarrow z_{0}}\frac{f(z)}{g(z)} = \frac{f'(z_0)}{g'(z_{0})}$$

## Série de Laurent
Soit $\Omega \subset \mathbb{C}$ ouvert, $z_{0} \in \Omega$, et $f : \Omega \setminus \set{z_{0}} \mapsto \mathbb{C}$
La série de Laurent de $f$ en $z_{0}$, si elle existe, est:
$$L_{z_{0}}f(z) = \sum^{\infty}_{n=-\infty}c_{n}(z - z_{0})^{n}$$
La **partie régulière** est $\sum^{\infty}_{n=0}c_{n}(z - z_{0})^{n}$
La **partie singulière** est $\sum^{-1}_{n=-\infty}c_{n}(z - z_{0})^{n}$ 
Le coefficient $c_{-1}$ de $f$ en $z_0$ est le **résidu** noté $Res_{z_{0}}(f)$ 


## Théorème de Laurent
Soit $\Omega \subset \mathbb{C}$ ouvert, $z_{0} \in \Omega$ et $f: \Omega \setminus \set{z_{0}} \mapsto \mathbb{C}$ holomorphe.
Alors, $f$ admet un développement en série de Laurent. En d'autres mots, $\exists \epsilon \gt 0$ tel que, $\forall z \in B_{z}(z_{0}) \setminus \set{z_{0}}$ : $$f(z) = \sum^{\infty}_{n = - \infty} c_{n}(z - z_{0})^{n} = L_{z_{0}}f(z) $$
Le **Rayon de convergence** $R \gt 0$ est le plus grand $\epsilon \gt 0$ tel que:
$$f(z) = L_{z_{0}}f(z), \: \forall z \in B_{R}(z_{0}) \setminus \set{z_{0}}$$
>[!Info] Point Régulier
>Si la partie singulière de $L_{z_{0}}f$ est nulle, alors $z_{0}$ est appelé un **point régulier**. Dans ce cas, notre série de Laurent est une série de Taylor.

### Pôle
Si la partie singulière de $L_{z_{0}}f$ commence à la puissance $-m$ pour un certain $m \in \mathbb{N}^{*}$, $z_{0}$ est appelé un **pôle** d'ordre $m$. 
Dans ce cas, la partie singulière a un nombre fini de termes:
$$L_{z_{0}}f(z) = \sum^{\infty}_{n = -m} c_{n}(z-z_{0})^{n}$$