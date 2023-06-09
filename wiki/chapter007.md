!!Polyglot
#page(0)

# Chapter 7: Directional Derivatives

The vector field was a generalization of the directional derivative to functions on a manifold. When we want to generalize the directional derivative idea to operate on other manifold objects, such as directional derivatives of vector fields or of form fields, there are several useful choices. In the same way that a vector field applies to a function to produce a function, we will build directional derivatives so that when applied to any object it will produce another object of the same kind. All directional derivatives require a vector field to give the direction and scale factor.

We will have a choice of directional derivative operators that give different results for the rate of change of vector and form fields along integral curves. But all directional derivative operators must agree when computing rates of change of functions along integral curves. When applied to functions, all directional derivative operators give:

$$\begin{equation}
\mathcal{D}_{\mathsf{v}}(\mathsf{f}) = \mathsf{v}(\mathsf{f}).
\end{equation}$$

Next we specify the directional derivative of a vector field $\mathsf{u}$ with respect to a vector field $\mathsf{v}$. Let an integral curve of the vector field $\mathsf{v}$ be γ, parameterized by $t$, and let $\mathsf{m} = \gamma(t)$. Let $\mathsf{u}^{\prime}$ be a vector field that results from transporting the vector field $\mathsf{u}$ along γ for a parameter increment δ. How $\mathsf{u}$ is transported to make $\mathsf{u}^{\prime}$ determines the type of derivative. We formulate the method of transport by:

$$\begin{equation}
\mathsf{u}^{\prime} = F^{\mathsf{v}}_{\delta}\mathsf{u}.
\end{equation}$$

We can asume without loss of generality that $F^{\mathsf{v}}_{\delta}\mathsf{u}$ is a linear transformation over the reals on $\mathsf{u}$, because we care about its behavior only in an incremental region around $\delta = 0$.

Let $g$ be the comparison of the original vector field at a point with the transported vector field at that point:

$$\begin{equation}
g(\delta) = \mathsf{u}(\mathsf{f})(\mathsf{m})-(F^{\mathsf{v}}_{\delta}\mathsf{u})(\mathsf{f})(\mathsf{m}).
\end{equation}$$

So we can compute the directional derivative operator using only ordinary derivatives:

$$\begin{equation}
\mathcal{D}_{\mathsf{v}}\mathsf{u}(\mathsf{f})(\mathsf{m}) = Dg(0).
\end{equation}$$

The result $\mathcal{D}_{\mathsf{v}}\mathsf{u}$ is of type vector field.

The general pattern of constructing a directional derivative operator from a transport operator is given by the following schema:#Footnote(1)

```Scheme
(define (((((F->directional-derivative F) v) u) f) m)
(define (g delta)
(- ((u f) m) (((((F v) delta) u) f) m)))
((D g) 0))
```

The linearity of transport implies that

$$\begin{equation}
\mathcal{D}_{\mathsf{v}}(\alpha\mathsf{O}+\beta\mathsf{P}) = \alpha\mathcal{D}_{\mathsf{v}}\mathsf{O}+\beta\mathcal{D}_{\mathsf{v}}\mathsf{P},
\end{equation}$$

for any real α and β and manifold objects $\mathsf{O}$ and $\mathsf{P}$.

The directional derivative obeys superposition in its vector-field argument:

$$\begin{equation}
\mathcal{D}_{\mathsf{v}+\mathsf{w}} = \mathcal{D}_{\mathsf{v}}+\mathcal{D}_{\mathsf{w}}.
\end{equation}$$

The directional derivative is homogeneous over the reals in its vector-field argument:

$$\begin{equation}
\mathcal{D}_{\alpha\mathsf{v}} = \alpha\mathcal{D}_{\mathsf{v}},
\end{equation}$$

for any real α.#Footnote(2) This follows from the fact that for evolution along integral curves: when α is a real number,

$$\begin{equation}
\phi^{\alpha\mathsf{v}}_{t}(\mathsf{m}) = \phi^{\mathsf{v}}_{\alpha t}(\mathsf{m}).
\end{equation}$$

When applied to products of functions, directional derivative operators satisfy Leibniz's rule:

$$\begin{equation}
\mathcal{D}_{\mathsf{v}}(\mathsf{f}\mathsf{g}) = \mathsf{f}(\mathcal{D}_{\mathsf{v}}\mathsf{g})+(\mathcal{D}_{\mathsf{v}}\mathsf{f})\mathsf{g.}
\end{equation}$$

The Leibniz rule is extended to applications of one-form fields to vector fields:

$$\begin{equation}
\mathcal{D}_{\mathsf{v}}(\omega(\mathsf{y}))= \omega(\mathcal{D}_{\mathsf{v}}\mathsf{y})+(\mathcal{D}_{\mathsf{v}}\omega)(\mathsf{y}).
\end{equation}$$

The extension of the Leibniz rule, combined with the choice of transport of a vector field, determines the action of the directional derivative on form fields.#Footnote(3)

### Lie Derivative

The Lie derivative is one kind of directional derivative operator. We write the Lie derivative operator with respect to a vector field $\mathsf{v}$ as $\mathcal{L}_{\mathsf{v}}$.

### Functions

The Lie derivative of the function $\mathsf{f}$ with respect to the vector field $\mathsf{v}$ is given by:

$$\begin{equation}
\mathcal{L}_{\mathsf{v}}\mathsf{f} = \mathsf{v}(\mathsf{f}).
\end{equation}$$

The tangent vector $\mathsf{v}$ measures the rate of change of $\mathsf{f}$ along integral curves.

### Vector Fields

For the Lie derivative of a vector field $\mathsf{y}$ with respect to a vector field $\mathsf{v}$ we choose the transport operator $F^{\mathsf{v}}_{\delta}\mathsf{y}$ to be the pushforward of $\mathsf{y}$ along the integral curves of $\mathsf{v}$. Recall equation (6.15). So the Lie derivative of $\mathsf{y}$ with respect to $\mathsf{v}$ at the point $\mathsf{m}$ is

$$\begin{equation}
(\mathcal{L}_{\mathsf{v}}\mathsf{y})(\mathsf{f})(\mathsf{m}) = Dg(0),
\end{equation}$$

where

$$\begin{equation}
g(\delta) = \mathsf{y}(\mathsf{f})(\mathsf{m}) - ((\phi^{\mathsf{v}}_{\delta})_{*}\mathsf{y})(\mathsf{f})(\mathsf{m}).
\end{equation}$$

We can construct a procedure that computes the Lie derivative of a vector field by supplying an appropriate transport operator (F-Lie phi) for F in our schema F->directional-derivative. In this first stab at the Lie derivative, we introduce a coordinate system and we expand the integral curve to a given order. Because in the schema we evaluate the derivative of $g$ at 0, the dependence on the order and the coordinate system disappears. They will not be needed in the final version.

```Scheme
(define (Lie-directional coordsys order)
(let ((Phi (phi coordsys order)))
(F->directional-derivative (F-Lie Phi))))

(define (((F-Lie phi) v) delta)
(pushforward-vector ((phi v) delta) ((phi v) (- delta))))

(define ((((phi coordsys order) v) delta) m)
((point coordsys)
(series:sum (((exp (* delta v)) (chart coordsys)) m)
order)))
```

Expand the quantities in equation (7.13) to first order in δ:

$$\begin{equation}
\begin{aligned}
g(\delta) &= \mathsf{y}(\mathsf{f})(\mathsf{m})-(\phi^{\mathsf{v}}_{\delta *}\mathsf{y})(\mathsf{f})(\mathsf{m}) \\
&=\mathsf{y}(\mathsf{f})(\mathsf{m})-\mathsf{y}(\mathsf{f}\circ\phi^{\mathsf{v}}_{\delta})(\phi^{\mathsf{v}}_{-\delta}(\mathsf{m})) \\
&=(\mathsf{y}(\mathsf{f})-\mathsf{y}(\mathsf{f}+\delta\mathsf{v}(\mathsf{f})+\cdots)\\
&+\delta\mathsf{v}(\mathsf{y}(\mathsf{f}+\delta\mathsf{v}(\mathsf{f})+\cdots)))(\mathsf{m})+\cdots \\
&=(-\delta\mathsf{y}(\mathsf{v}(\mathsf{f}))+\delta\mathsf{v}(\mathsf{y}(\mathsf{f})))(\mathsf{m})+\cdots \\
&=\delta[\mathsf{v},\mathsf{y}](\mathsf{f})(\mathsf{m})+\mathcal{O}(\delta^{2}).
\end{aligned}
\end{equation}$$

So the Lie derivative of a vector field $\mathsf{y}$ with respect to a vector field $\mathsf{v}$ is a vector field that is defined by its behavior when applied to an arbitrary manifold function $\mathsf{f}$:

$$\begin{equation}
(\mathcal{L}_{\mathsf{v}}\mathsf{y})(\mathsf{f}) = [\mathsf{v},\mathsf{y}](\mathsf{f})
\end{equation}$$

Verifying this computation

