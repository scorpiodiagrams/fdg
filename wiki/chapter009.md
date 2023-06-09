!!Polyglot
#page(0)

# Chapter 9: Metrics

We often want to impose further structure on a manifold to allow us to define lengths and angles. This is done by generalizing the idea of the Euclidean dot product, which allows us to compute lengths of vectors and angles between vectors in traditional vector algebra.

For vectors $\vec{u} = u^x\hat{x} + u^y\hat{y} + u^z\hat{z}$ and $\vec{v} = v^x\hat{x} + v^y\hat{y} + v^z\hat{z}$ the dot product is $\vec{u} \cdot \vec{v} = u^xv^x + u^yv^y + u^zv^z$. The generalization is to provide coefficients for these terms and to include cross terms, consistent with the requirement that the function of two vectors is symmetric. This symmetric, bilinear, real-valued function of two vector fields is called a /metric field/.

For example, the natural metric on a sphere of radius $R$ is

$$\begin{equation}
\mathsf{g}(\mathsf{u}, \mathsf{v}) = \
R^2\left(\mathsf{d}\theta(\mathsf{u}) \mathsf{d}\theta(\mathsf{v}) \
+ (\sin \theta)^2 \mathsf{d}\phi(\mathsf{u}) \mathsf{d}\phi(\mathsf{v}) \right),
\end{equation}$$

and the Minkowski metric on the 4-dimensional space of special relativity is

$$\begin{equation}
\begin{aligned}
\mathsf{g}(\mathsf{u}, \mathsf{v}) = &\
\mathsf{d}x(\mathsf{u}) \mathsf{d}x(\mathsf{v}) \
+ \mathsf{d}y(\mathsf{u}) \mathsf{d}y(\mathsf{v}) \\
&+ \mathsf{d}z(\mathsf{u}) \mathsf{d}z(\mathsf{v}) \
- c^2 \mathsf{d}t(\mathsf{u}) \mathsf{d}t(\mathsf{v}).
\end{aligned}
\end{equation}$$

Although these examples are expressed in terms of a coordinate basis, the value of the metric on vector fields does not depend on the coordinate system that is used to specify the metric.

Given a metric field $\mathsf{g}$ and a vector field $\mathsf{v}$ the scalar field $\mathsf{g(v, v)}$ is the squared length of the vector at each point of the manifold.

### Metric Music
The metric can be used to construct a one-form field $\boldsymbol{\omega}_\mathsf{u}$ from a vector field $\mathsf{u}$, such that for any vector field $\mathsf{v}$ we have

$$\begin{equation}
\omega_\mathsf{u}(\mathsf{v}) = \mathsf{g(v, u)}.
\end{equation}$$

The operation of constructing a one-form field from a vector field using a metric is called "lowering" the vector field. It is sometimes notated as

$$\begin{equation}
\boldsymbol{\omega}_\mathsf{u} = g^\flat(\mathsf{u}).
\end{equation}$$

There is also an inverse metric that takes two one-form fields. It is defined by the relation

$$\begin{equation}
\delta_k^i = \sum_j {g^{-1} \left(\tilde{\mathsf{e}}^i, \tilde{\mathsf{e}}^j \right) \mathsf{g}(\mathsf{e}_j, \mathsf{e}_k)}.
\end{equation}$$

where $\mathsf{e}$ and $\tilde{\mathsf{e}}$ are any basis and its dual basis.

The inverse metric can be used to construct a vector field $\mathsf{v}_\omega$ from a one-form field $\boldsymbol{\omega}$, such that for any one-form field $\boldsymbol{\tau}$ we have

$$\begin{equation}
\boldsymbol{\tau}(\mathsf{v}_\omega) = \mathsf{g}^{-1}(\boldsymbol{\omega}, \boldsymbol{\tau}).
\end{equation}$$

This definition is implicit, but the vector field can be explicitly computed from the one-form field with respect to a basis as follows:

$$\begin{equation}
\mathsf{v}_\omega = \sum_i {g^{-1} \left(\boldsymbol{\omega}, \tilde{\mathsf{e}}^i \right) \mathsf{e}_i}.
\end{equation}$$

The operation of constructing a vector field from a one-form field using a metric is called "raising" the one-form field. It is sometimes notated

$$\begin{equation}
\mathsf{v}_\omega = \mathsf{g}^\sharp(\boldsymbol{\omega}).
\end{equation}$$

The raising and lowering operations allow one to interchange the vector fields and the one-form fields. However they should not be confused with the dual operation that allows one to construct a dual one-form basis from a vector basis or construct a vector basis from a one-form basis. The dual operation that interchanges bases is defined without assigning a metric structure on the space.

Lowering a vector field with respect to a metric is a simple program:

```Scheme
(define ((lower metric) u)
(define (omega v) (metric v u))
(procedure->1form-field omega))
```

But raising a one-form field to make a vector field is a bit more complicated:

```Scheme
(define (raise metric basis)
(let ((gi (metric:invert metric basis)))
(lambda (omega)
(contract (lambda (e i w^i)
         (* (gi omega w^i) e i))
       basis))))
```

where #Code(contract) is the trace over a basis of a two-argument function that takes a vector field and a one-form field as its arguments.#Footnote(1)

```Scheme
(define (contract proc basis)
(let ((vector-basis (basis->vector-basis basis))
(1form-basis (basis->1form-basis basis)))
(s:sigma/r proc
      vector-basis
      1form-basis)))
```

### Metric Compatibility

A connection is said to be compatible with a metric $\mathsf{g}$ if the covariant derivative for that connection obeys the "product rule":

$$\begin{equation}
\Delta_\mathsf{X}\left(g(\mathsf{Y}, \mathsf{Z})\right) = g\left(\Delta_\mathsf{X}(\mathsf{Y}), \mathsf{Z}\right) \
+ g\left(\mathsf{Y}, \Delta_\mathsf{X}(\mathsf{Z})\right).
\end{equation}$$

For a metric there is a unique torsion-free connection that is compatible with it. The Christoffel coefficients of the first kind are computed from the metric by the following:

$$\begin{equation}
\bar{\Gamma}_{i j k} = \frac{1}{2}\left(\mathsf{e}_{k}\left(\mathsf{g}\left(\mathsf{e}_{i}, \mathsf{e}_{j}\right)\right) \
+ \mathsf{e}_{j}\left(\mathsf{g}\left(\mathsf{e}_{i}, \mathsf{e}_{k}\right)\right) \
- \mathsf{e}_{i}\left(\mathsf{g}\left(\mathsf{e}_{j}, \mathsf{e}_{k}\right)\right)\right)
\end{equation}$$

for the coordinate basis $\mathsf{e}$. We can then construct the Christoffel coefficients of the second kind (the ones used previously to define a connection) by "raising the first index." To do this we define a function of three vectors, with a weird currying:

$$\begin{equation}
\sum_{ijk} {\bar{\Gamma}_{ijk} \tilde{\mathsf{e}}^i(\mathsf{u}) \tilde{\mathsf{e}}^j(\mathsf{v}) \tilde{\mathsf{e}}^k(\mathsf{w})}.
\end{equation}$$

This function takes two vector fields and produces a one-form field. We can use it with equation (9.7) to construct a new function that takes two vector fields and produces a vector field:

$$\begin{equation}
\hat{\Gamma}(\mathsf{v}, \mathsf{w}) \
= \sum_i {\mathsf{g}^{-1} \left(\tilde{\Gamma}(\mathsf{v}, \mathsf{w}), \tilde{\mathsf{e}}^i\right)\mathsf{e}_i}.
\end{equation}$$

We can now construct the Christoffel coefficients of the second kind:

$$\begin{equation}
\Gamma_{jk}^i = \tilde{\mathsf{e}}^i \left(\hat{\Gamma}\left(\mathsf{e}_j, \mathsf{e}_k \right)\right) \
= \sum_m \bar{\Gamma}_{mjk} \mathsf{g}^{-1} \left(\tilde{\mathsf{e}}^m, \tilde{\mathsf{e}}^i \right)
\end{equation}$$

The Cartan forms are then just

$$\begin{equation}
\varpi_j^i \
= \sum_k \Gamma_{jk}^i \tilde{\mathrm{e}}^k \
= \sum_k \tilde{\mathrm{e}}^i \left(\hat{\Gamma}\left(\mathrm{e}_j, \mathrm{e}_k \right)\right) \tilde{\mathrm{e}}^k.
\end{equation}$$

So, for example, we can compute the Christoffel coefficients for the sphere from the metric for the sphere. First, we need the metric:

```Scheme
(define ((g-sphere R) u v)
(* (square R)
(+ (* (dtheta u) (dtheta v))
(* (compose (square sin) theta)
  (dphi u)
  (dphi v)))))
```

The Christoffel coefficients of the first kind are a complex structure with all three indices down:

```Scheme
((Christoffel->symbols
(metric->Christoffel-1 (g-sphere 'R) S2-basis))
((point S2-spherical) (up 'theta0 'phi0)))
;; (down
;;  (down (down 0 0)
;;        (down 0 (* (* (cos theta0) (sin theta0)) (expt R 2))))
;;  (down (down 0 (* (* (cos theta0) (sin theta0)) (expt R 2)))
;;        (down (* (* -1 (cos theta0) (sin theta0)) (expt R 2))
;;              0)))
```

And the Christoffel coefficients of the second kind have the innermost index up:

```Scheme
((Christoffel->symbols
(metric->Christoffel-2 (g-sphere 'R) S2-basis))
((point S2-spherical) (up 'theta0 'phi0)))
;; (down (down (up 0 0)
;;             (up 0 (/ (cos theta0) (sin theta0))))
;;       (down (up 0 (/ (cos theta0) (sin theta0)))
;;             (up (* -1 (cos theta0) (sin theta0)) 0)))
```

