!!Polyglot
#page(0)

# Chapter 4: Basis Fields

A vector field may be written as a linear combination of basis vector fields. If #Code(n) is the dimension, then any set of #Code(n) linearly independent vector fields may be used as a basis. The coordinate basis $\mathsf{X}$ is an example of a basis.#Footnote(1)

We will see later that not every basis is a coordinate basis: in order to be a coordinate basis, there must be a coordinate system such that each basis element is the directional derivative operator in a corresponding coordinate direction.

Let $\mathsf{e}$ be a tuple of basis vector fields, such as the coordinate basis
$\mathsf{X}$. The general vector field $\mathsf{v}$ applied to an arbitrary manifold function $\mathsf{f}$ can be expressed as a linear combination

$$\begin{equation}
\mathsf{v}(\mathsf{f})(\mathsf{m}) = \mathsf{e}(\mathsf{f})(\mathsf{m}) \
\mathsf{b}(\mathsf{m}) = \
\sum_i \mathsf{e}_i(\mathsf{f})(\mathsf{m})\mathsf{b}^i(\mathsf{m}),
\end{equation}$$

where $\mathsf{b}$ is a tuple-valued coefficient function on the manifold. When expressed in a coordinate basis, the coefficients that specify the direction of the vector are naturally expressed as functions $b^i$ of the coordinates of the manifold point. Here, the coefficient function $\mathsf{b}$ is more naturally expressed as a tuple-valued function on the manifold. If $b$ is the coefficient function expressed as a function of coordinates, then $\mathsf{b} = b \circ \chi$ is the coefficient function as a function on the manifold.

The coordinate-basis forms have a simple definition in terms of the coordinate-basis vectors and the coordinates (equation 3.40). With this choice, the dual property, equation (3.41), holds without further fuss. More generally, we can define a basis of one-forms $\tilde{\mathsf{e}}$ that is dual to $\mathsf{e}$ in that the property

$$\begin{equation}
\tilde{\mathsf{e}}^i(\mathsf{e}_j)(\mathsf{m}) = \delta^i_j
\end{equation}$$

is satisfied, analogous to property (3.41). Figure 4.1 illustrates the duality of basis fields.

TODO add Figure 4.1 with this caption: Let arrows $\mathsf{e_0}$ and $\mathsf{e_q}$ depict the vectors of a basis vector field at a particular point. Then the foliations shown by the parallel lines depict the dual basis one-form fields at that point. The dotted lines represent the field $\tilde{\mathsf{e}}^0$ and the dashed lines represent the field $\tilde{\mathsf{e}}^1$. The spacings of the lines are 1/3 unit. That the vectors pierce three of the lines representing their duals and do not pierce any of the lines representing the other basis elements is one way to see the relationship $\tilde{\mathsf{e}}^i(\mathsf{e}_j) = \delta^i_j.$

To solve for the dual basis $\tilde{\mathsf{e}}$ given the basis $\mathsf{e}$, we express the basis vectors $\mathsf{e}$ in terms of a coordinate basis#Footnote(2)

$$\begin{equation}
\mathsf{e}_j(\mathsf{f}) = \sum_k {\mathsf{X}(\mathsf{f}) \mathsf{c}_j^k},
\end{equation}$$

and the dual one-forms $\tilde{\mathsf{e}}$ in terms of the dual coordinate one-forms

$$\begin{equation}
\tilde{\mathsf{e}}^i(\mathsf{v}) = \sum_l {\mathsf{d}_l^i \tilde{\mathsf{X}}^l(\mathsf{v})},
\end{equation}$$

then

$$\begin{equation}
\begin{aligned}
\tilde{\mathsf{e}}^{i}\left(\mathsf{e}_{j}\right) &=\sum_{l} \mathsf{d}_{l}^{i} \widetilde{\mathsf{X}}^{l}\left(\mathsf{e}_{j}\right) \\
&=\sum_{l} \mathsf{d}_{l}^{i} \mathsf{e}_{j}\left(\chi^{l}\right) \\
&=\sum_{l} \mathsf{d}_{l}^{i} \sum_{k} \mathsf{X}_{k}\left(\chi^{l}\right) \mathsf{c}_{j}^{k} \\
&=\sum_{k l} \mathsf{d}_{l}^{i} \delta_{k}^{l} \mathsf{c}_{j}^{k} \\
&=\sum_{k} \mathsf{d}_{k}^{i} \mathsf{c}_{j}^{k}.
\end{aligned}
\end{equation}$$