```Scheme
(let ((v (literal-vector-field 'v-rect R3-rect))
(w (literal-vector-field 'w-rect R3-rect))
(f (literal-manifold-function 'f-rect R3-rect)))
((- ((((Lie-directional R3-rect 2) v) w) f)
((commutator v w) f))
((point R3-rect) (up 'x0 'y0 'z0))))
0
```
Although this is tested to second order, evaluating the derivative at zero ensures that first order is enough. So we can safely define:

```Scheme
(define ((Lie-derivative-vector V) Y)
(commutator V Y))
```
We can think of the Lie derivative as the rate of change of the manifold function $\mathsf{y}(\mathsf{f})$ as we move in the $\mathsf{v}$ direction, adjusted to take into account that some of the variation is due to the variation of $\mathsf{f}$:

$$\begin{equation}
\begin{aligned}
(\mathcal{L}_{\mathsf{v}}\mathsf{y})(\mathsf{f}) = [\mathsf{v},\mathsf{y}](\mathsf{f}) \\
&=\mathsf{v}(\mathsf{y}(\mathsf{f}))-\mathsf{y}(\mathsf{v}(\mathsf{f})) \\
&=\mathsf{v}(\mathsf{y}(\mathsf{f}))-\mathsf{y}(\mathcal{L}_{\mathsf{v}}(\mathsf{f})).
\end{aligned}
\end{equation}$$

The first term in the commutator, $\mathsf{v}(\mathsf{y}(\mathsf{f}))$, measures the rate of change of the combination $\mathsf{y}(\mathsf{f})$ along the integral curves of $\mathsf{v}$. The change in $\mathsf{y}(\mathsf{f})$ is due to both the intrinsic change in $\mathsf{y}$ along the curve and the change in $\mathsf{f}$ along the curve; the second term in the commutator subtracts this latter quantity. The result is the intrinsic change in $\mathsf{y}$ along the integral curves of $\mathsf{v}$.

Additionally, we can extend the product rule, for any manifold function $\mathsf{g}$ and any vector field $\mathsf{u}$:

$$\begin{equation}
\begin{aligned}
\mathcal{L}_{\mathsf{v}}(\mathsf{g}\mathsf{u})(\mathsf{f})=[\mathsf{v},\mathsf{g}\mathsf{u}](\mathsf{f}) \\
&=\mathsf{v}(\mathsf{g})\mathsf{u}(\mathsf{f})+\mathsf{g}[\mathsf{v},\mathsf{u}](\mathsf{f}) \\
&=(\mathcal{L}_{\mathsf{v}}\mathsf{g})\mathsf{u}(\mathsf{f})+\mathsf{g}(\mathcal{L}_{\mathsf{v}}\mathsf{u})(\mathsf{f}).
\end{aligned}
\end{equation}$$

### An Alternate View

We can write the vector field

$$\begin{equation}
\mathsf{y}(\mathsf{f})=\sum_{i}y^{i}\mathsf{e}_{i}(\mathsf{f}).
\end{equation}$$

By the extended product rule (equation 7.17) we get

$$\begin{equation}
\mathcal{L}_{\mathsf{v}}\mathsf{y}(\mathsf{f})=\sum_{i}(\mathsf{v}(\mathsf{y}^{i})\mathsf{e}_{i}(\mathsf{f})+\mathsf{y}^{i}\mathcal{L}_{\mathsf{v}}\mathcal{e}_{i}(\mathsf{f})).
\end{equation}$$

Because the Lie derivative of a vector field is a vector field, we can extract the components of $\mathcal{L}_{\mathsf{v}}\mathsf{e}_{i}$ using the dual basis. We define $\Delta^{i}_{j}(\mathsf{v})$ to be those components:

$$\begin{equation}
\Delta^{i}_{j}(\mathsf{v}) = \tilde{\mathsf{e}}^{i}(\mathcal{L}_{\mathsf{v}}\mathsf{e}_{j}) = \tilde{\mathsf{e}}^{i}([\mathsf{v},\mathsf{e}_{j}]).
\end{equation}$$

So the Lie derivative can be written

$$\begin{equation}
(\mathcal{L}_{\mathsf{v}}\mathsf{y})(\mathsf{f}) = \sum_{i}\bigg(\mathsf{v}(\mathsf{y}^{i})+\sum_{j}\Delta^{i}_{j}(\mathsf{v})\mathsf{y}^{j}\bigg)\mathsf{e}_{i}(f).
\end{equation}$$

The components of the Lie derivatives of the basis vector fields are the structure constants for the basis vector fields. (See equation 4.37.) The structure constants are antisymmetric in the lower indices:

$$\begin{equation}
\tilde{\mathsf{e}}^{i}(\mathcal{L}_{\mathsf{e}_{k}}\mathsf{e}_{j}) = \tilde{\mathsf{e}}^{i}([\mathsf{e}_{k},\mathsf{e}_{j}]) = \mathsf{d}^{i}_{kj}.
\end{equation}$$

Resolving $\mathsf{v}$ into components and applying the product rule, we get

$$\begin{equation}
(\mathcal{L}_{\mathsf{v}}\mathsf{y})(\mathsf{f}) = \sum_{k}\big(\mathsf{v}^{k}[\mathsf{e}_{k},\mathsf{y}](\mathsf{f})-\mathsf{y}(\mathsf{v}^{k})\mathsf{e}_{k})(\mathsf{f})\big).
\end{equation}$$

So $\Delta^{i}_{j}$ is related to the structure constants by

$$\begin{equation}
\begin{aligned}
\Delta^{i}_{j}(\mathsf{v}) = \tilde{\mathsf{e}}^{i}(\mathcal{L}_{\mathsf{v}}\mathsf{e}_{j}) \\
&=\sum_{k}\big(\mathsf{v}^{k}\tilde{\mathsf{e}}^{i}([\mathsf{e}_{k},\mathsf{e}_{j}])-\mathsf{e}_j(\mathsf{v}^{k})\tilde{\mathsf{e}}^{i}(\mathsf{e}_{k})\big) \\
&=\sum_{k}\big(\mathsf{v}^{k}\mathsf{d}^{i}_{kj}-\mathsf{e}_{j}(\mathsf{v}^{k})\delta^{i}_{k}\big) \\
&=\sum_{k}\mathsf{v}^{k}\mathsf{d}^{i}_{kj}-\mathsf{e}_{j}(\mathsf{v}^{i}).
\end{aligned}
\end{equation}$$

Note: Despite their appearance, the $\Delta^{i}_{j}$ are not form fields because $\Delta^{i}_{j}(\mathsf{f}\mathsf{v})\neq\mathsf{f}
\Delta^{i}_{j}(\mathsf{v})$.

### Form Fields

We can also define the Lie derivative of a form field ω with respect to the vector field $\mathsf{v}$ by its action on an arbitrary vector field $\mathsf{y}$, using the extended Leibniz rule (see equation 7.10):

$$\begin{equation}
(\mathcal{L}_{\mathsf{v}}(\omega))(\mathsf{y})\equiv\mathsf{v}(\omega(\mathsf{y}))-\omega(\mathcal{L}_{\mathsf{v}}\mathsf{y}).
\end{equation}$$

The first term computes the rate of change of the combination $\omega(\mathsf{y})$ along the integral curve of $\mathsf{v}$, while the second subtracts ω applied to the change in $\mathsf{y}$. The result is the change in ω along the curve.

The Lie derivative of a $k$-form field ω with respect to a vector field $\mathsf{v}$ is a $k$-form field that is defined by its behavior when applied to $k$ arbitrary vector fields $\mathsf{w}_{0},\ldots,\mathsf{w}_{k-1}$. We generalize equation (7.25):

$$\begin{equation}
\begin{aligned}
\mathcal{L}_{\mathsf{v}}&\omega(\mathsf{w}_{0},\ldots,\mathsf{w}_{k-1}) \\
&= \mathsf{v}(\omega(\mathsf{w}_{0},\ldots,\mathsf{w}_{k-1}))-\sum_{i=0}^{k-1}\omega(\mathsf{w}_{0},\ldots,\mathcal{L}_{\mathsf{v}}\mathsf{w}_{i},\ldots,\mathsf{w}_{k-1}).
\end{aligned}
\end{equation}$$

### Uniform Interpretation

Consider abstracting equations (7.16), (7.25), and (7.27). The Lie derivative of an object, $\mathsf{a}$, that can apply to other objects, $\mathsf{b}$, to produce manifold functions, $\mathsf{a}(\mathsf{b}):\mathsf{M}\to\mathsf{R}^{n}$, is

$$\begin{equation}
(\mathcal{L}_{\mathsf{v}}\mathsf{a})(\mathsf{b}) = \mathsf{v}(\mathsf{a}(\mathsf{b}))-\mathsf{a}(\mathcal{L}_{\mathsf{v}}\mathsf{b}).
\end{equation}$$

The first term in this expression computes the rate of change of the compound object $\mathsf{a}(\mathsf{b})$ along integral curves of $\mathsf{v}$, while the second subtracts the change in $\mathsf{a}$ due to the change in $\mathsf{b}$ along the curves. The result is a measure of the "intrinsic" change in $\mathsf{a}$ along integral curves of $\mathsf{v}$, with $\mathsf{b}$ held "fixed."

