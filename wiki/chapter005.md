!!Polyglot
#page(0)

# Chapter 5: Integration

We know how to integrate real-valued functions of a real variable. We want to extend this idea to manifolds, in such a way that the integral is independent of the coordinate system used to compute it.

The integral of a real-valued function of a real variable is the limit of a sum of products of the values of the function on subintervals and the lengths of the increments of the independent variable in those subintervals:

$$\begin{equation}
\int_{a}^{b} f = \int_{a}^{b} f(x)dx = \lim _{\Delta x_{i} \rightarrow 0}
\sum_{i} f(x_{i}) \Delta x_{i}
\end{equation}$$

If we change variables $(x = g(y))$, then the form of the integral changes:

$$\begin{equation}
\begin{aligned}
\int_{a}^{b} f &= \int_{a}^{b} f(x)dx \\
&= \int_{g^{-1}(a)}^{g^{-1}(b)} f(g(y)) Dg(y)dy \\
&= \int_{g^{-1}(a)}^{g^{-1}(b)} (f \circ g)Dg
\end{aligned}
\end{equation}$$

We can make a coordinate-independent notion of integration in the following way. An interval of the real line is a 1-dimensional manifold with boundary. We can assign a coordinate chart χ to this manifold. Let $x = \chi(\mathsf{m})$. The coordinate basis is associated with a coordinate-basis vector field, here ${\partial}/{\partial \mathsf{x}}$. Let ω be a one-form on this manifold. The application of ω to ${\partial}/{\partial \mathsf{x}}$ is a real-valued function on the manifold. If we compose this with the inverse chart, we get a real-valued function of a real variable. We can then write the usual integral of this function

$$\begin{equation}
I = \int_{a}^{b} \omega ({\partial}/{\partial \mathsf{x}}) \circ \chi^{-1}
\end{equation}$$

It turns out that the value of this integral is independent of the coordinate chart used in its definition. Consider a different coordinate chart $x\prime = \chi\prime (\mathsf{m})$, with associated basis vector field ${\partial}/{\partial x\prime}$. Let $g = \chi\prime \circ \chi^{-1}$. We have

$$\begin{equation}
\begin{aligned}
\int_{a^{\prime}}^{b^{\prime}} & \boldsymbol{\omega}\left(\partial / \partial \mathrm{x}^{\prime}\right) \circ \chi^{\prime-1} \\
&=\int_{a^{\prime}}^{b^{\prime}} \boldsymbol{\omega}\left(\partial / \partial \mathrm{x}\left(D\left(\chi \circ \chi^{\prime-1}\right) \circ \chi^{\prime}\right)\right) \circ \chi^{\prime-1} \\
&=\int_{a^{\prime}}^{b^{\prime}}\left(\boldsymbol{\omega}(\partial / \partial \mathrm{x}) D\left(\chi \circ \chi^{\prime-1}\right) \circ \chi^{\prime}\right) \circ \chi^{\prime-1} \\
&=\int_{a^{\prime}}^{b^{\prime}}\left(\boldsymbol{\omega}(\partial / \partial \mathrm{x}) \circ \chi^{\prime-1}\right) D\left(\chi \circ \chi^{\prime-1}\right) \\
&=\int_{a}^{b}\left(\left(\left(\boldsymbol{\omega}(\partial / \partial \mathrm{x}) \circ \chi^{-1}\right) D\left(\chi \circ \chi^{\prime-1}\right)\right) \circ g\right) D g \\
&=\int_{a}^{b} \boldsymbol{\omega}(\partial / \partial \mathrm{x}) \circ \chi^{-1},
\end{aligned}
\end{equation}$$

where we have used the rule for coordinate transformations of basis vectors (equation 3.19), linearity of forms in the first two lines, and the rule for change-of-variables under an integral in the last line.#Footnote(1)

Because the integral is independent of the coordinate chart, we can write simply

$$\begin{equation}
I = \int_{\mathsf{M}} \omega ,
\end{equation}$$

where $\mathsf{M}$ is the 1-dimensional manifold with boundary corresponding to the interval.

We are exploiting the fact that coordinate basis vectors in different coordinate systems are related by a Jacobian (see equation 3.19), which cancels the Jacobian that appears in the change-of-variables formula for integration (see equation 5.2).

###  Higher Dimensions

We have seen that we can integrate one-forms on 1-dimensional manifolds. We need higher-rank forms that we can integrate on higher-dimensional manifolds in a coordinate-independent manner.