### Exercise 9.1: Metric Compatibility

The connections constructed from a metric by equation (9.13) are "metric compatible," as described in equation (9.9). Demonstrate that this is true for a literal metric, as described on page 6, in $\mathbf{R}^4$. Your program should produce a zero.

###  Metrics and Lagrange Equations

In the Introduction (Chapter 1) we showed that the Lagrange equations for a free particle constrained to a 2-dimensional surface are equivalent to the geodesic equations for motion on that surface. We illustrated that in detail in Section 7.4 for motion on a sphere.

Here we expand this understanding to show that the Christoffel symbols can be derived from the Lagrange equations. Specifically, if we solve the Lagrange equations for the acceleration (the highest-order derivatives) we find that the Christoffel symbols are the symmetrized coefficients of the quadratic velocity terms.

Consider the Lagrange equations for a free particle, with Lagrangian

$$\begin{equation}
L_2(t, x, v) = \frac{1}{2}g(x)(v,v).
\end{equation}$$

If we solve the Lagrange equations for the accelerations, the accelerations can be expressed with the geodesic equations (7.79):

$$\begin{equation}
D^2 q^i + \sum_{jk} \left(\Gamma_{jk}^i \circ \chi^{-1} \circ q \right) Dq^j Dq^{k} = 0.
\end{equation}$$

We can verify this computationally. Given a metric, we can construct a Lagrangian where the kinetic energy is the metric applied to the velocity twice: The kinetic energy is proportional to the squared length of the velocity vector.

```Scheme
(define (metric->Lagrangian metric coordsys)
(define (L state)
(let ((q (ref state 1)) (qd (ref state 2)))
(define v
(components->vector-field (lambda (m) qd) coordsys))
((* 1/2 (metric v v)) ((point coordsys) q))))
L)
```

The following code compares the Christoffel symbols with the coefficients of the terms of second order in velocity appearing in the accelerations, determined by solving the Lagrange equations for the highest-order derivative.#Footnote(2) We extract these terms by taking two partials with respect to the structure of velocities. Because the elementary partials commute we get two copies of each coefficient, requiring a factor of 1/2.

```Scheme
(let* ((metric (literal-metric 'g R3-rect))
(q (typical-coords R3-rect))
(L2 (metric->Lagrangian metric R3-rect)))
(+ (* 1/2
(((expt (partial 2) 2) (Lagrange-explicit L2))
(up 't q (corresponding-velocities q))))
((Christoffel->symbols
(metric->Christoffel-2 metric
                     (coordinate-system->basis R3-rect)))
((point R3-rect) q))))
;; (down (down (up 0 0 0) (up 0 0 0) (up 0 0 0))
;;       (down (up 0 0 0) (up 0 0 0) (up 0 0 0))
;;       (down (up 0 0 0) (up 0 0 0) (up 0 0 0)))
```

We get a structure of zeros, demonstrating the correspondence between Christoffel symbols and coefficients of the Lagrange equations.

Thus, if we have a metric specifying an inner product, the geodesic equations are equivalent to the Lagrange equations for the Lagrangian that is equal to the inner product of the generalized velocities with themselves

### Kinetic Energy or Arc Length

A geodesic is a path of stationary length with respect to variations in the path that keep the endpoints fixed. On the other hand, the solutions of the Lagrange equations are paths of stationary action that keep the endpoints fixed. How are these solutions related?

The integrand of the traditional action is the Lagrangian, which is in this case the Lagrangian $L_2$, the kinetic energy. The integrand of the arc length is

$$\begin{equation}
L_1(t, x, v) = \sqrt{g(x)(v, v)} = \sqrt{2L_2(t, x, v)}
\end{equation}$$

and the path length is

$$\begin{equation}
\tau = \int_{t_1}^{t_2}{L_1\left(t, q(t), Dq(t)\right)dt}.
\end{equation}$$

If we compute the Lagrange equations for $L_2$ we get the Lagrange equations for $L_1$ with a correction term. Since

$$\begin{equation}
L_2(t, x, v) = \frac{1}{2}(L_1(t, x, v))^2,
\end{equation}$$

and the Lagrange operator for $L_2$ is#Footnote(3)

$$\begin{equation}
\boldsymbol{E}[L_2] = D_t \partial_2 L_2 - \partial_1 L_2,
\end{equation}$$

we find

$$\begin{equation}
\boldsymbol{E}[L_2] =L_1 \boldsymbol{E}[L_1] + \partial_2 L_1 D_t L_1.
\end{equation}$$

$L_2$ is the kinetic energy. It is conserved along solution paths, since there is no explicit time dependence. Because of the relation between $L_1$ and $L_2$, $L_1$ is also a conserved quantity. Let $L_1$ take the constant value $a$ on the geodesic coordinate path $q$ we are considering. Then $\tau = a(t_2 - t_1)$. Since $L_1$ is conserved, $(D_t L_1) \circ \boldsymbol{\Gamma}[q] = 0$ on the geodesic path $q$, and both $\boldsymbol{E}[L_1] \circ \boldsymbol{\Gamma}[q] = 0$ and $\boldsymbol{E}[L_2] \circ \boldsymbol{\Gamma}[q] = 0$, as required by equation (9.20).