### Properties of the Lie Derivative

As required by properties 7.7-7.5, the Lie derivative is linear in its arguments:

$$\begin{equation}
\mathcal{L}_{\alpha\mathsf{v}+\beta\mathsf{w}} = \alpha\mathcal{L}_{\mathsf{v}}+\beta\mathcal{L}_{\mathsf{w}},
\end{equation}$$

and

$$\begin{equation}
\mathcal{L}_\mathsf{v}(\alpha\mathsf{a}+\beta\mathsf{b})=\alpha\mathcal{L}_{\mathsf{v}}\mathsf{a}+\beta\mathcal{L}_{\mathsf{v}}\mathsf{b},
\end{equation}$$

with $\alpha,\beta\in\mathsf{R}$ and vector fields or one-form fields $\mathsf{a}$ and $\mathsf{b}$.

For any $k$-form field ω and any vector field $\mathsf{v}$ the exterior derivative commutes with the Lie derivative with respect to the vector field:

$$\begin{equation}
\mathcal{L}_{\mathsf{v}}(\mathsf{d}\omega) = \mathsf{d}(\mathcal{L}_{\mathsf{v}}\omega).
\end{equation}$$

If ω is an element of surface then $\mathsf{d}\omega$ is an element of volume. The Lie derivative computes the rate of change of its argument under a deformation described by the vector field. The answer is the same whether we deform the surface before computing the volume or compute the volume and then deform it.

We can verify this in 3-dimensional rectangular space for a general one-form field:#Footnote(4)
```Scheme
(((- ((Lie-derivative V) (d theta))
(d ((Lie-derivative V) theta)))
X Y)
R3-rect-point)
0
```
and for the general two-form field:
```Scheme
(((- ((Lie-derivative V) (d omega))
(d ((Lie-derivative V) omega)))
X Y Z)
R3-rect-point)
0
```
The Lie derivative satisfies another nice elementary relationship. If $\mathsf{v}$ and $\mathsf{w}$ are two vector fields, then

$$\begin{equation}
[\mathcal{L}_{\mathsf{v}},\mathcal{L}_{\mathsf{w}}] = \mathcal{L}_{[\mathsf{v},\mathsf{w}]}.
\end{equation}$$

Again, for our general one-form field θ:

```Scheme
((((- (commutator (Lie-derivative X) (Lie-derivative Y))
(Lie-derivative (commutator X Y)))
theta)
Z)
R3-rect-point)
0
```
and for the two-form field ω:

```Scheme
((((- (commutator (Lie-derivative X) (Lie-derivative Y))
(Lie-derivative (commutator X Y)))
omega)
Z V)
R3-rect-point)
0
```
### Exponentiating Lie Derivatives

The Lie derivative computes the rate of change of objects as they are advanced along integral curves. The Lie derivative of an object produces another object of the same type, so we can iterate Lie derivatives. This gives us Taylor series for objects along the curve.

The operator $e^{t\mathcal{L}_{\mathsf{v}}} = 1+t\mathcal{L}_{v}+\tfrac{t^{2}}{2!}\mathcal{L}^{2}_{\mathsf{v}}+\ldots$ evolves objects along the curve by parameter $t$. For example, the exponential of a Lie derivative applied to a vector field is

$$\begin{equation}
\begin{aligned}
e^{t\mathcal{L}_{\mathsf{v}}}\mathsf{y} = \mathsf{y}+t\mathcal{L}_{\mathsf{v}}\mathsf{y}+\frac{t^{2}}{2}{\mathcal{L}_{\mathsf{v}}}^{2}\mathsf{y}+\cdots \\
&= \mathsf{y}+t[\mathsf{v},\mathsf{y}]+\frac{t^{2}}{2}[\mathsf{v},[\mathsf{v},\mathsf{y}]]+\cdots .
\end{aligned}
\end{equation}$$

Consider a simple case. We advanced the coordinate-basis vector field ${\partial}/{\partial\mathsf{y}}$ by an angle $a$ around the circle. Let $\mathsf{J}_{z} = {x\partial}/{\partial\mathsf{y}} - {y\partial}/{\partial\mathsf{x}}$, the circular vector field. We recall
```Scheme
(define Jz (- (* x d/dy) (* y d/dx)))
```
We can apply the exponential of the Lie derivative with respect to $\mathsf{J}_{z}$ to ${\partial}/{\partial\mathsf{y}}$. We examine how the result affects a general function on the manifold:
```Scheme
(series:for-each print-expression
((((exp (* 'a (Lie-derivative Jz))) d/dy)
(literal-manifold-function 'f-rect R3-rect))
((point R3-rect) (up 1 0 0)))
5)
/(((partial 0) f-rect) (up 1 0))/
/(* -1 a (((partial 1) f-rect) (up 1 0)))/
/(* -1/2 (expt a 2) (((partial 0) f-rect) (up 1 0)))/
/(* 1/6 (expt a 3) (((partial 1) f-rect) (up 1 0)))/
/(* 1/24 (expt a 4) (((partial 0) f-rect) (up 1 0)))/
/;Value: .../
```
Apparently the result is
```Scheme

$$\begin{equation}
\text{exp}(\alpha\mathcal{L}_{(\mathsf{x}\,{\partial}/{\partial\mathsf{y}}-\mathsf{y}\,{\partial}/{\partial\mathsf{x}})})\frac{\partial}{\partial\mathsf{y}}
=-\sin(a)\frac{\partial}{\partial\mathsf{x}}+\cos(a)\frac{\partial}{\partial\mathsf{y}}.
\end{equation}$$

```
### Interior Product

There is a simple but useful operation available between vector fields and form fields called /interior product/. This is the substitution of a vector field $\mathsf{v}$ into the first argument of a $p$-form field ω to produce a $p-1$-form field:

$$\begin{equation}
(i_{\mathsf{v}}\omega)(\mathsf{v}_{1},\ldots\mathsf{v}_{\mathsf{p}-1})=\omega(\mathsf{v},\mathsf{v}_{1},\ldots\mathsf{v}_{\mathsf{p-1}}).
\end{equation}$$

There is a mundane identity corresponding to the product rule for the Lie derivative of an interior product:

$$\begin{equation}
\mathcal{L}_{\mathsf{v}}(i_{\mathsf{y}}\omega)=i_{\mathcal{L}_{\mathsf{v}}\mathsf{y}}\omega+{i}_{\mathsf{y}}(\mathcal{L}_{\mathsf{v}}\omega).
\end{equation}$$

And there is a rather nice identity for the Lie derivative in terms of the interior product and the exterior derivative, called /Cartan's formula/:

$$\begin{equation}
\mathcal{L}_{\mathsf{v}}\omega=i_{\mathsf{v}}(\mathsf{d}\omega)+\mathsf{d}(i_{\mathsf{v}}\omega).
\end{equation}$$

We can verify Cartan's formula in a simple case with a program:
```Scheme
(define X (literal-vector-field 'X-rect R3-rect))
(define Y (literal-vector-field 'Y-rect R3-rect))
(define Z (literal-vector-field 'Z-rect R3-rect))

(define a (literal-manifold-function 'alpha R3-rect))
(define b (literal-manifold-function 'beta R3-rect))
(define c (literal-manifold function 'gamma R3-rect))

(define omega
(+ (* a (wedge dx dy))
(* b (wedge dy dz))
(* c (wedge dz dx))))

(define ((L1 X) omega)
(+ ((interior-product X) (d omega))
(d ((interior-product X) omega))))

((- (((Lie-derivative X) omega) Y Z)
(((L1 X) omega) Y Z))
((point R3-rect) (up 'x0 'y0 'z0)))
0
```
Note that $i_{\mathsf{v}}\circ{i}_{\mathsf{u}} + {i}_{\mathsf{u}}\circ{i}_{\mathsf{v}} = 0$. One consequence of this is that ${i}_{\mathsf{v}}\circ{i}_{\mathsf{v}}=0$.

### Covariant Derivative

The covariant derivative is another kind of directional derivative operator. We write the covariant derivative operator with respect to a vector field $\mathsf{v}$ as $\nabla_{\mathsf{v}}$. This is pronounced "covariant derivative with respect to $\mathsf{v}$" or "nabla $\mathsf{v}$."

### Covariant Derivative of Vector Fields

We may also choose our $F^{\mathsf{v}}_{\delta}\mathsf{u}$ to define what we mean by "parallel" transport of the vector field $\mathsf{u}$ along an integral curve of the vector field $\mathsf{v}$. This may correspond to our usual understanding of parallel in situations where we have intuitive insight.

The notion of parallel transport is path dependent. Remember our example from the Introduction, page 1: Start at the North Pole carrying a stick along a line of longitude to the Equator, always pointing it south, parallel to the surface of the Earth. Then proceed eastward for some distance, still pointing the stick south. Finally, return to the North Pole along this new line of longitude, keeping the stick pointing south all the time. At the pole the stick will not point in the same direction as it did at the beginning of the trip, and the discrepancy will depend on the amount of eastward motion.#Footnote(5)

