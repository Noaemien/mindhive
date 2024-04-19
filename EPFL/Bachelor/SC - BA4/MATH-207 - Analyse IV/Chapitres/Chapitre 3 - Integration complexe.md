
## Courbes
### Courbe régulière
L'ensemble $\Gamma \subset \mathbb{C}$ est une *courbe régulière* si il existe $[a, b] \subset \mathbb{R}$, et une fonction $\gamma$, appelé une paramétrisation de $\Gamma$ :
$$\begin{align*}
\gamma \colon [a,b] & \longmapsto \Gamma \subset \mathbb{C}\\
t & \longmapsto \gamma(t) &= \alpha(t) + i \beta(t)
\end{align*}$$
Cette paramétrisation doit suivre les propriétés suivantes:
1. $\Gamma = \{\gamma(t)|t \in [a,b]\}$ 
2. $\gamma \in C^{1}(]a,b[, \mathbb{C})$ 
3. $\gamma'(t) = \alpha'(t) + i\beta'(t) \ne 0$ pour tout $t \in ]a,b[$ 
### Courbe régulière simple
Une courbe régulière $\Gamma$ de paramétrisation $\gamma \colon [a,b] \longmapsto \Gamma$ est *simple* si $\gamma$ est injective sur $]a,b[$ 

### Courbe régulière fermée
Une courbe régulière $\Gamma$ de paramétrisation $\gamma \colon [a, b] \mapsto \Gamma$ est *fermée* si:
$$ \gamma(a) = \gamma(b)$$

#### Orientation
Une courbe régulière fermée est orientée positivement si elle laisse l'intérieur à gauche
### Courbe régulière par morceau
Courbe qui est l'union de plusieurs courbes régulières

## Intégrale complexe
### Définition

Soit $\Omega \subset \mathbb{C}$ un ensemble ouvert, $f(z) : \Omega \mapsto \mathbb{C}$ une fonction continue, et $\Gamma \subset \Omega$ une courbe.
Si $\Gamma \subset \mathbb{C}$ une courbe régulière de paramétrisation $\gamma \colon [a, b] \mapsto \Gamma$, alors l'intégrale de $f$ le long de $\Gamma$ est le nombre:
$$\int_{\Gamma}fdz \stackrel{\mathclap{\textnormal{déf}}}{=} \int^{b}_{a} f(\gamma(t)) * \gamma'(t)dt$$
Si $\Gamma$ régulière par morceaux -> sommes d'intégrales

### Théorème de Cauchy
Soit $\Omega \subset \mathbb{C}$ un ensemble ouvert, $f : \Omega \mapsto \mathbb{C}$ holomorphe, et $\Gamma$ une courbe fermée, régulière par morceaux et simplement connexe.
Alors le théorème de Cauchy dit:
$$\int^{}_{\Gamma}fdz = 0$$
La réciproque est également vraie

### Formule Intégrale de Cauchy (FIC)

Soit $\Omega \subset \mathbb{C}$ un ensemble ouvert,  $f : \Omega \mapsto \mathbb{C}$ holomorphe, et $\Gamma$ une courbe fermée et régulière par morceaux telle que $\textnormal{int}(\Gamma) \cup \Gamma = \textnormal{int}(\Gamma) \subset \Omega$
Si de plus $\gamma$ est orienté positivement, et $z_{0}\in \textnormal{int}(\Gamma)$, alors FIC dit que, pour tout $n \in \mathbb{N}$:
$$\int^{}_{\Gamma}\frac{f(z)}{(z-z_{0})^{n+1}}dz = 2\pi i \frac{f^{(n)}(z_{0})}{n!}$$

On appelle $z_{0}$ une singularité. L'intégrale ne dépend que de l'existence de cette singularité dans l'intérieur de la courbe. Même si $f$ est holomorphe $\frac{f(z)}{(z-z_{0})^{n+1}}$ ne l'est pas à cause de sa singularité.
Dès qu'on voit une singularité a l'intérieur de la courbe d'intégration on peut immédiatement essayer d'utiliser ce théorème
**Cas n = 0**
- $\int^{}_{\Gamma}\frac{f(z)}{z-z_{0}}dz = 2\pi i f(z_{0})$
#### Corollaire
Si $f$ est holomorphe alors $f$ est infiniment dérivable.
Intuition: Découle directement de $f^{(n)}(z_{0}) = \frac{n!}{2\pi i} \int^{}_{\Gamma}\frac{f(z)}{(z-z_{0})^{n+1}}dz$ 