Since $L_2$ is homogeneous of degree 2 in the velocities, $L_1$ is homogeneous of degree 1. So we cannot solve for the highest-order derivative in the Lagrange-Euler equations derived from $L_1$: The Lagrange equations of the Lagrangian $L_1$ are dependent. But although they do not uniquely specify the evolution, they do specify the geodesic path.

On the other hand, we can solve for the highest-order derivative in $\boldsymbol{E}[L_2]$. This is because $L_1 \boldsymbol{E}[L_1]$ is homogeneous of degree 2. So the equations derived from $L_2$ uniquely

#### For Two Dimensions

We can show this is true for a 2-dimensional system with a general metric. We define the Lagrangians in terms of this metric:

```Scheme
(define L2
(metric->Lagrangian (literal-metric 'm R2-rect)
             R2-rect))

(define (L1 state)
(sqrt (* 2 (L2 state))))
```
Although the mass matrix of $L_2$ is nonsingular
```Scheme
(determinant
(((partial 2) ((partial 2) L2))
(up 't (up 'x 'y) (up 'vx 'vy))))
;; (+ (* (m_00 (up x y)) (m_11 (up x y)))
;;    (* -1 (expt (m_01 (up x y)) 2)))
```
the mass matrix of $L_1$ has determinant zero

```Scheme
(determinant
(((partial 2) ((partial 2) L1))
(up 't (up 'x 'y) (up 'vx 'vy))))
;; 0
```
showing that these Lagrange equations are dependent.

We can show this dependence explicitly, for a simple system. Consider the simplest possible system, a geodesic (straight line) in a plane:

```Scheme
(define (L1 state)
(sqrt (square (velocity state))))
```

```Scheme
(((Lagrange-equations L1)
(up (literal-function 'x) (literal-function 'y)))
't)
;; (down
;;  (/ (+ (* (((expt D 2) x) t) (expt ((D y) t) 2))
;;        (* -1 ((D x) t) ((D y) t) (((expt D 2) y) t)))
;;     (expt (+ (expt ((D x) t) 2) (expt ((D y) t) 2)) 3/2))
;;  (/ (+ (* -1 (((expt D 2) x) t) ((D x) t) ((D y) t))
;;        (* (expt ((D x) t) 2) (((expt D 2) y) t)))
;;     (expt (+ (expt ((D x) t) 2) (expt ((D y) t) 2)) 3/2)))

```

These residuals must be zero; so the numerators must be zero.#Footnote(4) They are:

$$\begin{equation}
\begin{align*}
D^2x\,(Dy)^2 &= Dx\,Dy\,D^2y \\
D^2x\,Dx\,Dy &= (Dx)^2\,D^2y
\end{align*}
\end{equation}$$

Note that the only constraint is $D^2x\,Dy = Dx\,D^2y$, so the resulting Lagrange equations are dependent.

This is enough to determine that the result is a straight line, without specifying the rate along the line. Suppose $y = f(x)$, for path $(x(t), y(t))$. Then

$$\begin{equation}
Dy = Df(x)\,Dx\text{ and } D^2y = D^2f(x)\,Dx + Df(x)\,D^2(x).
\end{equation}$$

Substituting, we get

$$\begin{equation}
Df(x)\,Dx\,D^2x = Dx\left(D^2f(x)\,Dx + Df(x)\,D^2x\right)
\end{equation}$$

or

$$\begin{equation}
Df(x)\,D^2x = D^2f(x)\,Dx + Df(x)\,D^2x,
\end{equation}$$

so $D^2f(x) = 0$. Thus $f$ is a straight line, as required.

#### Reparametrization

More generally, a differential equation system $F[q](t) = 0$ is said to be /reparameterized/ if the coordinate path $q$ is replaced with a new coordinate path $q \circ f$. For example, we may change the scale of the independent variable. The system $F[q \circ f] = 0$ is said to be independent of the parameterization if and only if $F[q] \circ f = 0$. So the differential equation system is satisfied by $q \circ f$ if and only if it is satisfied by $q$.

The Lagrangian $L_1$ is homogeneous of degree 1 in the velocities; so

$$\begin{equation}
\boldsymbol{E}[L_1] \circ \Gamma[q \circ f] \
- \left(\boldsymbol{E}[L_1] \circ \Gamma[q] \circ f\right) Df = 0.
\end{equation}$$

We can check this in a simple case. For two dimensions $q = (x, y)$, the condition under which a reparameterization $f$ of the geodesic paths with coordinates $q$ satisfies the Lagrange equations for $L_1$ is:

```Scheme
(let ((x (literal-function 'x))
(y (literal-function 'y))
(f (literal-function 'f))
(E1 (Euler-Lagrange-operator L1)))
((- (compose E1
      (Gamma (up (compose x f)
                 (compose y f))
             4))
(* (compose E1
         (Gamma (up x y) 4)
         f)
(D f)))
't))
;; (down 0 0)
```