Consider the integral of a real-valued function, $\mathsf{f} : \mathsf{R}^{n}
\to \mathsf{R}$, over a region $\mathsf{U}$ in $\mathsf{R}^{n}$. Under a coordinate transformation $g : \mathsf{R}^{n} \to \mathsf{R}^{n}$, we have#Footnote(2)

$$\begin{equation}
\int_{\mathsf{U}} \mathsf{f} = \int_{g^{-1}(\mathsf{U})} (\mathsf{f} \circ g) \det (Dg).
\end{equation}$$

A rank $n$ form field takes $n$ vector field arguments and produces a real-valued manifold function: $\omega (\mathsf{v}, \mathsf{w}, \dots, \mathsf{u})(\mathsf{m})$. By analogy with the 1-dimensional case, higher-rank forms are linear in each argument. Higher-rank forms must also be antisymmetric under interchange of any two arguments in order to make a coordinate-free definition of integration analogous to equation (5.3).

Consider an integral in the coordinate system χ:

$$\begin{equation}
\int_{\chi (\mathsf{U})} \omega (\mathsf{X}_{0}, \mathsf{X}_{1}, \dots) \circ \chi^{-1}.
\end{equation}$$

Under coordinate transformations $g = \chi \circ \chi\prime^{-1}$, the integral becomes

$$\begin{equation}
\int_{\chi^{\prime}(\mathsf{U})}{\boldsymbol{\omega}(\mathsf{X}_{0}, \mathsf{X}_1, \dots) \circ \chi^{\prime-1} \det (Dg)}.
\end{equation}$$

Using the change-of-basis formula, equation (3.19):