Applying this at $\mathsf{m}$ we get

$$\begin{equation}
\tilde{\mathsf{e}}^i(\mathsf{e}_j)(\mathsf{m}) = \delta_j^i = \sum_k {\mathsf{d}_k^i(\mathsf{m}) \
\mathsf{c}_j^k(\mathsf{m}).}
\end{equation}$$

So the $\mathsf{d}$ coefficients can be determined from the $\mathsf{c}$ coefficents (essentially by matrix inversion).

A set of vector fields $\{\mathsf{e}_i\}$ may be linearly independent in the sense that a weighted sum of them may not be identically zero over a region, yet it may not be a basis in that region. The problem is that there may be some places in the region where the vectors are not independent. For example, two of the vectors may be parallel at a point but not parallel elsewhere in the region.

At such a point $\mathsf{m}$ the determinant of the matrix $\mathsf{c}(\mathsf{m})$ is zero. So at these points we cannot define the dual basis forms.#Footnote(3)

The dual form fields can be used to determine the coefficients $\mathsf{b}$ of a vector field $\mathsf{v}$ relative to a basis $\mathsf{e}$, by applying the dual basis form fields $\tilde{\mathsf{e}}$ to the vector field. Let

$$\begin{equation}
\mathsf{v}(\mathsf{f}) = \sum_i {\mathsf{e}_i(\mathsf{f}) \mathsf{b}^i}.
\end{equation}$$

Then

$$\begin{equation}
\tilde{\mathsf{e}}^j(\mathsf{v}) = \mathsf{b}^j.
\end{equation}$$

Define two general vector fields:

```Scheme
(define e0
  (+ (* (literal-manifold-function 'e0x R2-rect) d/dx)
     (* (literal-manifold-function 'e0y R2-rect) d/dy)))

(define e1
  (+ (* (literal-manifold-function 'e1x R2-rect) d/dx)
     (* (literal-manifold-function 'e1y R2-rect) d/dy)))
```
We use these as a vector basis and compute the dual:
```Scheme
(define e-vector-basis (down e0 e1))
(define e-dual-basis
  (vector-basis->dual e-vector-basis R2-polar))
```
The procedure vector-basis->dual requires an auxiliary coordinate system (here #Code(R2-polar)) to get the $\mathsf{c}_j^k$ coefficient functions from which we compute the $\mathsf{d}_i^k$ coefficient functions. However, the final result is independent of this coordinate system. Then we can verify that the bases $\mathsf{e}$ and $\tilde{\mathsf{e}}$ satisfy the dual relationship (equation 3.41) by applying the dual basis to the vector basis:

```Scheme
((e-dual-basis e-vector-basis) R2-rect-point)
;; (up (down 1 0) (down 0 1))
```

Note that the dual basis was computed relative to the polar coordinate system: the resulting objects are independent of the coordinates in which they were expressed!

Or we can make a general vector field with this basis and then pick out the coefficients by applying the dual basis:

```Scheme
(define v
  (* (up (literal-manifold-function 'b^0 R2-rect)
         (literal-manifold-function 'b^1 R2-rect))
     e-vector-basis))

((e-dual-basis v) R2-rect-point)
;; (up (bˆ0 (up x0 y0)) (bˆ1 (up x0 y0)))
```

### Change of Basis

Suppose that we have a vector field v expressed in terms of one basis $\mathsf{e}$ and we want to reexpress it in terms of another basis $\mathsf{e^\prime}$. We have

$$\begin{equation}
\mathsf{v}(\mathsf{f}) = \sum_i{\mathsf{e}_i(\mathsf{f})\mathsf{b}^i} \
= \sum_i{\mathsf{e^\prime}_j(\mathsf{f})\mathsf{b^\prime}^j}.
\end{equation}$$

The coefficients $\mathsf{b^\prime}$ can be obtained from $\mathsf{v}$ by applying the dual basis

$$\begin{equation}
\mathsf{b^\prime}^j = \mathsf{\tilde{e}^\prime}^j(\mathsf{v}) = \sum_i{\mathsf{\tilde{e}^\prime}^j(\mathsf{e}_i)\mathsf{b}^i}.
\end{equation}$$

Let

$$\begin{equation}
\mathsf{J}_i^j = \mathsf{\tilde{e}^\prime}^j(\mathsf{e}_i),
\end{equation}$$

then

$$\begin{equation}
\mathsf{b^\prime}^j = \sum_i{\mathsf{J}_i^j \mathsf{b}^i},
\end{equation}$$

and

$$\begin{equation}
\mathsf{e}_i(\mathsf{f}) = \sum_j{\mathsf{e^\prime}_j(\mathsf{f})\mathsf{J}_i^j}.
\end{equation}$$

he Jacobian $\mathsf{J}$ is a structure of manifold functions. Using tuple arithmetic, we can write

$$\begin{equation}
\mathsf{b^\prime} = \mathsf{J}\mathsf{b}
\end{equation}$$

and

$$\begin{equation}
\mathsf{e}(\mathsf{f}) = \mathsf{e^\prime}(\mathsf{f})\mathsf{J}.
\end{equation}$$

We can write
```Scheme
(define (Jacobian to-basis from-basis)
(s:map/r (basis->1form-basis to-basis)
        (basis->vector-basis from-basis)))
```
The polar components are:

```Scheme
(define b-polar
(* (Jacobian (coordinate-system->basis R2-polar)
            (coordinate-system->basis R2-rect))
  b-rect))

(b-polar ((point R2-rect) (up 'x0 'y0)))
;; (up
;;  (/ (+ (* x0 (bˆ0 (up x0 y0))) (* y0 (bˆ1 (up x0 y0))))
;;     (sqrt (+ (expt x0 2) (expt y0 2))))
;;  (/ (+ (* x0 (bˆ1 (up x0 y0))) (* -1 y0 (bˆ0 (up x0 y0))))
;;     (+ (expt x0 2) (expt y0 2))))
   ```

We can also get the polar components directly:

```Scheme
(((coordinate-system->1form-basis R2-polar)
(literal-vector-field 'b R2-rect))
((point R2-rect) (up 'x0 'y0)))

;; (up
;;  (/ (+ (* x0 (bˆ0 (up x0 y0))) (* y0 (bˆ1 (up x0 y0))))
;;     (sqrt (+ (expt x0 2) (expt y0 2))))
;;  (/ (+ (* x0 (bˆ1 (up x0 y0))) (* -1 y0 (bˆ0 (up x0 y0))))
;;     (+ (expt x0 2) (expt y0 2))))
```

We see that they are the same.

If $\mathsf{K}$ is the Jacobian that relates the basis vectors in the other direction

$$\begin{equation}
\mathsf{e^\prime}(\mathsf{f}) = \mathsf{e}(\mathsf{f})\mathsf{K}
\end{equation}$$

then

$$\begin{equation}
\mathsf{K}\mathsf{J} = \mathsf{I} = \mathsf{J}\mathsf{K}
\end{equation}$$

where $\mathsf{I}$ is a manifold function that returns the multiplicative identity.

The dual basis transforms oppositely. Let

$$\begin{equation}
\boldsymbol{\omega} = \sum_i{\mathsf{a}_i \tilde{\mathsf{e}}^{i}} = \sum_i{\mathsf{a}^{\prime}_i \tilde{\mathsf{e}}^{\prime i}}.
\end{equation}$$

The coefficients are#Footnote(4)

$$\begin{equation}
\mathsf{a}_i = \boldsymbol{\omega}(\mathsf{e}_i) = \sum_j{\mathsf{a}^\prime_j \tilde{\mathsf{e}}^{\prime j}}(\mathsf{e}_i) \
= \sum_j{\mathsf{a}^\prime_j \mathsf{J}^j_i}
\end{equation}$$

or, in tuple arithmetic,

$$\begin{equation}
\mathsf{a} = \mathsf{a}^\prime \mathsf{J}.
\end{equation}$$

Because of equation (4.18) we can deduce

$$\begin{equation}
\tilde{\mathsf{e}} = \mathsf{K}\tilde{\mathsf{e}}^\prime.
\end{equation}$$

### Rotation Basis

One interesting basis for rotations in 3-dimensional space is not a coordinate basis.

Rotations are the actions of the special orthogonal group SO(3), which is a 3-dimensional manifold. The elements of this group may be represented by the set of $3 \times 3$ orthogonal matrices with determinant $+1$.

We can use a coordinate patch on this manifold with Euler angle coordinates: each element has three coordinates, $\theta$, $\phi$, $\psi$. A manifold point may be represented by a rotation matrix. The rotation matrix for Euler angles is a product of three simple rotations: $M(\theta, \phi, \psi) = R_z(\phi)R_x(\theta)R_z(\psi)$, where $R_x$ and $R_z$ are functions that take an angle and produce the matrices representing rotations about the $x$ and $z$ axes, respectively. We can visualize $\theta$ as the colatitude of the pole from the $\hat{z}$-axis, $\phi$ as the longitude, and $\psi$ as the rotation around the pole.

Given a rotation specified by Euler angles, how do we change the Euler angle to correspond to an incremental rotation of size $\epsilon$ about the $\hat{x}$-axis? The direction $(a, b, c)$ is constrained by the equation

$$\begin{equation}
R_{x}(\epsilon) M(\theta, \phi, \psi)=M(\theta + a \epsilon, \phi + b \epsilon, \psi + c \epsilon).
\end{equation}$$

Linear equations for $(a, b, c)$ can be found by taking the derivative of this equation with respect to $\epsilon$. We find

$$\begin{equation}
0 = c \cos{\theta} + b,
\end{equation}$$

$$\begin{equation}
0 = a \sin{\phi} - c \cos{\phi} \sin{\theta},
\end{equation}$$

$$\begin{equation}
1 = c \sin{\phi} \sin{\theta} + a \cos{\phi},
\end{equation}$$

with the solution

$$\begin{equation}
a = \cos{\phi},
\end{equation}$$

$$\begin{equation}
b = -\frac{\sin{\phi} \cos{\theta}}{\sin{\theta}},
\end{equation}$$

$$\begin{equation}
c = \frac{\sin{\phi}}{\sin{\theta}}.
\end{equation}$$

Therefore, we can write the basis vector field that takes directional derivatives in the direction of incremental $x$ rotations as

$$\begin{equation}
\begin{aligned}
\mathsf{e}_{x} &=a \frac{\partial}{\partial \theta}+b \frac{\partial}{\partial \phi}+c \frac{\partial}{\partial \psi} \\
&=\cos \phi \frac{\partial}{\partial \theta}-\frac{\sin \phi \cos \theta}{\sin \theta} \frac{\partial}{\partial \phi}+\frac{\sin \phi}{\sin \theta} \frac{\partial}{\partial \psi} .
\end{aligned}
\end{equation}$$

Similarly, vector fields for the incremental y and z rotations are

$$\begin{equation}
\mathsf{e}_{y}=\frac{\cos \phi \cos \theta}{\sin \theta} \frac{\partial}{\partial \phi}+\sin \phi \frac{\partial}{\partial \theta}-\frac{\cos \phi}{\sin \theta} \frac{\partial}{\partial \psi}
\end{equation}$$

$$\begin{equation}
\mathsf{e}_{z} = \frac{\partial}{\partial \phi}.
\end{equation}$$

### Commutators

The commutator of two vector fields is defined as

$$\begin{equation}
[\mathsf{v}, \mathsf{w}](\mathsf{f}) = \mathsf{v}(\mathsf{w}(\mathsf{f})) - \mathsf{w}(\mathsf{v}(\mathsf{f})).
\end{equation}$$

In the special case that the two vector fields are coordinate basis fields, the commutator is zero:

$$\begin{equation}
\begin{aligned}
\left[\mathsf{X}_{i}, \mathsf{X}_{j}\right](\mathsf{f}) &=\mathsf{X}_{i}\left(\mathsf{X}_{j}(\mathsf{f})\right)-\mathsf{X}_{j}\left(\mathsf{X}_{i}(\mathsf{f})\right) \\
&=\partial_{i} \partial_{j}\left(\mathsf{f} \circ \chi^{-1}\right) \circ \chi-\partial_{j} \partial_{i}\left(\mathsf{f} \circ \chi^{-1}\right) \circ \chi \\
&=0,
\end{aligned}
\end{equation}$$

because the individual partial derivatives commute. The vanishing commutator is telling us that we get to the same manifold point by integrating from a point along first one basis vector field and then another as from integrating in the other order. If the commutator is zero we can use the integral curves of the basis vector fields to form a coordinate mesh.

More generally, the commutator of two vector fields is a vector field. Let $\mathsf{v}$ be a vector field with coefficient function $\mathsf{c} = c \circ \chi$, and $\mathsf{u}$ be a vector field with coefficient function $\mathsf{b} = b \circ \chi$, both with respect to the coordinate basis $\mathsf{X}$. Then

$$\begin{equation}
\begin{aligned}
[\mathsf{u}, \mathsf{v}](\mathsf{f})=& \mathsf{u}(\mathsf{v}(\mathsf{f}))-\mathsf{v}(\mathsf{u}(\mathsf{f})) \\
=& \mathsf{u}\left(\sum_{i} \mathsf{X}_{i}(\mathsf{f}) \mathsf{c}^{i}\right)-\mathsf{v}\left(\sum_{j} \mathsf{X}_{j}(\mathsf{f}) \mathsf{b}^{j}\right) \\
=& \sum_{j} \mathsf{X}_{j}\left(\sum_{i} \mathsf{X}_{i}(\mathsf{f}) \mathsf{c}^{i}\right) \mathsf{b}^{j}-\sum_{i} \mathsf{X}_{i}\left(\sum_{j} \mathsf{X}_{j}(\mathsf{f}) \mathsf{b}^{j}\right) \mathsf{c}^{i} \\
=& \sum_{i j}\left[\mathsf{X}_{j}, \mathsf{X}_{i}\right](\mathsf{f}) \mathsf{c}^{i} \mathsf{~b}^{j} \\
&+\sum_{i} \mathsf{X}_{i}(\mathsf{f}) \sum_{j}\left(\mathsf{X}_{j}\left(\mathsf{c}^{i}\right) \mathsf{b}^{j}-\mathsf{X}_{j}\left(\mathsf{~b}^{i}\right) \mathsf{c}^{j}\right) \\
=& \sum_{i} \mathsf{X}_{i}(\mathsf{f}) \mathsf{a}^{i},
\end{aligned}
\end{equation}$$

where the coefficient function $\mathsf{a}$ of the commutator vector field is

$$\begin{equation}
\begin{aligned}
\mathsf{a}^i &= \sum_j \left(\mathsf{X}_j \left( \mathsf{c}^i \right) \mathsf{b}^j \
- \mathsf{X}_j \left(\mathsf{b}^i \right) \mathsf{c}^j \right) \\
&= \mathsf{u} \left(\mathsf{c}^i \right) - \mathsf{v} \left(\mathsf{b}^i \right).
\end{aligned}
\end{equation}$$

We used the fact, shown above, that the commutator of two coordinate basis fields is zero.

We can check this formula for the commutator for the general vector fields #Code(e0) and #Code(e1) in polar coordinates:

```Scheme
(let* ((polar-basis (coordinate-system->basis R2-polar))
 (polar-vector-basis (basis->vector-basis polar-basis))
 (polar-dual-basis (basis->1form-basis polar-basis))
 (f (literal-manifold-function 'f-rect R2-rect)))
((- ((commutator e0 e1) f)
(* (- (e0 (polar-dual-basis e1))
      (e1 (polar-dual-basis e0)))
   (polar-vector-basis f)))
R2-rect-point))
;; 0
```

Let $\mathsf{e}$ be a tuple of basis vector fields. The commutator of two basis fields can be expressed in terms of the basis vector fields:

$$\begin{equation}
[\mathsf{e}_i, \mathsf{e}_j](\mathsf{f}) = \sum_k{\mathsf{d}_{ij}^k \mathsf{e}_k(\mathsf{f})},
\end{equation}$$

where $\mathsf{d}_{ij}^k$ are functions of $\mathsf{m}$, called the /structure constants/ for the basis vector fields. The coefficients are

$$\begin{equation}
\mathsf{d}_{ij}^k = \tilde{\mathsf{e}}^k\left(\left[\mathsf{e}_i, \mathsf{e}_j \right]\right).
\end{equation}$$

The commutator $[\mathsf{u}, \mathsf{v}]$ with respect to a non-coordinate basis $\mathsf{e}_i$ is

$$\begin{equation}
[\mathsf{u}, \mathsf{v}](\mathsf{f}) = \sum_k{\mathsf{e}_k \left(\mathsf{f} \right)\left( \
\mathsf{u}(\mathsf{c}^k) - \mathsf{v}(\mathsf{b}^k) + \sum_{ij}{\mathsf{c}^i \mathsf{b}^j \mathsf{d}_{ji}^k} \
\right)}
\end{equation}$$

Define the vector fields #Code(Jx), #Code(Jy), and #Code(Jz) that generate rotations about the three rectangular axes in three dimensions:#Footnote(5)

```Scheme
(define Jz (- (* x d/dy) (* y d/dx)))
(define Jx (- (* y d/dz) (* z d/dy)))
(define Jy (- (* z d/dx) (* x d/dz)))
```

```Scheme
(((+ (commutator Jx Jy) Jz) g) R3-rect-point)
;; 0
```

```Scheme
(((+ (commutator Jy Jz) Jx) g) R3-rect-point)
;; 0
```

```Scheme
(((+ (commutator Jz Jx) Jy) g) R3-rect-point)
;; 0
```

We see that

$$\begin{equation}
\begin{aligned}
\left[\mathsf{J}_x, \mathsf{J}_y \right] &= -\mathsf{J}_z \\
\left[\mathsf{J}_y, \mathsf{J}_z \right] &= -\mathsf{J}_x \\
\left[\mathsf{J}_z, \mathsf{J}_x \right] &= -\mathsf{J}_y
\end{aligned}
\end{equation}$$

We can also compute the commutators for the basis vector fields
$\mathsf{e}_x$, $\mathsf{e}_y$, and $\mathsf{e}_z$ in the SO(3) manifold (see equations 4.29–4.31) that correspond to rotations about the $x$, $y$, and $z$ axes, respectively:#Footnote(6)

```Scheme
(((+ (commutator e x e y) e z) f) SO3-point)
;; 0
```

```Scheme
(((+ (commutator e y e z) e x) f) SO3-point)
;; 0
```

```Scheme
(((+ (commutator e z e x) e y) f) SO3-point)
;; 0
```

You can tell if a set of basis vector fields is a coordinate basis by calculating the commutators. If they are nonzero, then the basis is not a coordinate basis. If they are zero then the basis vector fields can be integrated to give the coordinate system.

Recall equation (3.31)

$$\begin{equation}
\left(e^{t \mathsf{v}} \right)\left(\mathsf{m} \right) \
= \left(\mathsf{f} \circ \phi_t^{\mathsf{v}} \right)\left(\mathsf{m} \right).
\end{equation}$$

Iterating this equation, we find

$$\begin{equation}
\left(e^{s \mathsf{w}} e^{t \mathsf{v}} \right)\left(\mathsf{m} \right) \
= \left(\mathsf{f} \circ \phi_t^{\mathsf{v}} \circ \phi_s^{\mathsf{w}} \right)\left(\mathsf{m} \right).
\end{equation}$$

Notice that the evolution under $\mathsf{w}$ occurs before the evolution under $\mathsf{v}$.

To illustrate the meaning of the commutator, consider the evolution around a small loop with sides made from the integral curves of two vector fields $\mathsf{v}$ and $\mathsf{w}$. We will first follow $\mathsf{v}$, then $\mathsf{w}$, then $−\mathsf{v}$, and then $−\mathsf{w}$:

$$\begin{equation}
\left(e^{\epsilon \mathsf{v}} e^{\epsilon \mathsf{w}} \
e^{-\epsilon \mathsf{v}} e^{-\epsilon \mathsf{w}} \mathsf{f} \right)\left(\mathsf{m}\right).
\end{equation}$$

To second order in $\epsilon$ the result is#Footnote(7)

$$\begin{equation}
\left(e^{\epsilon^2 [\mathsf{v}, \mathsf{w}]} \mathsf{f} \right)\left(\mathsf{m}\right)
\end{equation}$$

This result is illustrated in figure 4.2.

Take a point $\mathsf{0}$ in $\mathsf{M}$ as the origin. Then, presuming $[\mathsf{e}_i, \mathsf{e}_j] = 0$, the coordinates $x$ of the point $\mathsf{m}$ in the coordinate system corresponding to the $\mathsf{e}$ basis satisfy#Footnote(8)

$$\begin{equation}
\mathsf{m} = \phi_1^{x \mathsf{e}}(\mathsf{0}) = \chi^{-1}(x),
\end{equation}$$

where $\chi$ is the coordinate function being defined. Because the elements of $\mathsf{e}$ commute, we can translate separately along the integral curves in any order and reach the same point; the terms in the exponential can be factored into separate exponentials if needed.

### Exercise 4.1: Alternate Angles

Note that the Euler angles are singular at $\theta = 0$ (where $\phi$ and
$\psi$ become degenerate), so the representations of $\mathsf{e}_x$,
$\mathsf{e}_y$, and $\mathsf{e}_z$ (defined in equations 4.29–4.31) have problems there. An alternate coordinate system avoids this problem, while introducing a similar problem elsewhere in the manifold. Consider the "alternate angles" $(\theta_a, \phi_a, \psi_a)$ which define a rotation matrix via $M(\theta_a, \phi_a, \psi_a) = R_z(\phi_a) R_x(\theta_a)
R_y(\psi_a)$.

*a.* Where does the singularity appear in these alternate coordinates? Do you think you could define a coordinate system for rotations that has no singularities?

*b.* What do the $\mathsf{e}_x$, $\mathsf{e}_y$, and $\mathsf{e}_z$ basis vector fields look like in this coordinate system?

### Exercise 4.2: General Commutators

Verify equation (4.38).

### Exercise 4.3: SO(3) Basis and Angular Momentum Basis

How are $\mathsf{J}_x$, $\mathsf{J}_y$, and $\mathsf{J}_z$ related to $\mathsf{e}_x$, $\mathsf{e}_y$, and $\mathsf{e}_z$ in equations (4.29–4.31)?

----
### Footnotes


#FootnoteRef(1) We cannot say if the basis vectors are orthogonal or normalized until we introduce a metric.

#FootnoteRef(2) We write the vector components on the right and the tuple of basis vectors on the left because if we think of the basis vectors as organized as a row and the components as organized as a column then the formula is just a matrix multiplication.

#FootnoteRef(3) This is why the set of vector fields and the set of one-form fields are modules rather than vector spaces.

#FootnoteRef(4) We see from equations (4.15) and (4.16) that $\mathsf{J}$ and $\mathsf{K}$ are inverses. We can obtain their coefficients by: $\mathsf{J}_i^j = \tilde{\mathsf{e}}^{\prime j}(\mathsf{e}_i)$ and $\mathsf{K}_i^j = \tilde{\mathsf{e}}^j(\mathsf{e}_i^\prime)$.

#FootnoteRef(5) Using
```Scheme
(define R3-rect (coordinate-system-at 'rectangular 'origin R3))
(define-coordinates (up x y z) R3-rect)
(define R3-rect-point ((point R3-rect) (up 'x0 'y0 'z0)))
(define g (literal-manifold-function 'g-rect R3-rect))
```

#FootnoteRef(6) Using
```Scheme
(define Euler-angles (coordinate-system-at 'Euler 'Euler-patch SO3))
(define Euler-angles-chi-inverse (point Euler-angles))
(define-coordinates (up theta phi psi) Euler-angles)
(define SO3-point ((point Euler-angles) (up 'theta 'phi 'psi)))
(define f (literal-manifold-function 'f-Euler Euler-angles))
```

#FootnoteRef(7) For non-commuting operators $A$ and $B$,

$$\begin{equation}
\begin{align*}
e^{A} e^{B} e^{-A} e^{-B} & \\
=&\left(1+A+\frac{A^{2}}{2}+\cdots\right)\left(1+B+\frac{B^{2}}{2}+\cdots\right) \\
& \times\left(1-A+\frac{A^{2}}{2}+\cdots\right)\left(1-B+\frac{B^{2}}{2}+\cdots\right) \\
=& 1+[A, B]+\cdots,
\end{align*}
\notag
\end{equation}$$

to second order in $A$ and $B$. All higher-order terms can be written in terms of higher-order commutators of $A$ and $B$. An example of a higher-order commutator is $[A, [A, B]]$.

#FootnoteRef(8) Here $x$ is an up-tuple structure of components, and $\mathsf{e}$ is down-tuple structure of basis vectors. The product of the two contracts to make a scaled vector, along which we translate by one unit.

#FootnoteEnd