This residual is identically satisfied, showing that the Lagrange equations for $L_1$ are independent of the parameterization of the independent variable.

The Lagrangian $L_2$ is homogeneous of degree 2 in the velocities; so

$$\begin{equation*}
\begin{align*}
\boldsymbol{E}[L_2][q \circ f] - &(\boldsymbol{E}[L_2][q]  \circ f)(Df)^2 \\
&= \left(\partial_2 L_2 \circ \Gamma[q] \circ f\right)(D^2f).
\end{align*}
\end{equation*}$$

Although the Euler-Lagrange equations for $L_1$ are invariant under an arbitrary reparameterization $(Df \ne 0)$, the Euler-Lagrange equations for $L_2$ are invariant only for a restricted set of $f$. The conditions under which a reparameterization $f$ of geodesic paths with coordinates $q$ satisfies the Lagrange equations for $L_2$ are:

```Scheme
(let ((q (up (literal-function 'x) (literal-function 'y)))
(f (literal-function 'f)))
((- (compose (Euler-Lagrange-operator L2)
      (Gamma (compose q f) 4))
(* (compose (Euler-Lagrange-operator L2)
         (Gamma q 4)
         f)
(expt (D f) 2)))
't))
;; (down
;;  (* (+ (* ((D x) (f t)) (m 00 (up (x (f t)) (y (f t)))))
;;        (* ((D y) (f t)) (m 01 (up (x (f t)) (y (f t))))))
;;     (((expt D 2) f) t))
;;  (* (+ (* ((D x) (f t)) (m 01 (up (x (f t)) (y (f t)))))
;;        (* ((D y) (f t)) (m 11 (up (x (f t)) (y (f t))))))
;;     (((expt D 2) f) t)))
```

We see that if these expressions must be zero, then $D^2f = 0$. This tells us that $f$ is at most affine in $t: f(t) = at + b$.

### Exercise 9.2: SO(3) Geodesics

We have derived a basis for SO(3) in terms of incremental rotations around the rectangular axes. See equations (4.29, 4.30, 4.31). We can use the dual basis to define a metric on SO(3).

```Scheme
(define (SO3-metric v1 v2)
(+ (* (e^x v1) (e^x v2))
(* (e^y v1) (e^y v2))
(* (e^z v1) (e^z v2))))
```

This metric determines a connection. Show that uniform rotation about an arbitrary axis traces a geodesic on SO(3).

### Exercise 9.3: Curvature of a Spherical Surface

The 2-dimensional surface of a 3-dimensional sphere can be embedded in three dimensions with a metric that depends on the radius:

```Scheme
(define M (make-manifold S^2-type 2 3))
(define spherical
(coordinate-system-at 'spherical 'north-pole M))
(define-coordinates (up theta phi) spherical)
(define spherical-basis (coordinate-system->basis spherical))

(define ((spherical-metric r) v1 v2)
(* (square r)
(+ (* (dtheta v1) (dtheta v2))
(* (square (sin theta))
  (dphi v1) (dphi v2)))))
```

If we raise one index of the Ricci tensor (see equation 8.20) by contracting it with the inverse of the metric tensor we can further contract it to obtain a scalar manifold function:

$$\begin{equation}
R = \sum_{ij} \mathsf{g}\left(\tilde{\mathsf{e}}^i, \tilde{\mathsf{e}}^j\right) \
r\left(\mathsf{e}^i, \mathsf{e}^j\right).
\end{equation}$$

The #Code(trace2down) procedure converts a tensor that takes two vector fields into a tensor that takes a vector field and a one-form field, and then it contracts the result over a basis to make a trace. It is useful for getting the Ricci scalar from the Ricci tensor, given a metric and a basis.

```Scheme
(define ((trace2down metric basis) tensor)
(let ((inverse-metric-tensor
(metric:invert metric-tensor basis)))
(contract
(lambda (v1 w1)
(contract
(lambda (v w)
 (* (inverse-metric-tensor w1 w)
    (tensor v v1)))
basis))
basis)))
```

Evaluate the Ricci scalar for a sphere of radius $r$ to obtain a measure of its intrinsic curvature. You should obtain the answer $2/r^2$.

### Exercise 9.4: Curvature of a Pseudosphere