$$\begin{equation}
\mathsf{X} (\mathsf{f}) = \mathsf{X}^{\prime} (\mathsf{f}) (D(\chi^{\prime} \circ \chi^{-1})) \circ \chi = \mathsf{X}^{\prime}(\mathsf{f})(D(g^{-1}) \circ \chi .
\end{equation}$$

If we let $M = (D(g^{-1})) \circ \chi$ then

$$\begin{equation}
\begin{aligned}
(\omega (\mathsf{X}_{0}, &\mathsf{X}_{1}, \dots) \circ \chi^{\prime-1}) \det(Dg) \\
&= (\omega (\mathsf{X}^{\prime} M_{0}, \mathsf{X}^{\prime}M_{1}, \dots) \circ \chi^{\prime-1}) \det(Dg) \\
&= (\omega(\mathsf{X}^{\prime}_{0}, \mathsf{X}^{\prime}_{1}, \dots) \circ \chi^{\prime-1}) \alpha (M_{0}, M_{1}, \dots) \det(Dg),
\end{aligned}
\end{equation}$$

using the multilinearity of $\boldsymbol{\omega}$, where $M_{i}$ is the ${i}^{\text{th}}$ column of $M$. The function $\alpha$ is multilinear in the columns of $M$. To make a coordinate-independent integration we want the expression (5.10) to be the same as the integrand in

$$\begin{equation}
I^{\prime} = \int_{\chi^{\prime}(\mathsf{U})} \omega(\mathsf{X}^{\prime}_{0}, \mathsf{X}^{\prime}_{1}, \dots) \circ \chi^{\prime-1}.
\end{equation}$$

For this to be the case, $\alpha (M_{0}, M_{1}, \dots)$ must be
$(\det(D(g))^{-1} = \det(M)$. So α is an antisymmetric function, and thus so is $ω$.

Thus higher-rank form fields must be antisymmetric multilinear functions from vector fields to manifold functions. So we have a coordinate-independent definition of integration of form fields on a manifold and we can write

$$\begin{equation}
I = I^{\prime} = \int_{\mathsf{U}} \omega
\end{equation}$$

### Wedge Product

There are several ways we can construct antisymmetric higher-rank forms. Given two one-form fields ω and τ we can form a two-form field $\omega \wedge \tau$ as follows:

$$\begin{equation}
(\omega \wedge \tau)(\mathsf{v}, \mathsf{w}) = \omega(\mathsf{v})\tau(\mathsf{w}) - \omega(\mathsf{w})\tau(\mathsf{v}).
\end{equation}$$

More generally we can form the wedge of higher-rank forms. Let ω be a $k$-form field and τ be an $l$-form field. We can form a $(k+l)$-form field $\omega \wedge \tau$ as follows:

$$\begin{equation}
\omega \wedge \tau = \frac{(k+l)!}{k!l!} \text{Alt}(\omega \otimes \tau)
\end{equation}$$

where, if $η$ is a function on $m$ vectors,

$$\begin{equation}
\begin{aligned}
\text{Alt}(\eta) (\mathsf{v}_{0}, \dots, \mathsf{v}_{m-1}) \\
&= \frac{1}{m!} \sum_{\sigma \epsilon \text{Perm} (m)} \text{Parity} (\sigma) \eta (\mathsf{v}_{\sigma(0)}, \dots, \mathsf{v}_{\sigma (m-1)}),
\end{aligned}
\end{equation}$$

and where

$$\begin{equation}
\begin{aligned}
\omega \otimes \tau &(\mathsf{v}_{0}, \dots, \mathsf{v}_{k-1}, \mathsf{v}_{k}, \dots, \mathsf{v}_{k+l-1}) \\
&= \omega (\mathsf{v}_{0}, \dots, \mathsf{v}_{k-1}) \tau (\mathsf{v}_{k}, \dots, \mathsf{v}_{k+l-1}).
\end{aligned}
\end{equation}$$

The wedge product is associative, and thus we need not specify the order of a multiple application. The factorial coefficients of these formulas are chosen so that

$$\begin{equation}
(\mathsf{d}\mathsf{x} \wedge \mathsf{d}\mathsf{y} \wedge \dots) ({\partial}/{\partial\mathsf{x}}), {\partial}/{\partial\mathsf{y}}, \dots) = 1.
\end{equation}$$

This is true independent of the coordinate system.

Equation (5.17) gives us

$$\begin{equation}
\int_{\mathsf{U}} \mathsf{d}\mathsf{x} \wedge \mathsf{d}\mathsf{y} \wedge \ldots = \text{Volume}(\mathsf{U})
\end{equation}$$

where $\text{Volume}(\mathsf{U})$ is the ordinary volume of the region corresponding to $\mathsf{U}$ in the Euclidean space of $\mathsf{R}^{n}$ with the orthonormal coordinate system $(x, y, \ldots)$.#Footnote(3)

An example two-form (see figure 5.1) is the oriented area of a parallelogram in the $(x,y)$ coordinate plane at the point $\mathsf{m}$ spanned by two vectors $\mathsf{u} = \mathsf{u}^{0}{\partial}/{\partial\mathsf{x}} +
\mathsf{u}^{1}{\partial}/{\partial\mathsf{y}}$ and $\mathsf{v} =
{\mathsf{v}^{0}{\partial}/{\partial\mathsf{x}} +
\mathsf{v}^{1}}{\partial}/{\partial\mathsf{y}},$ which is given by

$$\begin{equation}
\mathsf{A} (\mathsf{u}, \mathsf{v}) (\mathsf{m}) = \mathsf{u}^{0} (\mathsf{m}) \mathsf{v}^{1} - \mathsf{v}^{0} (\mathsf{m}) \mathsf{u}^{1} (\mathsf{m}).
\end{equation}$$

Note that this is the area of the parallelogram in the coordinate plane, which is the range of the coordinate function. It is not the area on the manifold. To define that, we need more structure — the metric. We will put a metric on the manifold in Chapter 9.

### 3-Dimensional Euclidean Space

Let's specialize to 3-dimensional Euclidean space. Following equation (5.18) we can write the coordinate-area two-form in another way: $\mathsf{A} = \mathsf{d}\mathsf{x} \wedge \mathsf{d}\mathsf{y}.$ As code:

```Scheme
(define-coordinates (up x y z) R3-rect)

(define u (+ (* 'u^0 d/dx) (* 'u^1 d/dy)))
(define v (+ (* 'v^0 d/dx) (* 'v^1 d/dy)))
```

```Scheme
(((wedge dx dy) u v) R3-rect-point)
;; (+ (* u^0 v^1) (* -1 u^1 v^0))
```

If we use cylindrical coordinates and define cylindrical vector fields we get the analogous answer in cylindrical coordinates:

```Scheme
(define-coordinates (up r theta z) R3-cyl)

(define a (+ (* 'a^0 d/dr) (* 'a^1 d/dtheta)))
(define b (+ (* 'b^0 d/dr) (* 'b^1 d/dtheta)))
```

```Scheme
(((wedge dr dtheta) ab) ((point R3-cyl) (up 'r0 'theta0 'z0)))
;; (+ (* a^0 b^1 ) (* -1 a^1 b^0))
```

The moral of this story is that this is the area of the parallelogram in the coordinate plane. It is not the area of the manifold!

There is a similar story with volumes. The wedge product of the elements of the coordinate basis is a three-form that measures our usual idea of coordinate volumes in $\mathsf{R}^{3}$ with a Euclidean metric:

```Scheme
(define u (+ (* 'u^0 d/dx) (* 'u^1 d/dy) (* 'u^2 d/dz)))
(define v (+ (* 'v^0 d/dx) (* 'v^1 d/dy) (* 'v^2 d/dz)))
(define w (+ (* 'w^0 d/dx) (* 'w^1 d/dy) (* 'w^2 d/dz)))
```

```Scheme
(((wedge dx dy dz) u v w) R3-rect-point)
;; (+ (* u^0 v^1 w^2)
;;    (* -1 u^0 v^2 w^1)
;;    (* -1 u^1 v^0 w^2)
;;    (* u^1 v^2 w^0)
;;    (* u^2 v^0 w^1)
;;    (* -1 u^2 v^1 w^0))
```

This last expression is the determinant of a $3 \times 3$ matrix:

```Scheme
(- (((wedge dx dy dz) u v w) R3-rect-point)
   (determinant
    (matrix-by-rows (list 'u^0 'u^1 'u^2)
                    (list 'v^0 'v^1 'v^2)
                    (list 'w^0 'w^1 'w^2))))
;; 0
```

If we did the same operations in cylindrical coordinates we would get the analogous formula, showing that what we are computing is volume in the coordinate space, not volume on the manifold.

Because of antisymmetry, if the rank of a form is greater than the dimension of the manifold then the form is identically zero. The $k$-forms on an $n$-dimensional manifold form a module of dimension $\binom{n}{k}$. We can write a coordinate-basis expression for a $k$-form as

$$\begin{equation}
\omega = \sum_{{i}_{0}, \ldots, {i}_{k-1}}^{n} \omega_{{i}_{0}, \ldots, {i}_{k-1}} \mathsf{d}\mathsf{x}^{{i}_{0}} \wedge \ldots \wedge \mathsf{d}\mathsf{x}^{i_{k-1}}.
\end{equation}$$

$$\begin{equation}
\omega = \sum_{{i}_{0}, \ldots, {i}_{k-1}}^{n}.
\end{equation}$$


The antisymmetry of the wedge product implies that

$$\begin{equation}
\omega_{{i}_{\sigma(0)}, \ldots, {i}_{\sigma (k-1)}} = \text{Parity}(\sigma)\omega_{i_{0}, \ldots, {i}_{k-1}},
\end{equation}$$

from which we see that there are only $\binom{n}{k}$ independent components of $ω$.

### Exercise 5.1: Wedge Product

Pick a coordinate system and use the computer to verify that

a. the wedge product is associative for forms in your coordinate system;
b. formula (5.17) is true in your coordinate system.

### Exterior Derivative

The intention of introducing the exterior derivative is to capture all of the classical theorems of "vector analysis" into one unified Stokes's Theorem, which asserts that the integral of a form on the boundary of a manifold is the integral of the exterior derivative of the form on the interior of the manifold:#Footnote(4)

$$\begin{equation}
\int_{\partial\mathsf{M}} \omega = \int_{\mathsf{M}} \mathsf{d} \omega .
\end{equation}$$

As we have seen in equation (3.34), the differential of a function on a manifold is a one-form field. If a function on a manifold is considered to be a form field of rank zero,#Footnote(5) then the differential operator increases the rank of the form by one. We can generalize this to $k$-form fields with the exterior derivative operation.

Consider a one-form ω. We define#Footnote(6)

$$\begin{equation}
\mathsf{d}\omega (\mathsf{v}_{1}, \mathsf{v}_{2}) = \mathsf{v}_{1} (\omega (\mathsf{v}_{2})) - \mathsf{v}_{2} (\omega(\mathsf{v}_{1})) - \omega([\mathsf{v}_{1}, \mathsf{v}_{2}]).
\end{equation}$$

More generally, the exterior derivative of a $k$-form field is a $k+1$-form field, given by:#Footnote(7)

$$\begin{equation}
\begin{aligned}
\mathsf{d} \omega & (\mathsf{v}_{0}, \ldots, \mathsf{v}_{k}) = \\
& \sum_{i=0}^{k} \bigg\{  ((-1)^{i} \mathsf{v}_{i}(\omega (\mathsf{v}_{0}, \ldots, \mathsf{v}_{i-1}, \mathsf{v}_{i+1}, \ldots, \mathsf{v}_{k}))+ \\
& \left. \sum_{j=i+1}^{k} (-1)^{i+j} \omega (\mathsf{v}_{i}, \mathsf{v}_{j}], \mathsf{v}_{0}, \ldots, \mathsf{v}_{i-1}, \mathsf{v}_{i+1}, \ldots, \mathsf{v}_{j-1}, \mathsf{v}_{j+1}, \ldots, \mathsf{v}_{k})) \right\}.
\end{aligned}
\end{equation}$$