So if we try to carry a stick parallel to itself and tangent to the sphere, around a closed path, the stick generally does not end up pointing in the same direction as it started. The result of carrying the stick from one point on the sphere to another depends on the path taken. However, the direction of the stick at the endpoint of a path does not depend on the rate of transport, just on the particular path on which it is carried. Parallel transport over a zero-length path is the identity.

A vector may be resolved as a linear combination of other vectors. If we parallel-transport each component, and form the same linear combination, we get the transported original vector. Thus parallel transport on a particular path for a particular distance is a linear operation.

So the transport function $F^{\mathsf{v}}_{\delta}$ is a linear operator on the components of its argument, and thus:

$$\begin{equation}
F^{\mathsf{v}}_{\delta}\mathsf{u}(\mathsf{f})(\mathsf{m})=\sum_{i,j}(A^{i}_{j}(\delta)(\mathsf{u}^{j}\circ\phi^{\mathsf{v}}_{-\delta})\mathsf{e}_{i}(\mathsf{f}))(\mathsf{m})
\end{equation}$$

for some functions $A^{i}_{j}$ that depend on the particular path (hence its tangent vector $\mathsf{v}$) and the initial point. We reach back along the integral curve to pick up the components of $\mathsf{u}$ and then parallel-transport them forward by the matrix $A^{i}_{j}(\delta)$ to form the components of the parallel-transported vector at the advanced point.

As before, we compute

$$\begin{equation}
\nabla_{\mathsf{v}}\mathsf{u}(\mathsf{f})(\mathsf{m})=Dg(0),
\end{equation}$$

where

$$\begin{equation}
g(\delta)=\mathsf{u}(\mathsf{f})(\mathsf{m})-(F^{\mathsf{v}}_{\delta}\mathsf{u})(\mathsf{f})(\mathsf{m}).
\end{equation}$$

Expanding with respect to a basis $\set{\mathsf{e}_{i}}$ we get

$$\begin{equation}
g(\delta)=\sum_{i}\Bigg(\mathsf{u}^{i}\mathsf{e}_{i}(\mathsf{f})-\sum_{j}A^{i}_{j}(\delta)(\mathsf{u}^{j}\circ\phi^{\mathsf{v}}_{-\delta})\mathsf{e}_{i}(\mathsf{f})\Bigg)(\mathsf{m}).
\end{equation}$$

By the product rule for derivatives,

$$\begin{equation}
\begin{aligned}
Dg(\delta)= \\
\sum_{ij}\big(A^{i}_{j}(\delta)((\mathsf{v}(\mathsf{u}^{j}))\circ\phi^{\mathsf{v}}_{-\delta})\mathsf{e}_{i}(\mathsf{f})-DA^{i}_{j}(\delta)(\mathsf{u}^{j}\circ\phi^{\mathsf{v}}_{-\delta})\mathsf{e}_{i}(\mathsf{f})\big)(\mathsf{m}).
\end{aligned}
\end{equation}$$

So, since $A^{i}_{j}(0)(\mathsf{m})$ is the identity multiplier,
and $\phi^{\mathsf{v}}_{0}$ is the identity function,

$$\begin{equation}
Dg(0)=\sum_{i}\Bigg(\mathsf{v}(\mathsf{u}^{i})(\mathsf{m})\mathsf{e}_{i}(\mathsf{f})-\sum_{j}DA^{i}_{j}(0)\mathsf{u}^{j}(\mathsf{m})\mathsf{e}_{i}(\mathsf{f})\Bigg)\,(\mathsf{m}).
\end{equation}$$

We need $DA^{i}_{j}(0)$. Parallel transport depends on the path, but not on the parameterization of the path. From this we can deduce that $DA^{i}_{j}(0)$ can be written as one-form fields applied to the vector field $\mathsf{v}$, as follows.

Introduce $B$ to make the dependence of $A$s on $\mathsf{v}$
explicit:

$$\begin{equation}
A^{i}_{j}(\delta) = B^{i}_{j}(\mathsf{v})(\delta).
\end{equation}$$

Parallel transport depends on the path but not on the rate along the path. Incrementally, if we scale the vector field $\mathsf{v}$ by $ξ$,

$$\begin{equation}
\frac{d}{d\delta}(B(\mathsf{v})(\delta)) = \frac{d}{d\delta}(B(\xi\mathsf{v})({\delta}/{\xi})).
\end{equation}$$

Using the chain rule

$$\begin{equation}
D(B(\mathsf{v}))(\delta) = \frac{1}{\xi}D(B(\xi\mathsf{v}))(\frac{\delta}{\xi}),
\end{equation}$$

so, for $\delta = 0$,

$$\begin{equation}
\xi{D}(B(\mathsf{v}))(0) = D(B(\xi\mathsf{v}))(0).
\end{equation}$$

The scale factor ξ can vary from place to place. So $DA^{i}_{j}(0)$ is homogeneous in $\mathsf{v}$ over manifold functions. This is stronger than the homogeneity required by equation (7.7).

The superposition property (equation (7.6)) is true of the ordinary directional derivative of manifold functions. By analogy we require it to be true of directional derivatives of vector fields.

These two properties imply that $DA^{i}_{j}(0)$ is a one-form field:

$$\begin{equation}
DA^{i}_{j}(0) = -\varpi^{i}_{j}(\mathsf{v}),
\end{equation}$$

where the minus sign is a matter of convention.

As before, we can take a stab at computing the covariant derivative of a vector field by supplying an appropriate transport operator for F in F->directional-derivative. Again, this is expanded to a given order with a given coordinate system. These will be unnecessary in the final version.
```Scheme
(define (covariant-derivative-vector omega coordsys order)
(let ((Phi (phi coordsys order)))
(F->directional-derivative
(F-parallel omega Phi coordsys))))

(define ((((((F-parallel omega phi coordsys) v) delta) u) f) m)
(let ((basis (coordinate-system->basis coordsys)))
(let ((etilde (basis->1form-basis basis))
(e (basis->vector-basis basis)))
(let ((m0 (((phi v) (- delta)) m)))
(let ((Aij (+ (identity-like ((omega v) m0))
(* delta (- ((omega v) m0)))))
(ui ((etilde u) m0)))
(* ((e f) m) (* Aij ui)))))))
```
So

$$\begin{equation}
Dg(0) = \sum_{i}\left(\mathsf{v}(\mathsf{u}^{i})(\mathsf{m})+\sum_{j}\varpi^{i}_{j}(\mathsf{v})(\mathsf{m})\mathsf{u}^{j}(\mathsf{m})\right)\mathsf{e}_{i}(\mathsf{f})(\mathsf{m}).
\end{equation}$$

Thus the covariant derivative is

$$\begin{equation}
\nabla_{\mathsf{v}}\mathsf{u}(\mathsf{f}) = \sum_{i}\left(\mathsf{v}(\mathsf{u}^{i})+\sum_{j}\varpi^{i}_{j}(\mathsf{v})\mathsf{u}^{j}\right)\mathsf{e}_{i}(\mathsf{f}).
\end{equation}$$

The one-form fields $\varpi^{i}_{j}$ are called the /Cartan one-forms/, or the /connection one-forms/. They are defined with respect to the basis $\mathsf{e}$.

As a program, the covariant derivative is:#Footnote(6)
```Scheme
(define ((((covariant-derivative-vector Cartan) V) U) f)
(let ((basis (Cartan->basis Cartan))
(Cartan-forms (Cartan->forms Cartan)))
(let ((vector-basis (basis->vector-basis basis))
(1form-basis (basis->1-form-basis basis)))
(let ((u-components (1form-basis U)))
(* (vector-basis f)
(+ (V u-components)
(* (Cartan-forms V) u-components)))))))
```
An important property of $\nabla_{\mathsf{v}}\mathsf{u}$ is that it is linear over manifold functions $\mathsf{g}$ in the first argument

$$\begin{equation}
\nabla_{\mathsf{g}\mathsf{v}}\mathsf{u}(\mathsf{f}) = \mathsf{g}\nabla_{\mathsf{v}}\mathsf{u}(\mathsf{f}),
\end{equation}$$

consistent with the fact that the Cartan forms $\varpi^{i}_{j}$
share the same property.

Additionally, we can extend the product rule, for any manifold function $\mathsf{g}$ and any vector field $\mathsf{u}$:

$$\begin{equation}
\begin{aligned}
\nabla_{\mathsf{v}}(\mathsf{g}\mathsf{u})(\mathsf{f}) &= \sum_{i}\left(\mathsf{v}(\mathsf{gu}^{i})+\sum_{j}\varpi^{i}_{j}(\mathsf{v})\mathsf{gu}^{j}\right)\mathsf{e}_{i}(\mathsf{f}) \\
&= \sum_{i}\mathsf{v}(\mathsf{g})\mathsf{u}^{i}\mathsf{e}_{i}(\mathsf{f})+\mathsf{g}\nabla_{\mathsf{v}}(\mathsf{u})(\mathsf{f}) \\
&= (\nabla_{\mathsf{v}}\mathsf{g})\mathsf{u}(\mathsf{f})+\mathsf{g}\nabla_{\mathsf{v}}(\mathsf{u})(\mathsf{f}).
\end{aligned}
\end{equation}$$