Compute the scalar curvature of the pseudosphere (see [exercise 8.2](#exercise_8.2)). You should obtain the value −2.

### General Relativity

By analogy to Newtonian mechanics, relativistic mechanics has two parts. There are equations of motion that describe how particles move under the influence of "forces" and there are field equations that describe how the forces arise. In general relativity the only force considered is gravity. However, gravity is not treated as a force. Instead, gravity arises from curvature in the spacetime, and the equations of motion are motion along geodesics of that space.

The geodesic equations for a spacetime with the metric

$$\begin{equation}
\begin{aligned}
\end{aligned}
\end{equation}$$

are Newton's equations to lowest order in $V/c^2$:

$$\begin{equation}

\end{equation}$$

### Exercise 9.5: Newton's Equations

Verify that Newton's equations (9.25) are indeed the lowest-order terms of the geodesic equations for the metric (9.24).

Einstein's field equations tell how the local energy-momentum distribution determines the local shape of the spacetime, as described by the metric tensor $g$. The equations are traditionally written

$$\begin{equation}
\end{equation}$$

where $R_{\mu \nu}$ are the components of the Ricci tensor (equation 8.20),
$R$ is the Ricci scalar (equation 9.23),#Footnote(5) and $\Lambda$ is the cosmological constant.

$T_{\mu \nu}$ are the components of the stress-energy tensor describing the energy-momentum distribution. Equivalently, one can write

$$\begin{equation}
R_{\mu \nu} = \frac{8 \pi G}{c^4} \left(T_{\mu \nu} - \frac{1}{2} T g_{\mu \nu} \right) - \Lambda g_{\mu \nu}
\end{equation}$$

where $T =T_{\mu \nu} g^{\mu \nu}$.#Footnote(6)

Einstein's field equations arise from a heuristic derivation by analogy to the Poisson equation for a Newtonian gravitational field:

$$\begin{equation}
\operatorname{Lap}(V) = 4\pi G \rho
\end{equation}$$

where $V$ is the gravitational potential field at a point, $\rho$ is the mass density at that point, and $\operatorname{Lap}$ is the Laplacian operator.

The time-time component of the Ricci tensor derived from the metric (9.24) is the Laplacian of the potential, to lowest order.

```Scheme
(define (Newton-metric M G c V)
(let ((a
(+ 1 (* (/ 2 (square c))
        (compose V (up x y z))))))
(define (g v1 v2)
(+ (* -1 (square c) a (dt v1) (dt v2))
(* (dx v1) (dx v2))
(* (dy v1) (dy v2))
(* (dz v1) (dz v2))))
g))

(define (Newton-connection M G c V)
(Christoffel->Cartan
(metric->Christoffel-2 (Newton-metric M G c V)
                 spacetime-rect-basis)))

(define nabla
(covariant-derivative
(Newton-connection 'M 'G ':c
             (literal-function 'V (-> (UP Real Real Real) Real)))))

```

```Scheme
(((Ricci nabla (coordinate-system->basis spacetime-rect))
d/dt d/dt)
((point spacetime-rect) (up 't 'x 'y 'z)))
;; mess
```

The leading terms of the mess are

```Scheme
(+ (((partial 0) ((partial 0) V)) (up x y z))
(((partial 1) ((partial 1) V)) (up x y z))
(((partial 2) ((partial 2) V)) (up x y z)))
```

which is the Laplacian of V . The other terms are smaller by $V/c^2$.

Now consider the right-hand side of equation (9.27). In the Poisson equation the source of the gravitational potential is the density of matter. Let the time-time component of the stress-energy tensor $T_{00}$ be the matter density $\rho$. Here is a program for the stress-energy tensor:

```Scheme
(define (Tdust rho)
(define (T w1 w2)
(* rho (w1 d/dt) (w2 d/dt)))
T)
```

If we evaluate the right-hand side expression we obtain#Footnote(7)

```Scheme
(let ((g (Newton-metric 'M 'G ':c V)))
(let ((T ij ((drop2 g spacetime-rect-basis) (Tdust 'rho))))
(let ((T ((trace2down g spacetime-rect-basis) T ij)))
((- (T ij d/dt d/dt) (* 1/2 T (g d/dt d/dt)))
((point spacetime-rect) (up 't 'x 'y 'z))))))
;; (* 1/2 (expt :c 4) rho)
```

So, to make the Poisson analogy we get

$$\begin{equation}
R_{\mu \nu} = \frac{8 \pi G}{c^4} \left(T_{\mu \nu} - \frac{1}{2} T g_{\mu \nu} \right) - \Lambda g_{\mu \nu}
\end{equation}$$

as required.

### Exercise 9.6: Curvature of Schwarzschild Spacetime

In spherical coordinates around a nonrotating gravitating body the metric of Schwarzschild spacetime is given as:#Footnote(8)

```Scheme
(define-coordinates (up t r theta phi) spacetime-sphere)

(define (Schwarzschild-metric M G c)
(let ((a (- 1 (/ (* 2 G M) (* (square c) r)))))
(lambda (v1 v2)
(+ (* -1 (square c) a (dt v1) (dt v2))
(* (/ 1 a) (dr v1) (dr v2))
(* (square r)
   (+ (* (dtheta v1) (dtheta v2))
      (* (square (sin theta))
         (dphi v1) (dphi v2))))))))
```

Show that the Ricci curvature of the Schwarzschild spacetime is zero. Use the definition of the Ricci tensor in equation (8.20).

### Exercise 9.7: Circular Orbits in Schwarzschild Spacetime

Test particles move along geodesics in spacetime. Now that we have a metric for Schwarzschild spacetime (page 147) we can use it to construct the geodesic equations and determine how test particles move. Consider circular orbits. For example, the circular orbit along a line of constant longitude is a geodesic, so it should satisfy the geodesic equations. Here is the equation of a circular path along the zero longitude line.

```Scheme
(define (prime-meridian r omega)
(compose (point spacetime-sphere)
  (lambda (t) (up t r (* omega t) 0))
  (chart R1-rect)))
```

This equation will satisfy the geodesic equations for compatible values of the radius #Code(r) and the angular velocity =omega. If you substitute this into the geodesic equation and set the residual to zero you will obtain a constraint relating =r and #Code(omega). Do it.

Surprise: You should find out that $\omega^2 r^3 = GM$ --- Kepler's law!

### Exercise 9.8: Stability of Circular Orbits

In Schwarzschild spacetime there are stable circular orbits if the coordinate $r$ is large enough, but below that value all orbits are unstable. The critical value of $r$ is larger than the Schwarzschild horizon radius. Let's find that value.

For example, we can consider a perturbation of the orbit of constant longitude. Here is the result of adding an exponential variation of size #Code(epsilon):

```Scheme
(define (prime-meridian+X r epsilon X)
(compose
(point spacetime-sphere)
(lambda (t)
(up (+ t (* epsilon (* (ref X 0) (exp (* 'lambda t)))))
(+ r (* epsilon (* (ref X 1) (exp (* 'lambda t)))))
(+ (* (sqrt (/ (* 'G 'M) (expt r 3))) t)
   (* epsilon (* (ref X 2) (exp (* 'lambda t)))))
0))
(chart R1-rect)))
```

Plugging this into the geodesic equation yields a structure of residuals:

```Scheme
(define (geodesic-equation+X-residuals eps X)
(let ((gamma (prime-meridian+X 'r eps X)))
(((((covariant-derivative Cartan gamma) d/dtau)
((differential gamma) d/dtau))
(chart spacetime-sphere))
((point R1-rect) 't))))
```

The characteristic equation in the eigenvalue #Code(lambda) can be obtained as the numerator of the expression:

```Scheme
(determinant
(submatrix (((* (partial 1) (partial 0))
     geodesic-equation+X-residuals)
    0
    (up 0 0 0))
   0 3 0 3))
```

Show that the orbits are unstable if $r < 6GM / c^2$.

### Exercise 9.9: Friedmann-Lemaître-Robertson-Walker

The Einstein tensor $G_{\mu \nu}$ (see footnote 5) can be expressed as a program:

```Scheme
(define (Einstein coordinate-system metric-tensor)
(let* ((basis (coordinate-system->basis coordinate-system))
(connection
 (Christoffel->Cartan
  (metric->Christoffel-2 metric-tensor basis)))
(nabla (covariant-derivative connection))
(Ricci-tensor (Ricci nabla basis))
(Ricci-scalar
 ((trace2down metric-tensor basis) Ricci-tensor)))
(define (Einstein-tensor v1 v2)
(- (Ricci-tensor v1 v2)
(* 1/2 Ricci-scalar (metric-tensor v1 v2))))
Einstein-tensor))

(define (Einstein-field-equation coordinate-system metric-tensor Lambda stress-energy-tensor)
(let ((Einstein-tensor
(Einstein coordinate-system metric-tensor)))
(define EFE-residuals
(- (+ Einstein-tensor (* Lambda metric-tensor))
(* (/ (* 8 :pi :G) (expt :c 4))
   stress-energy-tensor)))
EFE-residuals))
```

One exact solution to the Einstein equations was found by Alexander Friedmann in 1922. He showed that a metric for an isotropic and homogeneous spacetime was consistent with a similarly isotropic and homogeneous stress-energy tensor in Einstein's equations. In this case the residuals of the Einstein equations gave ordinary differential equations for the time-dependent scale of the universe. These are called the Robertson-Walker equations. Friedmann's metric is:

```Scheme
(define (FLRW-metric c k R)
(define-coordinates (up t r theta phi) spacetime-sphere)
(let ((a (/ (square (compose R t)) (- 1 (* k (square r)))))
(b (square (* (compose R t) r))))
(define (g v1 v2)
(+ (* -1 (square c) (dt v1) (dt v2))
(* a (dr v1) (dr v2))
(* b (+ (* (dtheta v1) (dtheta v2))
        (* (square (sin theta))
           (dphi v1) (dphi v2))))))
g))
```

Here #Code(c) is the speed of light, #Code(k) is the intrinsic curvature, and #Code(R) is a length scale that is a function of time.

The associated stress-energy tensor is

```Scheme
(define (Tperfect-fluid rho p c metric)
(define-coordinates (up t r theta phi) spacetime-sphere)
(let* ((basis (coordinate-system->basis spacetime-sphere))
(inverse-metric (metric:invert metric basis)))
(define (T w1 w2)
(+ (* (+ (compose rho t)
      (/ (compose p t) (square c)))
   (w1 d/dt) (w2 d/dt))
(* (compose p t) (inverse-metric w1 w2))))
T))
```

where #Code(rho) is the energy density, and #Code(p) is the pressure in an ideal fluid model.

The Robertson-Walker equations are:

$$\begin{equation}
\begin{aligned}
\left(\frac{DR(t)}{R(t)} \right)^2 + \frac{kc^2}{(R(t))^2} - \frac{\Lambda c^2}{3} = \frac{8 \pi G}{3} \rho(t), \\
2 \frac{D^2 R(t)}{R(t)} - \frac{2}{3} \Lambda c^2 = -8 \pi G \left(\frac{\rho(t)}{3} + \frac{p(t)}{c^2} \right).
\end{aligned}
\end{equation}$$

Use the programs supplied to derive the Robertson-Walker equations.

### Exercise 9.10: Cosmology

For energy to be conserved, the stress-energy tensor must be constrained so that its covariant divergence is zero

$$\begin{equation}
\sum_\mu \Delta_{e_\mu} T\left(\tilde{\mathsf{e}}^\mu, \omega \right) = 0
\end{equation}$$

for every one-form $\omega$.

a. Show that for the perfect fluid stress-energy tensor and the FLRW metric this constraint is equivalent to the differential equation

$$\begin{equation}
D\left(c^2 \rho R^3\right) + pD\left(R^3\right) = 0.
\end{equation}$$

b. Assume that in a "matter-dominated universe" radiation pressure is negligible, so $p = 0$. Using the Robertson-Walker equations (9.30) and the energy conservation equation (9.32) show that the observation of an expanding universe is compatible with a negative curvature universe, a flat universe, or a positive curvature universe: $k \in \left\{−1, 0, +1\right\}$.

----
### Footnotes


#FootnoteRef(1) Notice that #Code(raise) and #Code(lower) are not symmetrical. This is because vector fields and form fields are not symmetrical: a vector field takes a manifold function as its argument, whereas a form field takes a vector field as its argument. This asymmetry is not apparent in traditional treatments based on index notation.

#FootnoteRef(2) The procedure #Code(Lagrange-explicit) produces the accelerations of the coordinates. In this code the division operator (#Code(/)) multiplies its first argument on the left by the inverse of its second argument.

```Scheme
(define (Lagrange-explicit L)
(let ((P ((partial 2) L))
(F ((partial 1) L)))
(/ (- F (+ ((partial 0) P) (* ((partial 1) P) velocity)))
((partial 2) P))))
```

#FootnoteRef(3) $\mathbf{E}$ is the Euler-Lagrange operator, which gives the residuals of the Lagrange equations for a Lagrangian. $\mathbf{\Gamma}$ extends a configuration-space path $q$ to make a state-space path, with as many terms as needed: $\mathbf{\Gamma}[q](t) = (t, q(t), Dq(t), \ldots)$. The total time derivative $D_t$ is defined by $D_t F \circ \mathbf{\Gamma}[q] = D\left(F \circ
\mathbf{\Gamma}[q]\right)$ for any state function $F$ and path $q$. The Lagrange equations are $\mathbf{E}$[L] \circ \Gamma[q] = 0. See [19](references!bib_19) for more details.

#FootnoteRef(4) We cheated: We hand-simplified the denominator to make the result more obvious.

#FootnoteRef(5) The tensor with components $G_{\mu \nu} = R_{\mu \nu} - \frac{1}{2} R g_{\mu \nu}$ is called the Einstein tensor. In his search for an appropriate field equation for gravity, Einstein demanded /general covariance/ (independence of coordinate system) and local Lorentz invariance (at each point transformations must preserve the line element). These considerations led Einstein to look for a tensor equation (see Appendix C).

#FootnoteRef(6) Start with equation (9.26). Raise one index of both sides, and then contract. Notice that the trace $g_\mu^\mu = 4$, the dimension of spacetime.
This gets $R = −\left(\frac{8 \pi G}{c^4}\right) T$ , from which we can deduce equation (9.27).

#FootnoteRef(7) The procedure #Code(trace2down) is defined on page 144. This expression also uses #Code(drop2), which converts a tensor field that takes two one-form fields into a tensor field that takes two vector fields. Its definition is

```Scheme
(define ((drop2 metric-tensor basis) tensor)
(lambda (v1 v2)
(contract
(lambda (e1 w1)
(contract
(lambda (e2 w2)
 (* (metric-tensor v1 e1) (tensor w1 w2) (metric-tensor e2 v2)))
basis))
basis)))
```

#FootnoteRef(8) The spacetime manifold is built from $\mathbf{R}^4$ with the addition of appropriate coordinate systems:

```Scheme
(define spacetime (make-manifold R^n 4))
(define spacetime-rect
(coordinate-system-at 'rectangular 'origin spacetime))
(define spacetime-sphere
(coordinate-system-at 'spacetime-spherical 'origin spacetime))
```