This formula is coordinate-system independent. This is the way we compute the exterior derivative in our software.

If the form field ω is represented in a coordinate basis

$$\begin{equation}
\omega = \sum_{{i}_{0}=0, \ldots, {i}_{k-1}=0}^{n-1} \mathsf{a}_{{i}_{{0}}, \ldots, {{i}_{k-1}}} \mathsf{d}\mathsf{x}^{{i}_{0}} \wedge \ldots \wedge \mathsf{d}\mathsf{x}^{{i}_{k-1}}
\end{equation}$$

then the exterior derivative can be expressed as

$$\begin{equation}
\mathsf{d}\omega = \sum_{{i}_{0}=0, \ldots, {i}_{k-1}=0}^{n-1} \mathsf{d}\mathsf{a}_{{i}_{{0}}, \ldots, {{i}_{k-1}}} \mathsf{d}\mathsf{x}^{{i}_{0}} \wedge \ldots \wedge \mathsf{d}\mathsf{x}^{{i}_{k-1}}.
\end{equation}$$

Though this formula is expressed in terms of a coordinate basis, the result is independent of the choice of coordinate system.

### Computing Exterior Derivatives

We can test that the computation indicated by equation (5.24) is equivalent to the computation indicated by equation (5.26) in three dimensions with a general one-form field:

```Scheme
(define a (literal-manifold-function 'alpha R3-rect))
(define b (literal-manifold-function 'beta R3-rect))
(define c (literal-manifold-function 'gamma R3-rect))

(define theta (+ (* a dx) (* b dy) (* c dz)))
```

The test will require two arbitrary vector fields

```Scheme
(define X (literal-vector-field 'X-rect R3-rect))
(define Y (literal-vector-field 'Y-rect R3-rect))
```

```Scheme
(((- (d theta)
     (+ (wedge (d a) dx)
        (wedge (d b) dy)
        (wedge (d c) dz)))
  X Y)
 R3-rect-point)
;; 0
```

We can also try a general two-form field in 3-dimensional space:

Let

$$\begin{equation}
\omega = a\mathsf{d}\mathsf{y} \wedge \mathsf{d}\mathsf{z} + b \mathsf{d}\mathsf{z} \wedge \mathsf{d}\mathsf{x} + c \mathsf{d}\mathsf{x} \wedge \mathsf{d}\mathsf{y},
\end{equation}$$

where $a = \alpha \circ \chi,$ $b = \beta \circ \chi,$ $c = \gamma \circ \chi,$
and α, β, and γ are real-valued functions of three real arguments. As a program,

```Scheme
(define omega
  (+ (* a (wedge dy dz))
     (* b (wedge dz dx))
     (* c (wedge dx dy))))
```

Here we need another vector field because our result will be a three-form field.

```Scheme
(define Z (literal-vector-field 'Z-rect R3-rect))
```

```Scheme
(((- (d omega)
     (+ (wedge (d a) dy dz)
        (wedge (d b) dz dx)
        (wedge (d c) dx dy)))
  X Y Z)
 R3-rect-point)
;; 0
```

### Properties of Exterior Derivatives

The exterior derivative of the wedge of two form fields obeys the graded Leibniz rule. It can be written in terms of the exterior derivatives of the component form fields:

$$\begin{equation}
\mathsf{d}(\omega \wedge \tau) = \mathsf{d}\omega \wedge \tau + (-1)^{k} \omega \wedge \mathsf{d} \tau,
\end{equation}$$

where $k$ is the rank of ω.

A form field ω that is the exterior derivative of another form field $\omega =
\mathsf{d}\theta$ is called exact. A form field whose exterior derivative is zero is called closed.

Every exact form field is a closed form field: applying the exterior derivative operator twice always yields zero:

$$\begin{equation}
\mathsf{d}^{2} \omega = 0
\end{equation}$$