### An Alternate View

As we did with the Lie derivative (equations 7.18-7.21), we can write the vector field

$$\begin{equation}
\mathsf{u}(\mathsf{f})(\mathsf{m}) = \sum_{i}\mathsf{u}^{i}(\mathsf{m})\mathsf{e}_{i}(\mathsf{f})(\mathsf{m}).
\end{equation}$$

By the extended product rule, equation (7.51), we get:

$$\begin{equation}
\nabla_{\mathsf{v}}\mathsf{u}(\mathsf{f}) = \sum_{i}(\mathsf{v}(\mathsf{u}^{i})\mathsf{e}_{i}(\mathsf{f})+\mathsf{u}^{i}\nabla_{\mathsf{v}}\mathsf{e}_{i}(\mathsf{f})).
\end{equation}$$

Because the covariant derivative of a vector field is a vector field we can extract the components of $\nabla_{\mathsf{v}}\mathsf{e}_{i}$ using the dual basis:

$$\begin{equation}
\varpi^{i}_{j}(\mathsf{v}) = \tilde{\mathsf{e}}^{i}(\nabla_{\mathsf{v}}\mathsf{e}_{j}).
\end{equation}$$

This gives an alternate expression for the Cartan one forms. So

$$\begin{equation}
\nabla_{\mathsf{v}}\mathsf{u}(\mathsf{f}) = \sum_{i}\left(\mathsf{v}(\mathsf{u}^{i})+\sum_{j}\varpi^{i}_{j}(\mathsf{v})\mathsf{u}^{j}\right)\mathsf{e}_{i}(\mathsf{f}).
\end{equation}$$

This analysis is parallel to the analysis of the Lie derivative,
except that here we have the Cartan form fields $\varpi^{i}_{j}$
and there we had $\Delta^{i}_{j}$, which are not form fields.

Notice that the Cartan forms appear here (equation 7.53) in terms of the covariant derivatives of the basis vectors. By contrast, in the first derivation (see equation 7.42) the Cartan forms appear as the derivatives of the linear forms that accomplish the parallel transport of the coefficients.

The Cartan forms can be constructed from the dual basis one-forms:

$$\begin{equation}
\varpi^{i}_{j}(\mathsf{v})(\mathsf{m}) = \sum_{k}\Gamma^{i}_{jk}(\mathsf{m})\tilde{\mathsf{e}}^{k}(\mathsf{v})(\mathsf{m}).
\end{equation}$$

The connection coefficient functions $\Gamma^{i}_{jk}$ are called the /Christoffel coefficients/ (traditionally called /Christoffel symbols/).#Footnote(7) Making use of the structures,#Footnote(8), the Cartan forms are

$$\begin{equation}
\varpi(\mathsf{v}) = \Gamma\tilde{\mathsf{e}}(\mathsf{v}).
\end{equation}$$

Conversely, the Christoffel coefficients may be obtained from the Cartan forms

$$\begin{equation}
\Gamma^{i}_{jk} = \varpi^{i}_{j}(\mathsf{e}_{k}).
\end{equation}$$

### Covariant Derivative of One-Form Fields

The covariant derivative of a vector field induces a compatible covariant derivative for a one-form field. Because the application of a one-form field to a vector field yields a manifold function, we can evaluate the covariant derivative of such an application. Let τ be a one-form field and $\mathsf{w}$ be a vector field. Then

$$\begin{equation}
\begin{aligned}
\nabla_{\mathsf{v}}(\tau(\mathsf{w})) &= \mathsf{v}\left(\sum_{j}\tau_{j}\mathsf{w}^{j}\right) \\
&= \sum_{j}(\mathsf{v}(\tau_{j})\mathsf{w}^{j}+\tau_{j}\mathsf{v}(\mathsf{w}^{j})) \\
&= \sum_{j}\left(\mathsf{v}(\tau_{j})\mathsf{w}^{j}+\tau_{j}\left(\tilde{\mathsf{e}}^{j}(\nabla_{\mathsf{v}}\mathsf{w})-\sum_{k}\varpi^{j}_{k}(\mathsf{v})\mathsf{w}^{k}\right)\right)\\
&= \sum_{j} \left(\mathsf{v}(\tau_{j})\mathsf{w}^{j}-\tau_{j}\sum_{k}\varpi^{j}_{k}(\mathsf{v})\mathsf{w}^{k}\right)+\tau(\nabla_{\mathsf{v}}\mathsf{w}) \\
&= \sum_{j}\left(\mathsf{v}(\tau_{j})\tilde{\mathsf{e}}^{j}-\tau_{j}\sum_{k}\varpi^{j}_{k}(\mathsf{v})\tilde{\mathsf{e}}^{k}\right)(\mathsf{w})+\tau(\nabla_{\mathsf{v}}\mathsf{w}).
\end{aligned}
\end{equation}$$

So if we define the covariant derivative of a one-form field to be

$$\begin{equation}
\nabla_{\mathsf{v}}(\tau) = \sum_{k}\left(\mathsf{v}(\tau_{k})-\sum_{j}\tau_{j}\varpi^{j}_{k}(\mathsf{v})\right)\tilde{\mathsf{e}}^{k},
\end{equation}$$

then the generalized product rule holds:

$$\begin{equation}
\nabla_{\mathsf{v}}(\tau(\mathsf{u})) = (\nabla_{\mathsf{v}}\tau)(\mathsf{u})+\tau(\nabla_{\mathsf{v}}\mathsf{u}).
\end{equation}$$

Alternatively, assuming the generalized product rule forces the definition of covariant derivative of a one-form field.

As a program this is

```Scheme
(define ((((covariant-derivative-1form Cartan) V) tau) U)
(let ((nabla_V ((covariant-derivative-vector Cartan) V)))
(- (V (tau U)) (tau (nabla_V U)))))
```
This program extends naturally to higher-rank form fields:

```Scheme
(define ((((covariant-derivative-form Cartan) V) tau) vs)
(let ((k (get-rank tau))
(nabla_V ((covariant-derivative-vector Cartan) V)))
(- (V (apply tau vs))
(sigma (lambda (i)
(apply tau
(list-with-substituted-coord vs i
(nabla_V (list-ref vs i)))))
0 (- k 1)))))
```
### Change of Basis

The basis-independence of the covariant derivative implies a relationship between the Cartan forms in one basis and the equivalent Cartan forms in another basis. Recall (equation 4.13) that the basis vector fields of two bases are always related by a linear transformation. Let $\mathsf{J}$ be the matrix of coefficient functions and let $\mathsf{e}$ and $\mathsf{e}^{\prime}$ be down tuples of basis vector fields. then

$$\begin{equation}
\mathsf{e}(\mathsf{f}) = \mathsf{e}^{\prime}(\mathsf{f})\mathsf{J}.
\end{equation}$$

We want the covariant derivative to be independent of basis. This will determine how the connection transforms with a change of basis:

$$\begin{equation}
\begin{aligned}
\nabla_{\mathsf{v}}\mathsf{u}(\mathsf{f}) &= \sum_{i}\mathsf{e}_{i}(\mathsf{f})\left(\mathsf{v}(\mathsf{u}^{i})+\sum_{j}\varpi^{i}_{j}(\mathsf{v})\mathrm{u}^{j}\right) \\
&= \sum_{ijk}\mathsf{e}^{\prime}_{i}(\mathsf{f})\mathsf{J}^{i}_{j}\left(\mathsf{v}\left((\mathsf{J}^{-1})^{j}_{k}(\mathsf{u}^{\prime})^{k}\right)+\sum_{l}\varpi^{j}_{k}(\mathsf{v})(\mathsf{J}^{-1})^{k}_{l}(\mathsf{u}^{\prime})^{l}\right) \\
&= \sum_{i}\mathsf{e}^{\prime}_{i}(\mathsf{f})\left(\mathsf{v}((\mathsf{u}^{\prime})^{i})+\sum_{jk}\mathsf{J}^{i}_{j}\mathsf{v}\left((\mathsf{J}^{-1})^{j}_{k}\right)(\mathsf{u}^{\prime})^{k}\right. \\
&+ \left( \sum_{jkl}\mathsf{J}^{i}_{j}\varpi^{j}_{k}(\mathsf{v})(\mathsf{J}^{-1})^{k}_{l}(\mathsf{u}^{\prime})^{l}\right) \\
&= \sum_{i}\mathsf{e}^{\prime}_{i}(\mathsf{f})\left(\mathsf{v}((\mathsf{u}^{\prime})^{i})+\sum_{j}(\varpi^{\prime})^{i}_{j}(\mathsf{v})(\mathsf{u}^{\prime})^{j}\right).
\end{aligned}
\end{equation}$$

The last line of equation (7.62) gives the formula for the covariant derivative we would have written down naturally in the primed coordinates; comparing with the next-to-last line, we see that

$$\begin{equation}
\varpi^{\prime}(\mathsf{v}) = \mathsf{Jv}(\mathsf{J}^{-1})+\mathsf{J}\varpi(\mathsf{v})\mathsf{J}^{-1}.
\end{equation}$$

This transformation rule is weird. It is not a linear transformation of $\varpi$ because the first term is an offset that depends on $\mathsf{v}$. So it is not required that
$\varpi^{\prime}=0$ when $\varpi=0$. Thus $\varpi$ is not a tensor field. See Appendix C.

We can write equation (7.61) in terms of components

$$\begin{equation}
\mathsf{e}_{i}(\mathsf{f}) = \sum_{j}\mathsf{e}^{\prime}_{j}(\mathsf{f})\mathsf{J}^{j}_{i}.
\end{equation}$$

Let $\mathsf{K}=\mathsf{J}^{-1}$, so $\sum_{j}\mathsf{K}^{i}_{j}(\mathsf{m})\mathsf{J}^{j}_{k}(\mathsf{m}) = \delta^{i}_{k}$. Then

$$\begin{equation}
{\varpi^{\prime}}^{i}_{l}(\mathsf{v}) = \sum_{j}\mathsf{J}^{i}_{j}\mathsf{v}(\mathsf{K}^{j}_{l})+\sum_{jk}\mathsf{J}^{i}_{j}\varpi^{j}_{k}(\mathsf{v})\mathsf{K}^{k}_{l}.
\end{equation}$$

The transformation rule for $\varpi$ is implemented in the following program:
```Scheme
(define (Cartan-transform Cartan basis-prime)
(let ((basis (Cartan->basis Cartan))
(forms (Cartan->forms Cartan))
(prime-dual-basis (basis->1form-basis basis-prime))
(prime-vector-basis (basis->vector-basis basis-prime)))
(let ((vector-basis (basis->vector-basis basis))
(1form-basis (basis->1form-basis basis)))
(let ((J-inv (s:map/r 1form-basis prime-vector-basis))
(J (s:map/r prime-dual-basis vector-basis)))
(let ((omega-prime-forms
(procedure->1form-field
(lambda (v)
(+ (* J (v J-inv))
(* J (* (forms v) J-inv)))))))
(make-Cartan omega-prime-forms basis-prime))))))
```
The s:map/r procedure constructs a tuple of the same shape as its second argument whose elements are the result of applying the first argument to the corresponding elements of the second argument.

We can illustrate that the covariant derivative is independent of the coordinate system in a simple case, using rectangular and polar coordinates in the plane.#Footnote(9) We can choose Christoffel coefficients for rectangular coordinates that are all zero:#Footnote(10)

```Scheme
(define R2-rect-Christoffel
(make-Christoffel
(let ((zero (lambda (m) 0)))
(down (down (up zero zero)
(up zero zero))
(down (up zero zero)
(up zero zero))))
R2-rect-basis))
```
With these Christoffel coefficients, parallel transport preserves the components relative to the rectangular basis. This corresponds to our usual notion of parallel in the plane. We will see later in Chapter 9 that these Christoffel coefficients are a natural choice for the plane. From these we obtain the Cartan form:#Footnote(11)

```Scheme
(define R2-rect-Cartan
(Christoffel->Cartan R2-rect-Christoffel))
```
And from equation (7.63) we can get the corresponding Cartan form for polar coordinates:

```Scheme
(define R2-polar-Cartan
(Cartan-transform R2-rect-Cartan R2-polar-basis))
```
The vector field ${\partial}/{\partial\theta}$ generates a rotation in the plane (the same as circular). The covariant derivative with respect to ${\partial}/{\partial\mathsf{x}}$ of ${\partial}/{\partial\theta}$ applied to an arbitrary manifold function is:
```Scheme
(define circular (- (* x d/dy) (* y d/x)))

(define f (literal-manifold-function 'f-rect R2-rect))
(define R2-rect-point ((point R2-rect) (up 'x0 'y0)))

(((((covariant-derivative R2-rect-Cartan) d/dx)
circular)
f)
R2-rect-point)
/(((partial 1) f-rect) (up x0 y0))/
```
Note that this is the same thing as ${\partial}/{\partial\mathsf{y}}$ applied to the function:
```Scheme
((d/dy f) R2-rect-point)
/(((partial 1) f-rect) (up x0 y0))/
```
In rectangular coordinates, where the Christoffel coefficients are zero, the covariant derivative $\nabla_{\mathsf{u}}\mathsf{v}$ is the vector whose coefficients are obtained by applying $\mathsf{u}$ to the coefficients of $\mathsf{v}$. Here, only one coefficient of ${\partial}/{\partial\theta}$ depends on $x$, the coefficient of ${\partial}/{\partial\mathsf{y}}$, and it depends linearly on $x$. So $\nabla_{{\partial}/{\partial\mathsf{x}}} {\partial}/{\partial\theta} = {\partial}/{\partial\mathsf{y}}$. (See figure 7.1.)

Note that we get the same answer if we use polar coordinates to compute the covariant derivative:
```Scheme
(((((covariant-derivative R2-polar-Cartan) d/dx) J) f)
R2-rect-point)
/(((partial 1) f-rect) (up x0 y0))/
```
In rectangular coordinates the Christoffel coefficients are all zero; in polar coordinates there are nonzero coefficients, but the value of the covariant derivative is the same. In polar coordinates the basis elements vary with position, and the Christoffel coefficients compensate for this.

Of course, this is a pretty special situation. Let's try something more general:
```Scheme
(define V (literal-vector-field 'V-rect R2-rect))
(define W (literal-vector-field 'W-rect R2-rect))

(((((- (covariant-derivative R2-rect-Cartan)
(covariant-derivative R2-polar-Cartan))
V)
W)
f)
R2-rect-point)
0
```
### Parallel Transport

We have defined parallel transport of a vector field along integral curves of another vector field. But not all paths are integral curves of a vector field. For example, paths that cross themselves are not integral curves of any vector field.

Here we extend the idea of a parallel transport of a stick to make sense for arbitrary paths on the manifold. Any path can be written as a map γ from the real-line manifold to the manifold $\mathsf{M}$. We construct a vector field over the map $\mathsf{u}_{\gamma}$ by parallel-transporting the stick to all points on the path γ.

For any path γ there are locally directional derivatives of functions on $\mathsf{M}$ defined by tangent vectors to the curve. The vector over the map $\mathsf{w}_{\gamma}=d\gamma({\partial}/{\partial\mathsf{t}})$ is a directional derivative of functions on the manifold $M$ along the path γ.

Our goal is to determine the equations satisfied by the vector field over the map $\mathsf{u}_{\gamma}$. Consider the parallel-transport $F^{\mathsf{w}_{\gamma}}_{\delta}\mathsf{u}_{\gamma}$.#Footnote(12) So a vector field $\mathsf{u}_{\gamma}$ is parallel-transported to itself if and only if $\mathsf{u}_{\gamma} = F^{\mathsf{w}_{\gamma}}_{\delta}\mathsf{u}_{\gamma}$. Restricted to a path, the equation analogous to equation (7.40) is

$$\begin{equation}
g(\delta)=\sum_{i}\left(u^{i}(t)-\sum_{j}A^{i}_{j}(\delta)u^{j}(t-\delta)\right)\mathsf{e}^{\gamma}_{i}(\mathsf{f})(\mathsf{t}),
\end{equation}$$

where the coefficient function $u^{i}$ is now a function on the real-line parameter manifold and where we have rewritten the basis as a basis over the map γ.#Footnote(13) Here $g(\delta)=0$ if $\mathsf{u}_{\gamma}$ is parallel-transported into itself.

Taking the derivative and setting $\delta=0$ we find

$$\begin{equation}
0=\sum_{i}\left(Du^{i}(t)+{\sum_{j}}^{\gamma}\varpi^{i}_{j}(\mathsf{w}_{\gamma})(t)u^{j}(t)\right)\mathsf{e}^{\gamma}_{i}(\mathsf{f})(\mathsf{t}).
\end{equation}$$

But this implies that

$$\begin{equation}
0=Du^{i}(t)+{\sum_{j}}^{\gamma}\varpi^{i}_{j}(\mathsf{w}_{\gamma})(\mathsf{t})u^{j}(t),
\end{equation}$$

an ordinary differential equation in the coefficients of
$\mathsf{u}_{\gamma}$.

We can abstract these equations of parallel transport by inventing a covariant derivative over a map. We also generalize the time line to a source manifold $\mathsf{N}$.

$$\begin{equation}
\nabla^{\gamma}_{\mathsf{v}}\mathsf{u}_{\gamma}(\mathsf{f})(\mathsf{n})=\sum_{i}\left(\mathsf{v}(u^{i})(\mathsf{n})+{\sum_{j}}^{\gamma}\varpi^{i}_{j}(d\gamma(\mathsf{v}))(\mathsf{n})u^{j}(\mathsf{n})\right)\mathsf{e}^{\gamma}_{i}(\mathsf{f})(\mathsf{n}),
\end{equation}$$

where the map $\gamma:\mathsf{N}\to\mathsf{M},\mathsf{v}$ is a vector on $\mathsf{N}$, $\mathsf{u}_{\gamma}$ is a vector over the map γ, $\mathsf{f}$ is a function on $\mathsf{M}$, and $\mathsf{n}$ is a point in $\mathsf{N}$. Indeed, if $\mathsf{w}$ is a vector field on $\mathsf{M}$, $\mathsf{f}$ is a manifold function on $\mathsf{M}$, and if $d\gamma(\mathsf{v})=\mathsf{w}_{\gamma}$ then

$$\begin{equation}
\nabla^{\gamma}_{\mathsf{v}}\mathsf{u}_{\gamma}(\mathsf{f})(\mathsf{n})=\nabla_{\mathsf{w}}\mathsf{u}(\mathsf{f})(\gamma(\mathsf{n})).
\end{equation}$$

This is why we are justified in calling $\nabla^{\gamma}_{\mathsf{v}}$ a covariant derivative.

Respecializing the source manifold to the real line, we can write the equations governing the parallel transport of $\mathsf{u}_{\gamma}$ as

$$\begin{equation}
\nabla^{\gamma}_{{\partial}/{\partial\mathsf{t}}}\mathsf{u}_{\gamma}=0.
\end{equation}$$

We obtain the set of differential equations (7.68) for the coordinates of $\mathsf{u}_{\gamma}$, the vector over the map γ,
that is parallel-transported along the curve γ:

$$\begin{equation}
Du^{i}(t)+{\sum_{j}}^{\gamma}\varpi^{i}_{j}(d\gamma({\partial}/{\partial t}))(\mathsf{t})u^{j}(t)=0.
\end{equation}$$

Expressing the Cartan forms in terms of the Christoffel coefficients we obtain

$$\begin{equation}
Du^{i}(t)+\sum_{j,k}\Gamma^{i}_{jk}(\gamma(\mathsf{t}))D\sigma^{k}(t)u^{j}(t)=0
\end{equation}$$

where $\sigma=\chi_{\mathsf{M}}\circ\gamma\circ\chi^{-1}_{\mathsf{R}}$ are the coordinates of the path ($\chi_{\mathsf{M}}$ and $\chi_{\mathsf{R}}$ are the coordinate functions for $\mathsf{M}$ and the real line).

### On a Sphere

Let's figure out what the equations of parallel transport of $\mathsf{u}_{\gamma}$, an arbitrary vector over the map γ, along an arbitrary path γ on a sphere are. We start by constructing the necessary manifold.
```Scheme
(define sphere (make-manifold S^2 2 3))
(define S2-spherical
(coordinate-system-at 'spherical 'north-pole sphere))
(define S2-basis
(coordinate-system->basis S2-spherical))
```
We need the path γ, which we represent as a map from the real line to $\mathsf{M}$, and $\mathsf{w}$, the parallel-transported vector over the map:

```Scheme
(define gamma
(compose (point S2-spherical)
(up (literal-function 'alpha)
(literal-function 'beta))
(chart R1-rect)))
```
where alpha is the colatitude and beta is the longitude.

We also need an arbitrary vector field u_gamma over the map gamma. To make this we multiply the structure of literal component functions by the vector basis structure.

```Scheme
(define basis-over-gamma
(basis->basis-over-map gamma S2-basis))

(define u_gamma
(* (up (compose (literal-function 'u^0)
(chart R1-rect))
(compose (literal-function 'u^1)
(chart R1-rect)))
(basis->vector-basis basis-over-gamma)))
```
We specify a connection by giving the Christoffel coefficients.#Footnote(14)
```Scheme
(define S2-Christoffel
(make-Christoffel
(let ((zero (lambda (point) 0)))
(down (down (up zero zero)
(up zero (/ 1 (tan theta))))
(down (up zero (/1 (tan theta)))
(up (-  (* (sin theta) (cos theta))) zero))))
S2-basis))

(define sphere-Cartan (Christoffel->Cartan S2-Christoffel))
```

Finally, we compute the residual of the equation (7.71) that governs parallel transport for this situation:#Footnote(15)
```Scheme
(define-coordinates t R1-rect)

(s:map/r
(lambda (omega)
((omega
(((covariant-derivative sphere-Cartan gamma)
d/dt)
u_gamma))
((point R1-rect) 'tau)))
(basis->1form-basis basis-over-gamma))
/(up + (* -1/
/(sin (alpha tau))/
/(cos (alpha tau))/
/((D beta) tau)/
/(u^1 tau))/
/((D u^0) tau))/
/(/ (+ (* (u^0 tau) (cos (alpha tau)) ((D beta) tau))/
/(* ((D alpha) tau) (cos (alpha tau)) (u^1 tau))/
/(* ((D u^1) tau) (sin (alpha tau))))/
/(sign (alpha tau))))/
```
Thus the equations governing the evolution of the components of the transported vector are:

$$\begin{equation}
Du^{0}(\tau)=\sin(\alpha(\tau))\cos(\alpha(\tau))D\beta(\tau)u^{1}(\tau),
\end{equation}$$

$$\begin{equation}
Du^{1}(\tau)=-\frac{\cos(\alpha(\tau))}{\sin(\alpha(\tau))}(D\beta(\tau)u^{0}(\tau)+D\alpha(\tau)u^{1}(\tau)).
\end{equation}$$

These equations describe the transport on a sphere, but more generally they look like

$$\begin{equation}
Du(\tau)=f(\sigma(\tau),D\sigma(\tau))u(\tau),
\end{equation}$$

where σ is the tuple of the coordinates of the path on the manifold and $u$ is the tuple of the components of the vector. The equation is linear in $u$ and is driven by the path σ, as in a variational equation.

We now set this up for numerical integration. Let $s(t)=(t,u(t))$ be a state tuple, combining the time and the coordinates of $\mathsf{u}_{\gamma}$ at that time. Then we define $g$:

$$\begin{equation}
g(s(t)) = Ds(t) = (1,Du(t)),
\end{equation}$$

where $Du(t)$ is the tuple of right-hand sides of equation (7.72).

### On a Great Circle

We illustrate parallel transport in a case where we should know the answer: we carry a vector along a great circle of a sphere. Given a path and Cartan forms for the manifold we can produce a state derivative suitable for numerical integration. Such a state derivative takes a state and produces the derivative of the state.
```Scheme
(define (g gamma Cartan)
(let ((omega
((Cartan->forms
(Cartan->Cartan-over-map Cartan gamma))
((differential gamma) d/dt))))
(define ((the-state-derivative) state)
(let ((t ((point R1-rect) (ref state 0)))
(u (ref state 1)))
(up 1 (* -1 (omega t) u))))
the-state-derivative))
```
The path on the sphere will be the target of a map from the real line. We choose one that starts at the origin of longitudes on the equator and follows the great circle that makes a given tilt angle with the equator.
```Scheme
(define ((transform tilt) coords)
(let ((colat (red coords 0))
(long (ref coord 1)))
(let ((x (* (sin colat) (cos long)))
(y (* (sin colat) (sign  long)))
(z (cos colat)))
(let ((vp ((rotate-x tilt) (up x y z))))
(let ((colatp (acos (ref vp 2)))
(longp (atan (ref vp 1) (ref vp 0))))
(up colatp long p))))))

(define (tilted-path tilt)
(define (coords t)
((transform tilt) (up :pi/2 t)))
(compose (point S2-spherical)
coords
(chart R1-rect)))
```
A southward pointing vector, with components (up 1 0), is transformed to an initial vector for the tilted path by multiplying by the derivative of the tilt transform at the initial point. We then parallel transport this vector by numerically integrating the differential equations. In this example we tilt by 1 radian, and we advance for $\pi/2$ radians. In this case we know the answer: by advancing by $\pi/2$ we walk around the circle a quarter of the way and at that point the transported vector points south:
```Scheme
((state-advancer (g (tilted-path 1) sphere-Cartan))
(up 0 (* ((D (transform 1)) (up :pi/2 0)) (up 1 0)))
pi/2)
/up 1.5707963267948957/
/(up .9999999999997626 7.376378522558262e-13))/
```
However, if we transport by 1 radian rather than $\pi/2$, the numbers are not so pleasant, and the transported vector no longer points south:
```Scheme
((state-advancer (g (tilted-path 1) (sphere-Cartan))
(up 0 (* ((D (transform 1)) (up :pi/2 0)) (up 1 0)))
1)
/(up 1. (up .7651502649360408 .9117920272006472))/
```
But the transported vector can be obtained by tilting the original southward-pointing vector after parallel-transporting along the equator:#Footnote(16)
```Scheme
(* ((D (transform 1)) (up :pi/2 1)) (up 1 0))
/(up .7651502649370375 .9117920272004736)/
```
### Geodesic Motion

In geodesic motion the velocity vector is parallel-transported by itself. Recall (equation 6.9) that the velocity is the differential of the vector ${\partial}/{\partial\mathsf{t}}$ over the map γ. The equation of geodesic motion is#Footnote(17)

$$\begin{equation}
\nabla^{\gamma}_{{\partial}/{\partial\mathsf{t}}}d\gamma({\partial}/{\partial\mathsf{t}})=0.
\end{equation}$$

In coordinates, this is

$$\begin{equation}
D^{2}\sigma^{i}(t)+\sum_{jk}\Gamma^{i}_{jk}(\gamma(t))D\sigma^{j}(t)D\sigma^{k}(t)=0,
\end{equation}$$

where $\sigma(t)$ is the coordinate path corresponding to the manifold path γ.

For example, let's consider geodesic motion on the surface of a unit sphere. We let gamma be a map from the real line to the sphere, with colatitude alpha and longitude beta, as before. The geodesic equation is:
```Scheme
(show-expression
(((((covariant-derivative sphere-Cartan gamma)
d/dt)
((differential gamma) d/dt))
(chart S2-spherical))
((point R1-rect) 't0)))
```

$$\begin{equation}
\left(\begin{array}{c}
-\cos(\alpha(t0))\sin(\alpha(t0))(D\beta(t0))^{2}+D^{2}\alpha(t0)\\
\frac{2D\beta(t0)\cos(\alpha(t0))D\alpha(t0)}{\sin(\alpha(t))}+D^{2}\beta(t0)
\end{array}\right)
\end{equation}$$

The geodesic equation is the same as the Lagrange equation for free motion constrained to the surface of the unit sphere. The Lagrangian for motion on the sphere is the composition of the free-particle Lagrangian and the state transformation induced by the coordinate constraint:#Footnote(18)
```Scheme
(define (Lfree s)
(* 1/2 (square (velocity s))))

(define (sphere->R3 s)
(let ((q (coordinate s)))
(let ((theta (ref q 0)) (phi (ref q 1)))
(up (* (sin theta) (cos phi))
(* (sin theta) (sin phi))
(cos theta)))))

(define Lsphere
(compose Lfree (F->C sphere->R3)))
```
Then the Lagrange equations are:
```Scheme
(show-expression
(((Lagrange-equations Lsphere)
(up (literal-function 'alpha)
(literal-function 'beta)))
't))
```

$$\begin{equation}
\left[\begin{array}{c}
-(D\beta(t))^{2}\sin(\alpha(t))\cos(\alpha(t))+D^{2}\alpha(t)\\
2D\alpha(t)D\beta(t)\sin(\alpha(t))\cos(\alpha(t))+D^{2}\beta(t)(\sin(\alpha(t)))^{2}
\end{array}\right]
\end{equation}$$

The Lagrange equations are true of the same paths as the geodesic equations. The second Lagrange equation is the second geodesic equation multiplied by $(\sin(\alpha(t)))^{2}$, and the Lagrange equations are arranged in a down tuple, whereas the geodesic equations are arranged in an up tuple.#Footnote(19) The two systems are equivalent unless $\alpha(t)=0$, where the coordinate system is singular.

### Exercise 7.1: Hamiltonian Evolution

We have just seen that the Lagrange equations for the motion of a free particle constrained to the surface of a sphere determine the geodesics on the sphere. We can investigate the phenomenon in the Hamiltonian formulation. The Hamiltonian is obtained from the Lagrangian by a Legendre transformation:
```Scheme
(define Hsphere
(Lagrangian->Hamiltonian Lsphere))
```
We can get the coordinate representation of the Hamiltonian vector field as follows:
```Scheme
((phase-space-derivative Hsphere)
(up 't (up 'theta 'phi) (down 'p_theta 'p_phi)))
/(up 1/
/(up p_theta/
/(/ p_phi (expt (sin theta) 2)))/
/(down (/ (* (expt p_phi 2) (cos theta))/
/(expt (sin theta) 3))/
/0))/
```
The state space for Hamiltonian evolution has five dimensions:
time, two dimensions of position on the sphere, and two dimensions of momentum:
```Scheme
(define state-space
(make-manifold R^n 5))
(define states
(coordinate-system-at 'rectangular 'origin state-space))
(define-coordinates
(up t (up theta phi) (down p_theta p_phi))
states)
```
So now we have coordinate functions and the coordinate-basis vector fields and coordinate-basis one-form fields.

a. Define the Hamiltonian vector field as a linear combination of these fields.

b. Obtain the first few terms of the Taylor series for the evolution of the coordinates $(\theta,\phi)$ by exponentiating the Lie derivative of the Hamiltonian vector field.

### Exercise 7.2: Lie Derivative and Covariant Derivative

How are the Lie derivative and the covariant derivative related?

a. Prove that for every vector field there exists a connection such that the covariant derivative for that connection and the given vector field is equivalent to the Lie derivative with respect to that vector field.

b. Show that there is no connection that for every vector field makes the Lie derivative the same as the covariant derivative with the chosen connection.

----
### Footnotes


#FootnoteRef(1) The directional derivative of a vector field must itself be a vector field. Thus the real program for this must make the function of $\mathsf{f}$ into a vector field. However, we leave out this detail here to make the structure clear.

#FootnoteRef(2) For some derivative operators α can be a real-valued manifold function.

#FootnoteRef(3) The action on functions, vector fields, and one-form fields suffices to define the action on all tensor fields. See Appendix C.

#FootnoteRef(4) In these experiments we need some setup.
```Scheme
(define a (literal-manifold-function 'alpha R3-rect))
(define b (literal-manifold-function 'beta R3-rect))
(define c (literal-manifold-function 'gamma R3-rect))

(define-coordinates (up x y z) R3-rect)

(define theta (+ (* a dx) (* b dy) (* c dz)))

(define omega
(+ (* a (wedge dy dz))
(* b (wedge dz dx))
(* c (wedge dx dy))))

(define X (literal-vector-field 'X-rect R3-rect))
(define Y (literal-vector-field 'Y-rect R3-rect))
(define Z (literal-vector-field 'Z-rect R3-rect))
(define V (literal-vector-field 'V-rect R3-rect))
(define R3-rect-point
((point R3-rect) (up 'x0 'y0 'z0)))
```

#FootnoteRef(5) In the introduction the stick was always kept east-west rather than pointing south, but the phenomenon is the same!

#FootnoteRef(6) This program is incomplete. It must construct a vector field; it must make a differential operator; and it does not apply to functions or forms.

#FootnoteRef(7) This terminology may be restricted to the case in which the basis is a coordinate basis.

#FootnoteRef(8) The structure of the Cartan forms $\varpi$ together with this equation forces the shape of the Christoffel coefficient structure.

#FootnoteRef(9) We will need a few definitions:
```Scheme
(define R2-rect-basis (coordinate-system->basis R2-rect))
(define R2-polar-basis (coordinate-system->basis R2-polar))
(define-coordinates (up x y) R2-rect)
(define-coordinates (up r theta) R2-polar)
```

#FootnoteRef(10) Since the Christoffel coefficients are basis-dependent they are packaged with the basis.

#FootnoteRef(11) The code for making the Cartan forms is as follows:
```Scheme
(define (Christoffel->Cartan Christoffel)
(let ((basis (Christoffel->basis Christoffel))
(Christoffel-symbols (Christoffel->symbols Christoffel)))
(make-Cartan
(* Christoffel-symbols (basis->1-form-basis basis))
basis)))
```

#FootnoteRef(12) The argument $\mathsf{w}_{\gamma}$ makes sense because our parallel-transport operator never depended on the vector field tangent to the integral curve existing off of the curve. Because the connection is a form field (see equation 7.47), it does not depend on the value of its vector argument anywhere except at the point where it is being evaluated.

The argument $\mathsf{u}_{\gamma}$ is more difficult. We must modify equation (7.37):

$$\begin{equation}
F^{\mathsf{w}_{\gamma}}_{\delta}\mathsf{u}_{\gamma}(\mathsf{f})(t)=\sum_{i,j}A^{i}_{j}(\delta)u^{j}(t-\delta)\mathsf{e}^{\gamma}_{i}(\mathsf{f})(t).
\end{equation}$$

#FootnoteRef(13) You may have noticed that $t$ and $\mathsf{t}$ appear here. The real-line manifold point $\mathsf{t}$ has coordinate $t$.

#FootnoteRef(14) We will show later that these Christoffel coefficients are a natural choice for the sphere.

#FootnoteRef(15) If we give covariant-derivative an extra argument, in addition to the Cartan form, the covariant derivative treats the extra argument as a map and transforms the Cartan form to work over the map.

#FootnoteRef(16) A southward-pointing vector remains southward-pointing when it is parallel-transported along the equator. To do this we do not have to integrate the differential equations, because we know the answer.

#FootnoteRef(17) The equation of a geodesic path is often said to be

$$\begin{equation}
\nabla_{\mathsf{v}}\mathsf{v}=0,
\end{equation}$$

but this is nonsense. The geodesic equation is a constraint on the path, but the path does not appear in this equation. Further, the velocity along a path is not a vector field, so it cannot appear in either argument to the covariant derivative.

What is true is that a vector field $\mathsf{v}$ all of whose integral curves are geodesics satisfies equation (7.77).

#FootnoteRef(18) The method of formulating a system with constraints by composing a free system with the state-space coordinate transformation that represents the constraints can be found in [19](references!bib_19), section 1.6.3. The procedure F->C takes a coordinate transformation and produces a corresponding transformation of Lagrangian state.

#FootnoteRef(19) The geodesic equations and the Lagrange equations are related by a contraction with the metric.

#FootnoteEnd