This is equivalent to the statement that partial derivatives with respect to different variables commute.#Footnote(8)

It is easy to show equation (5.29) for manifold functions:

$$\begin{equation}
\begin{aligned}
\mathsf{d}^{2} \mathsf{f} (\mathsf{u}, \mathsf{v}) = \mathsf{d}(\mathsf{d}\mathsf{f})(\mathsf{u}, \mathsf{v}) \\
&= \mathsf{u}(\mathsf{d}\mathsf{f}(\mathsf{v})) - \mathsf{v}(\mathsf{d}\mathsf{f}(\mathsf{u})) - \mathsf{d}\mathsf{f}([\mathsf{u},\mathsf{v}]) \\
&= \mathsf{u}(\mathsf{v}(\mathsf{f})) - \mathsf{v}(\mathsf{u}(\mathsf{f})) - [\mathsf{u}, \mathsf{v}](\mathsf{f}) \\
&= 0
\end{aligned}
\end{equation}$$

Consider the general one-form field $\theta$ defined on 3-dimensional rectangular space. Taking two exterior derivatives of $\theta$ yields a three-form field. It is zero:

(((d (d theta)) X Y Z) R3-rect-point)
0

Not every closed form field is an exact form field. Whether a closed form field is exact depends on the topology of a manifold.

###  Stokes's Theorem

The proof of the general Stokes's Theorem for n-dimensional orientable manifolds is quite complicated, but it is easy to see how it works for a 2-dimensional region $\mathsf{M}$ that can be covered with a single coordinate patch.#Footnote(9)

Given a coordinate chart $\chi(\mathsf{m})=(\mathsf{x}(\mathsf{m}),\mathsf{y}(\mathsf{m}))$ we can obtain a pair of coordinate-basis vectors ${\partial}/{\partial\mathsf{x}} = {X}_{0}$
and ${\partial}/{\partial\mathsf{y}} = {X}_{1}$.

The coordinate image of $\mathsf{M}$ can be divided into small rectangular areas in the $(x,y)$ coordinate plane. The union of the rectangular areas gives the coordinate image of $\mathsf{M}$. The clockwise integrals around the boundaries of the rectangles cancel on neighboring rectangles, because the boundary is traversed in opposite directions. But on the boundary of the coordinate image of $\mathsf{M}$ the boundary integrals do not cancel, yielding an integral on the boundary of $\mathsf{M}$. Area integrals over the rectangular areas add to produce an integral over the entire coordinate image of $\mathsf{M}$.

So, consider Stokes's Theorem on a small patch $\mathsf{P}$ of the manifold for which the coordinates form a rectangular region $(x_{min} < x < x_{max} \text{and} y_{min} < y < y_{max})$. Stokes's Theorem on $\mathsf{P}$ states

$$\begin{equation}
\int_{\partial\mathsf{P}} \omega = \int_{\mathsf{P}} \mathsf{d} \omega .
\end{equation}$$

The area integral on the right can be written as an ordinary multidimensional integral using the coordinate basis vectors (recall that the integral is independent of the choice of coordinates):

$$\begin{equation}
\begin{aligned}
\int_{\chi(\mathsf{P})} &\mathsf{d} \omega ({\partial}/{\partial\mathsf{x}}, {\partial}/{\partial\mathsf{y}}) \circ \chi^{-1} \\
&= \int_{x_{min}}^{x_{max}} \int_{y_{min}}^{y_{max}} ({\partial}/{\partial\mathsf{x}} (\omega ({\partial}/{\partial\mathsf{y}})) - {\partial}/{\partial\mathsf{y}}(\omega ({\partial}/{\partial\mathsf{x}}))) \circ \chi^{-1}.
\end{aligned}
\end{equation}$$

We have used equation (5.23) to expand the exterior derivative.

Consider just the first term of the right-hand side of equation (5.32). Then using the definition of basis vector field ${\partial}/{\partial\mathsf{x}}$ we obtain

$$\begin{equation}
\begin{aligned}
\int_{{x}_{min}}^{{x}_{max}} &\int_{{y}_{min}}^{{y}_{max}} ({\partial}/{\partial\mathsf{x}} (\omega ({\partial}/{\partial\mathsf{y}})) \circ \chi^{-1}) \\
&= \int_{{x}_{min}}^{{x}_{max}} \int_{{y}_{min}}^{{y}_{max}} (X_{0}(\omega ({\partial}/{\partial\mathsf{y}})) \circ \chi^{-1}) \\
&= \int_{{x}_{min}}^{{x}_{max}} \int_{{y}_{min}}^{{y}_{max}} \partial_{0} ((\omega ({\partial}/{\partial\mathsf{y}})) \circ \chi^{-1}).
\end{aligned}
\end{equation}$$

This integral can now be evaluated using the Fundamental Theorem of Calculus. Accumulating the results for both integrals

$$\begin{equation}
\begin{aligned}
\int_{\chi (\mathsf{P})} \mathsf{d}\omega &({\partial}/{\partial\mathsf{x}}, {\partial}/ {\partial\mathsf{y}}) \circ \chi^{-1} \\
= &\int_{{x}_{min}}^{{x}_{max}} ((\omega ({\partial}/{\partial\mathsf{x}})) \circ \chi^{-1}) (x, y_{min})dx \\
&\int_{{y}_{min}}^{{y}_{max}} ((\omega({\partial}/{\partial\mathsf{y}}) \circ \chi^{-1}) (x_{max}, y)dy \\
- &\int_{x_{min}}^{x_{max}} ((\omega (\partial / \partial\mathsf{x})) \circ \chi^{-1}) (x, y_{max})dx \\
- &\int_{{y}_{min}}^{{y}_{max}} ((\omega({\partial}/{\partial\mathsf{y}})) \circ \chi^{-1}) (x_{min}, y)dy \\
= &\int_{\partial\mathsf{P}} \omega,
\end{aligned}
\end{equation}$$

as was to be shown.

### Vector Integral Theorems

Green's Theorem states that for an arbitrary compact set $M \subset \mathrm{R}^{2}$, a 2-dimensional Euclidean space:

$$\begin{equation}
\int_{\partial M} ((\alpha \circ \chi) \mathsf{d}\mathsf{x} + (\beta \circ \chi) \mathsf{d}\mathsf{y}) = \int_{M} ((\partial_{0} \beta - \partial_{1}\alpha) \circ \chi) \mathsf{d}\mathsf{x} \wedge \mathsf{d}\mathsf{y}.
\end{equation}$$

We can test this. By Stokes's Theorem, the integrands are related by an exterior derivative. We need some vectors to test our forms:

```Scheme
(define v (literal-vector-field 'v-rect R2-rect))
(define w (literal-vector-field 'w-rect R2-rect))
```

We can now test our integrands:#Footnote(10)

```Scheme
(define alpha (literal-function 'alpha R2->R))
(define beta (literal function 'beta R2->R))
```

```Scheme
(let ((dx (ref (basis->1form-basis R2-rect-basis) 0))
      (dy (ref (basis-1>form-basis R2-rect-basis) 1)))
  (((- (d (+ (* (compose alpha (chart R2-rect)) dx)
             (* (compose beta (chart R2-rect)) dy)))
       (* (compose (- ((partial 0) beta)
                      ((partial 1) alpha))
                   (chart R2-rect))
          (wedge dx dy)))
    v w)
   R2-rect-point))
;; 0
```

We can also compute the integrands for the Divergence Theorem: For an arbitrary compact set $M \subset \mathrm{R}^{3}$ and a vector field $\mathsf{w}$

$$\begin{equation}
\int_{M} \text{div}(\mathsf{w})dV = \int_{\partial M} \mathsf{w} \cdot \mathsf{n}dA
\end{equation}$$

where $\mathsf{n}$ is the outward-pointing normal to the surface $\partial M$. Again, the integrands should be related by an exterior derivative, if this is an instance of Stokes's Theorem.

Note that even the statement of this theorem cannot be made with the machinery we have developed at this point. The concepts "outward-pointing normal," area $A$, and volume $V$ on the manifold are not definable without using a metric (see Chapter 9). However, for orthonormal rectangular coordinates in $\mathrm{R}^{3}$ we can interpret the integrands in terms of forms.

Let the vector field describing the flow of stuff be

$$\begin{equation}
\mathsf{w} = \mathsf{a} \frac{\partial}{\partial\mathsf{x}} + \mathsf{b} \frac{\partial}{\partial\mathsf{y}} + \mathsf{c} \frac{\partial}{\partial\mathsf{z}}.
\end{equation}$$

The rate of leakage of stuff through each element of the boundary is ${\mathsf{w}} \cdot {\mathsf{n}dA}$. We interpret this as the two-form

$$\begin{equation}
\mathsf{a} \, \mathsf{d}\mathsf{y} \wedge \mathsf{d}\mathsf{z} + \mathsf{b} \, \mathsf{d}\mathsf{z} \wedge \mathsf{d}\mathsf{x} + \mathsf{c} \, \mathsf{d}\mathsf{x} \wedge \mathsf{d}\mathsf{y},
\end{equation}$$

because any part of the boundary will have $y - z$, $z - x$, and $x - y$ components, and each such component will pick up contributions from the normal component of the flux $w$. Formalizing this as code we have

```Scheme
(define a (literal-manifold-function 'a-rect R3-rect))
(define b (literal-manifold-function 'b-rect R3-rect))
(define c (literal-manifold function 'c-rect R3-rect))

(define flux-through-boundary-element
  (+ (* a (wedge dy dz))
     (* b (wedge dz dx))
     (* c (wedge dx dy))))
```

The rate of production of stuff in each element of volume is
$\text{div}(\mathsf{w})dV$. We interpret this as the three-form

$$\begin{equation}
\left(\frac{\partial}{\partial\mathsf{x}}\mathsf{a} + \frac{\partial}{\partial\mathsf{y}}\mathsf{b} + \frac{\partial}{\partial\mathsf{z}}\mathsf{c}\right)\: \mathsf{d}\mathsf{x} \wedge \mathsf{d}\mathsf{y} \wedge \mathsf{d}\mathsf{z}.
\end{equation}$$

or:

```Scheme
(define production-in-volume-element
  (* (+ (d/dx a) (d/dy b) (d/dz c))
     (wedge dx dy dz)))
```

Assuming Stokes's Theorem, the exterior derivative of the leakage of stuff per unit area through the boundary must be the rate of production of stuff per unit volume in the interior. We check this by applying the difference to arbitrary vector fields at an arbitrary point:

```Scheme
(define X (literal-vector-field 'X-rect R3-rect))
(define Y (literal-vector-field 'Y-rect R3-rect))
(define Z (literal-vector-field 'Z-rect R3-rect))
```

```Scheme
(((- production-in-volume-element
     (d flux-through-boundary-element))
  X Y Z)
 R3-rect-point)
0
```

as expected.

### Exercise 5.2: Graded Formula

Derive equation (5.28).

### Exercise 5.3: Iterated Exterior Derivative

We have shown that the equation (5.29) is true for manifold functions. Show that it is true for any form field.

----
### Footnotes


#FootnoteRef(1) Note $(D(\chi \circ \chi^{\prime-1}) \circ (\chi^{\prime} \circ
\chi^{-1})) D (\chi^{\prime} \circ \chi^{-1}) = 1$. With $g = \chi^{\prime}
\circ \chi^{-1}$ this is $(D(g^{-1} \circ g)(Dg) = 1$.

#FootnoteRef(2) The determinant is the unique function of the rows of its argument that i) is linear in each row, ii) changes sign under any interchange of rows, and iii) is one when applied to the identity multiplier.

#FootnoteRef(3) By using the word "orthonormal" here we are assuming that the range of the coordinate chart is an ordinary Euclidean space with the usual Euclidean metric. The coordinate basis in that chart is orthonormal. Under these conditions we can usefully use words like "length," "area," and "volume" in the coordinate space.

#FootnoteRef(4) This is a generalization of the Fundamental Theorem of Calculus.

#FootnoteRef(5) A manifold function $\mathsf{f}$ induces a form field $\hat{\mathsf{f}}$ of rank 0 as follows:

$$\begin{equation}
\hat{\mathsf{f}}()(\mathsf{m}) = \mathsf{f}(\mathsf{m}).
\end{equation}$$

#FootnoteRef(6) The definition is chosen to make Stokes's Theorem pretty.

#FootnoteRef(7) See Spivak, Differential Geometry, Volume 1, p.289.

#FootnoteRef(8) See Spivak, Calculus on Manifolds, p.92

#FootnoteRef(9) We do not develop the machinery for integration on chains that is usually needed for a full proof of Stokes's Theorem. This is adequately done in other books. A beautiful treatment can be found in Spivak, Calculus on Manifolds [17](references!bib_17).

#FootnoteRef(10) Using =(define R2-rect-basis (coordinate-system->basis R2-rect))=.

Here we extract $\mathsf{d}\mathsf{x}$ and $\mathsf{d}\mathsf{y}$ from R2-rect-basis to avoid globally installing coordinates.

#FootnoteEnd
