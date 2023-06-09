!!Polyglot
#page(0)

# Chapter 3: Vector Fields and One-Form Fields

We want a way to think about how a function varies on a manifold. Suppose we have some complex linkage, such as a multiple pendulum. The potential energy is an important function on the multi-dimensional configuration manifold of the linkage. To understand the dynamics of the linkage we need to know how the potential energy changes as the configuration changes. The change in potential energy for a step of a certain size in a particular di- rection in the configuration space is a real physical quantity; it does not depend on how we measure the direction or the step size. What exactly this means is to be determined: What is a step size? What is a direction? We cannot subtract two configurations to determine the distance between them. It is our job here to make sense of this idea.

So we would like something like a derivative, but there are problems. Since we cannot subtract two manifold points, we cannot take the derivative of a manifold function in the way described in elementary calculus. But we can take the derivative of a coordinate representation of a manifold function, because it takes real-number coordinates as its arguments. This is a start, but it is not independent of coordinate system. Let's see what we can build out of this.

### Vector Fields

In multiple dimensions the derivative of a function is the multiplier for the best linear approximation of the function at each argument point:#Footnote(1)

$$\begin{equation}
f(x + \Delta x) \approx f(x) + (Df(x)) \Delta x
\end{equation}$$

The derivative $Df(x)$ is independent of $\Delta x$. Although the derivative depends on the coordinates, the product $(Df(x)) \Delta x$ is invariant under change of coordinates in the following sense. Let $\phi = \chi \circ \chi^{\prime -1}$ be a coordinate transformation, and $x = \phi(y)$. Then $\Delta x = D\phi(y)\Delta y$ is the linear approximation to the change in $x$ when $y$ changes by $\Delta y$. If $f$ and $g$ are the representations of a manifold function in the two coordinate systems, $g(y) = f(\phi(y)) = f(x)$, then the linear approximations to the increments in $f$ and $g$ are equal:

$$\begin{equation}
Dg(y)\Delta y = Df(\phi(y))(D\phi(y)\Delta y) = Df(x)\Delta x.
\end{equation}$$

The invariant product $(Df(x)) \Delta x$ is the /directional derivative/ of $f$ at $x$ with respect to the vector specified by the tuple of components $\Delta x$ in the coordinate system. We can generalize this idea to allow the vector at each point to depend on the point, making a /vector field/. Let b be a function of coordinates. We then have a directional derivative of $f$ at each point $x$, determined by $b$

$$\begin{equation}
D_b(f)(x) = (Df(x)) b(x).
\end{equation}$$

Now we bring this back to the manifold and develop a useful generalization of the idea of directional derivative for functions on a manifold, rather than functions on $\mathbb{R}^n$. A /vector field/ on a manifold is an assignment of a vector to each point on the manifold. In elementary geometry, a vector is an arrow anchored at a point on the manifold with a magnitude and a direction. In differential geometry, a vector is an operator that takes directional derivatives of manifold functions at its anchor point. The direction and magnitude of the vector are the direction and scale factor of the directional derivative.

Let $\mathsf{m}$ be a point on a manifold, $\mathsf{v}$ be a vector field on the manifold, and $\mathsf{f}$ be a real-valued function on the manifold. Then $\mathsf{v}(\mathsf{f})$ is the directional derivative of the function $\mathsf{f}$ and $\mathsf{v}(\mathsf{f})(\mathsf{m})$ is the directional derivative of the function$\mathsf{f}$at the point $\mathsf{m}$. The vector field is an operator that takes a real-valued manifold function and a manifold point and produces a number. The order of arguments is chosen to make $\mathsf{v}(\mathsf{f})$ be a new manifold function that can be manipulated further. Directional derivative operators, unlike ordinary derivative operators, produce a result of the same type as their argument. Note that there is no mention here of any coordinate system. The vector field specifies a direction and magnitude at each manifold point that is independent of how it is described using any coordinate system.

A useful way to characterize a vector field in a particular coordinate system is by applying it to the coordinate functions. The resulting functions $b^i_{\chi,\mathsf{v}}$ are called the /coordinate component functions/ or /coefficient functions/ of the vector field; they measure how quickly the coordinate functions change in the direction of the vector field, scaled by the magnitude of the vector field:

$$\begin{equation}
b^i_{\chi,\mathsf{v}} = \mathsf{v}(\chi^i) \circ \chi^{-1}.
\end{equation}$$

Note that we have chosen the coordinate components to be functions of the coordinate tuple, not of a manifold point.

A vector with coordinate components $b_{\chi,\mathsf{v}}$ applies to a manifold function $\mathsf{f}$ via

$$\begin{equation}
\begin{align}
\mathsf{v}(\mathsf{f})(\mathsf{m})
&= ((D(\mathsf{f} \circ \chi^{-1}) b_{\chi, \mathsf{v}}) \circ \chi)(\mathsf{m}) \\
&= D(\mathsf{f} \circ \chi^{-1})(\chi(\mathsf{m})) b_{\chi, \mathsf{v}}(\chi(\mathsf{m})) \\
&= \sum_i \partial_i (\mathsf{f} \circ \chi^{-1}) (\chi(\mathsf{m})) b^i_{\chi, \mathsf{v}}(\chi (\mathsf{m})).
\end{align}
\end{equation}$$

In equation (3.4), the quantity $\mathsf{f} \circ$ is the coordinate representation of the manifold function $\mathsf{f}$. We take its derivative, and weight the components of the derivative with the coordinate components $b_{\chi, \mathsf{v}}$ of the vector field that specify its direction and magnitude. Since this product is a function of coordinates we use $\chi$ to extract the coordinates from the manifold point m. In equation (3.5), the composition of the product with the coordinate chart $\chi$ is replaced by function evaluation. In equation (3.6) the tuple multiplication is expressed explicitly as a sum of products of corresponding components. So the application of the vector is a linear combination of the partial derivatives of $\mathsf{f}$ in the coordinate directions weighted by the vector components. This computes the rate of change of $\mathsf{f}$
in the direction specified by the vector.

Equations (3.3) and (3.5) are consistent:

$$\begin{equation}
\begin{align}
\mathsf{v}(x) (\chi^{-1}(x)) \nonumber
&= D (\chi \circ \chi^{-1})(x) b_{\chi,\mathsf{v}}(x) \nonumber \\
&= D(I)(x) b_{\chi, \mathsf{v}}(x) \nonumber \\
&= b_{\chi,\mathsf{v}}(x).
\end{align}
\end{equation}$$

The coefficient tuple $b_{\chi,\mathsf{v}}(x)$ is an up structure compatible for addition to the coordinates. Note that for any vector field v the coefficients $b_{\chi, \mathsf{v}}(x)$ are different for different coordinate functions $\chi$. In the text that follows we will usually drop the subscripts on $b$, understanding that it is dependent on the coordinate system and the vector field.

We implement the definition of a vector field (3.4) as:
```Scheme
(define (components->vector-field components coordsys)
(define (v f)
 (compose (* (D (compose f (point coordsys)))
             components)
          (chart coordsys)))
(procedure->vector-field v))
```

The vector field is an operator, like derivative.#Footnote(2)

Given a coordinate system and coefficient functions that map coordinates to real values, we can make a vector field. For example, a general vector field can be defined by giving components relative to the coordinate system #Code(R2-rect) by
```Scheme
(define v
(components->vector-field
(up (literal-function ’b^0 R2->R)
    (literal-function ’b^1 R2->R))
R2-rect))
```
To make it convenient to define literal vector fields we provide a shorthand: ~(define v (literal-vector-field ’b R2-rect))~ This makes a vector field with component functions named #Code(b^0) and #Code(b^1) and names the result #Code(v). When this vector field is applied to an arbitrary manifold function it gives the directional derivative of that manifold function in the direction specified by the components #Code(b^0) and #Code(b^1):
```Scheme
((v (literal-manifold-function ’f-rect R2-rect)) R2-rect-point)
;; (+ (* (((partial 0) f-rect) (up x0 y0)) (b?0 (up x0 y0)))
;;    (* (((partial 1) f-rect) (up x0 y0)) (b?1 (up x0 y0))))
```
This result is what we expect from equation (3.6).

We can recover the coordinate components of the vector field by applying the vector field to the coordinate chart:
```Scheme
((v (chart R2-rect)) R2-rect-point)
;; (up (b?0 (up x y)) (b?1 (up x y)))
```

#####  Coordinate Representation

The vector field $\mathsf{v}$ has a coordinate representation $v$:

$$\begin{equation}
\begin{align}
\mathsf(\mathsf{f})(\mathsf{m}) \nonumber
&= D(f \circ \chi^{-1})(\chi(\mathsf{m})) b(\chi(\mathsf{m})) \nonumber \\
&= Df(x) b(x) \\
&= v(f)(x),
\end{align}
\end{equation}$$

with the definitions $f = \mathsf{f} \circ \chi^{-1}$ and $x = \chi(\mathsf{m})$. The function $b$ is the coefficient function for the vector field $\mathsf{v}$. It provides a scale factor for the component in each coordinate direction. However, $v$ is the coordinate representation of the vector field $\mathsf{v}$ in that it takes directional derivatives of coordinate representations of manifold functions.

Given a vector field #Code(v) and a coordinate system coordsys we can construct the coordinate representation of the vector field.#Footnote(3)
```Scheme
(define (coordinatize v coordsys)
 (define ((coordinatized-v f) x)
   (let ((b (compose (v (chart coordsys))
                     (point coordsys))))
     (* ((D f) x) (b x)))))
(make-operator coordinatized-v))
```
We can apply a coordinatized vector field to a function of coordinates to get the same answer as before.
```Scheme
(((coordinatize v R2-rect) (literal-function ’f-rect R2->R))
(up ’x0 ’y0))
;; (+ (* (((partial 0) f-rect) (up x0 y0)) (b?0 (up x0 y0)))
;;    (* (((partial 1) f-rect) (up x0 y0)) (b?1 (up x0 y0))))
```

#####  Vector Field Properties

The vector fields on a manifold form a vector space over the field of real numbers and a module over the ring of real-valued manifold functions. A module is like a vector space except that there is no multiplicative inverse operation on the scalars of a module. Manifold functions that are not the zero function do not necessarily have multiplicative inverses, because they can have isolated zeros. So the manifold functions form a ring, not a field, and vector fields must be a module over the ring of manifold functions rather than a vector space.

Vector fields have the following properties. Let $\mathsf{u}$ and $\mathsf{v}$ be vector fields and let $\alpha$ be a real-valued manifold function. Then

$$\begin{equation}
\begin{align}
&(\mathsf{u} + \mathsf{v})(f) = \mathsf{u}(\mathsf{f}) + \mathsf{v}(\mathsf{f}) \\
&(\alpha \mathsf{f})(\mathsf{f}) = \alpha(\mathsf{u}(\mathsf{f})).
\end{align}
\end{equation}$$

Vector fields are linear operators. Assume $\mathsf{f}$ and $\mathsf{g}$ are functions on the manifold, $a$ and $b$ are real constants.#Footnote(4) The constants $a$ and $b$ are not manifold functions, because vector fields take derivatives. See equation (3.13).

$$\begin{equation}
\begin{align}
&\mathsf{v}(a \mathsf{f} + b \mathsf{g}) (\mathsf{m})
= a \mathsf{v}(\mathsf{f})(\mathsf{m}) + b \mathsf{v}(\mathsf{g})(\mathsf{m}) \\
&\mathsf{v}(a \mathsf{f})(\mathsf{m})
= a \mathsf{v}(\mathsf{f})(\mathsf{m})
\end{align}
\end{equation}$$

Vector fields satisfy the product rule (Leibniz rule).

$$\begin{equation}
\mathsf(\mathsf{fg})(\mathsf{m})
= \mathsf{v}(\mathsf{f})(\mathsf{m}) \mathsf{g}(\mathsf{m})
+ \mathsf{f}(\mathsf{m}) \mathsf{v}(\mathsf{g})(\mathsf{m})
\end{equation}$$

Vector fields satisfy the chain rule. Let $F$ be a function on the range of $\mathsf{f}$.

$$\begin{equation}
\mathsf{v}(F \circ \mathsf{f})(\mathsf{m})
= DF(\mathsf{f}(\mathsf{m})) \mathsf{v}(\mathsf{f})(\mathsf{m})
\end{equation}$$

### Coordinate-Basis Vector Fields

For an $n$-dimensional manifold any set of $n$ linearly independent vector fields#Footnote(5) form a /basis/ in that any vector field can be expressed as a linear combination of the basis fields with manifold-function coefficients. Given a coordinate system we can construct a basis as follows: we choose the component tuple $b_i(x)$ (see equation 3.5) to be the $i$th unit tuple $u_(x)$---an up tuple with one in the $i$th position and zeros in all other positions---selecting the partial derivative in that direction. Here $u_i$ is a constant function. Like $b$, it formally takes coordinates of a point as an argument, but it ignores them. We then define the basis vector field $\mathsf{X}_i$ by

$$\begin{equation}
\begin{align}
\mathsf{X}_i(\mathsf{f})(\mathsf{m})
&= D(\mathsf{f} \circ \chi^{-1})(\chi(\mathsf{m})) u_i(\chi(\mathsf{m})) \\
&= \partial_i(\mathsf{f} \circ \chi^{-1})(\chi(\mathsf{m})).
\end{align}
\end{equation}$$

In terms of $\mathsf{X}_i$ the vector field of equation (3.6) is

$$\begin{equation}
\mathsf{v}(\mathsf{f})(\mathsf{m})
= \sum_{i} \mathsf{X}_i(\mathsf{f}) (\mathsf{m}) b^i(\chi(\mathsf{m})).
\end{equation}$$

We can also write

$$\begin{equation}
\mathsf{v}(\mathsf{f})(\mathsf{m})
= \mathsf{X}(\mathsf{f})(\mathsf{m}) b(\chi(\mathsf{m})),
\end{equation}$$

letting the tuple algebra do its job.

The basis vector field is often written

$$\begin{equation}
\frac{\partial}{\partial x^i} = \mathsf{X}_i,
\end{equation}$$

to call to mind that it is an operator that computes the directional derivative in the ith coordinate direction.

In addition to making the coordinate functions, the procedure #Code(define-coordinates) also makes the traditional named basis vectors. Using these we can examine the application of a rectangular basis vector to a polar coordinate function:
```Scheme
(define-coordinates (up x y) R2-rect)
(define-coordinates (up r theta) R2-polar)
```
```Scheme
((d/dx (square r)) R2-rect-point)
;; (* 2 x0)
```
More general functions and vectors can be made as combinations of these simple pieces:
```Scheme
(((+ d/dx (* 2 d/dy)) (+ (square r) (* 3 x))) R2-rect-point)
;; (+ 3 (* 2 x0) (* 4 y0))
```

#####  Coordinate Transformations

Consider a coordinate change from the chart $\chi$ to the chart $\chi'$.

$$\begin{equation}
\begin{align}
\mathsf{\mathsf{X}}(\mathsf{f})(m)
&= D(\mathsf{f} \circ \chi^{-1})(\chi(\mathsf{m})) \nonumber \\
&= D(\mathsf{f} \circ (\chi')^{-1} \circ \chi' \circ \chi^{-1})(\chi(\mathsf{m})) \nonumber \\
&= D(\mathsf{f} ? (\chi')^{^1})(\chi'(\mathsf{m}))(D(\chi' \circ \chi^{-1}))(\chi(\mathsf{m})) \nonumber \\
&= \mathsf{X}'(\mathsf{f})(\mathsf{m})(D(\chi' \circ \chi^{-1}))(\chi(\mathsf{m})).
\end{align}
\end{equation}$$

This is the rule for the transformation of basis vector fields. The second factor can be recognized as ``∂x'/∂x,'' the Jacobian.#Footnote(6)

The vector field does not depend on coordinates. So, from equation (3.17), we have

$$\begin{equation}
\mathsf{v}(\mathsf{f})(\mathsf{m})
= \mathsf{X}(\mathsf{f})(\mathsf{m}) b(\chi(\mathsf{m}))
= \mathsf{X}'(\mathsf{f})(\mathsf{m}) b'(\chi'(\mathsf{m})).
\end{equation}$$

Using equation (3.19) with $x = \chi(\mathsf{m})$ and $x' = \chi'(\mathsf{m})$, we deduce

$$\begin{equation}
D(\chi' \circ \chi^{-1})(x) b(x) = b'(x').
\end{equation}$$

Because $\chi' \circ \chi^{-1}$ is the inverse function of $\chi \circ (\chi')^{-1}$, their derivatives are multiplicative inverses,

$$\begin{equation}
D(\chi' \circ \chi^{1})(x) = (D(\chi \circ (\chi')^{1})(x'))^{1},
\end{equation}$$

and so

$$\begin{equation}
b(x) = D(\chi \circ (\chi')^{1})(x') b'(x'),
\end{equation}$$

as expected.#Footnote(7)

It is traditional to express this rule by saying that the basis elements transform /covariantly/ and the coefficients of a vector in terms of a basis transform contravariantly; their product is invariant under the transformation.

### Integral Curves

A vector field gives a direction and rate for every point on a manifold. We can start at any point and go in the direction specified by the vector field, tracing out a parametric curve on the manifold. This curve is an /integral curve/ of the vector field.

More formally, let $\mathsf{v}$ be a vector field on the manifold $\mathsf{M}$. An integral curve $\gamma^{\mathsf{v}}_{\mathsf{m}} \colon \mathsf{R} \to \mathsf{M}$ of $\mathsf{v}$ is a parametric path on $\mathsf{M}$ satisfying

$$\begin{equation}
\begin{align}
D(\mathsf{f} \circ \gamma^{\mathsf{v}}_{\mathsf{m}}) (t)
&= \mathsf{v}(\mathsf{f}) (\gamma^{\mathsf{v}}_{\mathsf{m}}(t))
= (\mathsf{v}(\mathsf{f}) \circ \gamma^{\mathsf{v}}_{\mathsf{m}})(t) \\
\gamma^{\mathsf{v}}_{\mathsf{m}}(0)
&= \mathsf{m},
\end{align}
\notag
\end{equation}$$

for arbitrary functions $\mathsf{f}$ on the manifold, with real values or structured real values. The rate of change of a function along an integral curve is the vector field applied to the function evaluated at the appropriate place along the curve. Often we will simply write $\gamma$, rather than $\gamma^{\mathsf{v}}_{\mathsf{m}}$. Another useful variation is $\phi^{\mathsf{v}}_t(\mathsf{m}) = \gamma^{\mathsf{v}}_{\mathsf{m}}(t)$.

We can recover the differential equations satisfied by a coordinate representation of the integral curve by letting $\mathsf{f} = \chi$, the coordinate function, and letting $\sigma = \chi \circ \gamma$ be the coordinate path corresponding to the curve $\gamma$. Then the derivative of the coordinate path $\sigma$ is

$$\begin{equation}
\begin{align}
D \sigma(t)
&=  D(\chi \circ \gamma)(t) \nonumber \\
&= (\mathsf{v}(\chi) \circ \gamma)(t) \nonumber \\
&= (\mathsf{v}(\chi) \circ \chi^{-1} \circ \chi \circ \gamma)(t) \\
&= (b \circ \sigma)(t)
\end{align}
\notag
\end{equation}$$

where $b = \mathsf{v}(\chi) \circ \chi^{-1}$ is the coefficient function for the vector field
$\mathsf{v}$ for coordinates $\chi$ (see equation 3.7). So the coordinate path $\sigma$
satisfies the differential equations

$$\begin{equation}
D \sigma = b \circ \sigma.
\notag
\end{equation}$$

Differential equations for the integral curve can be expressed only in a coordinate representation, because we cannot go from one point on the manifold to another by addition of an increment. However, we can do this by adding the coordinates to an increment of coordinates and then finding the corresponding point on the manifold.

Iterating the process described by equation (3.24) we can compute higher-order derivatives of functions along the integral curve:

$$\begin{equation}
\begin{align}
D(\mathsf{f} \circ \gamma)
&= \mathsf{v}(\mathsf{f}) \circ \gamma \nonumber \\
D^2(\mathsf{f} \circ \gamma)
&= D(\mathsf{v}(\mathsf{f}) \circ \gamma)
= \mathsf{v}(\mathsf{v}(\mathsf{f})) \circ \gamma \nonumber \\
&\cdots \nonumber \\
D^n(\mathsf{f} \circ \gamma)
&= \mathsf{v}^n(\mathsf{f}) \circ \gamma
\end{align}
\notag
\end{equation}$$

Thus, the evolution of $\mathsf{f} \circ \gamma$ can be written formally as a Taylor series in the parameter:

$$\begin{equation}
\begin{align}
&(f \circ \gamma)(t) \nonumber \\
&= (f \circ \gamma)(0) + t D(\mathsf{f} \circ \gamma)(0) + \frac{1}{2}t^2 D^2(\mathsf{f} \circ \gamma)(0) + \cdots \nonumber \\
&= (e^{tD} (\mathsf{f} \circ \gamma))(0) \nonumber \\
&= e^{t \mathsf{v} \mathsf{f}} (\gamma(0)).
\end{align}
\end{equation}$$

Using $\phi$ rather than $\gamma$

$$\begin{equation}
(\mathsf{f} \circ \gamma^{\mathsf{v}}_{\mathsf{m}})(t)
= (\mathsf{f} \circ \phi^{\mathsf{v}}_t)(\mathsf{m}),
\end{equation}$$

so, when the series converges,

$$\begin{equation}
(e^{t \mathsf{v}} \mathsf{f})(\mathsf{m})
= (\mathsf{f} \circ \phi^{\mathsf{v}}_t)(\mathsf{m}).
\end{equation}$$

In particular, let $\mathsf{f} = \chi$, then

$$\begin{equation}
\sigma(t)
= (\chi \circ \gamma)(t)
= (e^{tD} (\chi \circ \gamma))(0)
= (e^{t \mathsf{v}} \chi) (\gamma(0)),
\end{equation}$$

a Taylor series representation of the solution to the differential equation (3.27).

For example, a vector field \textsf{circular} that generates a rotation about the origin is:#Footnote(8)
```Scheme
(define circular (- (* x d/dy) (* y d/dx)))
```
We can exponentiate the circular vector field, to generate an evolution in a circle around the origin starting at #Code((1, 0#)):
```Scheme
(series:for-each print-expression
              (((exp (* ’t circular)) (chart R2-rect))
               ((point R2-rect) (up 1 0)))
              6)
;; (up 1 0)
;; (up 0 t)
;; (up (* -1/2 (expt t 2)) 0)
;; (up 0 (* -1/6 (expt t 3)))
;; (up (* 1/24 (expt t 4)) 0)
;; (up 0 (* 1/120 (expt t 5)))
```
These are the first six terms of the series expansion of the coordinates of the position for parameter #Code(t).

We can define an evolution operator $\mathsf{E}_{\Delta t, \mathsf{v}}$ using equation (3.31)

$$\begin{equation}
(E_{\Delta t, \mathsf{v}} \mathsf{f})(\mathsf{m})
= (e^{\Delta t \mathsf{v}} \mathsf{f})(\mathsf`m`)
= (\mathsf{f} \circ \phi^{\mathsf{v}}_{\Delta t}(\mathsf{m}).
\end{equation}$$

We can approximate the evolution operator by summing the series up to a given order:
```Scheme
(define ((((evolution order) delta-t v) f) m)
(series:sum
(((exp (* delta-t v)) f) m)
order))
```
We can evolve circular from the initial point up to the parameter
#Code(t), and accumulate the first six terms as follows:
```Scheme
((((evolution 6) ’delta-t circular) (chart R2-rect))
((point R2-rect) (up 1 0)))
;; (up (+ (* -1/720 (expt delta-t 6))
;;        (* 1/24 (expt delta-t 4))
;;        (* -1/2 (expt delta-t 2))
;;        1)
;;     (+ (* 1/120 (expt delta-t 5))
;;        (* -1/6 (expt delta-t 3))
;;        delta-t))
```
Note that these are just the series for $\cos \Delta t$ and $\sin \Delta t$, so the coordinate tuple of the evolved point is $(\cos \Delta t, \sin \Delta t)$.

For functions whose series expansions have finite radius of convergence, evolution can progress beyond the point at which the Taylor series converges because evolution is well defined whenever the integral curve is defined.

### Exercise 3.1: State Derivatives

Newton's equations for the motion of a particle in a plane, subject to a force that depends only on the position in the plane, are a system of second-order differential equations for the rectangular coordinates
$(X, Y)$ of the particle:

$$\begin{equation}
D^2X(t)
= A_x(X(t), Y(t)) \text{ and }
D^2Y(t) = A_y (X(t), Y(t)),
\end{equation}$$

where $A$ is the acceleration of the particle.

These are equivalent to a system of first-order equations for the coordinate path $\sigma = \chi \circ \gamma$, where $\chi = (\mathsf{t}, \mathsf{x}, \mathsf{y}, \mathsf{v}_x, \mathsf{v}_y)$ is a coordinate system on the manifold Rh $\mathbb{R}^5$. Then our equations are:

$$\begin{equation}
\begin{align}
D(\mathsf{t}   \circ \gamma) &= 1 \nonumber \\
D(\mathsf{x}   \circ \gamma) &= \mathsf{v}_x \circ \gamma \nonumber \\
D(\mathsf{y}   \circ \gamma) &= \mathsf{v}_y \circ \gamma \nonumber \\
D(\mathsf{v}_x \circ \gamma) &= A_x(\mathsf{x} \circ \gamma, \mathsf{y} \circ \gamma) \nonumber \\
D(\mathsf{v}_y \circ \gamma) &= A_y(\mathsf{x} \circ \gamma, \mathsf{y} \circ \gamma) \nonumber D(\mathsf{t} \circ \gamma) &= 1 \\
\end{align}
\end{equation}$$

Construct a vector field on $\mathbb{R}^5$ corresponding to this system of differential equations. Derive the first few terms in the series solution of this problem by exponentiation.

### One-Form Fields

A vector field that gives a velocity for each point on a topographic map of the surface of the Earth can be applied to a function, such as one that gives the height for each point on the topographic map, or a map that gives the temperature for each point. The vector field then provides the rate of change of the height or temperature as one moves in the way described by the vector field. Alternatively, we can think of a topographic map, which gives the height at each point, as measuring a velocity field at each point. For example, we may be interested in the velocity of the wind or the trajectories of migrating birds. The topographic map gives the rate of change of height at each point for each velocity vector field. The rate of change of height can be thought of as the number of equally-spaced (in height) contours that are pierced by each velocity vector in the vector field.

#####  Differential of a Function

For example, consider the /differential/#Footnote(9) df of a manifold function $\mathsf{f}$, defined as follows. If $\mathsf{df}$ is applied to a vector field $\mathsf{v}$ we obtain

$$\begin{equation}
\mathsf{df}(\mathsf{v}) = \mathsf{v}(\mathsf{f}),
\end{equation}$$

which is a function of a manifold point.

The differential of the height function on the topographic map is a function that gives the rate of change of height at each point for a velocity vector field. This gives the same answer as the velocity vector field applied to the height function.

The differential of a function is linear in the vector fields. The differential is also a linear operator on functions: if $\mathsf{f}_1$ and $\mathsf{f}_2$ are manifold functions, and if $c$ is a real constant, then

$$\begin{equation}
\mathsf{d}(\mathsf{f}_1 + \mathsf{f}_2) = \mathsf{df}_1 + \mathsf{df}_2
\end{equation}$$

and

$$\begin{equation}
\mathsf{d}(c \mathsf{f}) = c \mathsf{df}.
\end{equation}$$

Note that $c$ is not a manifold function.

#####  One-Form Fields

A one-form field is a generalization of this idea; it is something that measures a vector field at each point.

/One-form fields/ are linear functions of vector fields that produce real-valued functions on the manifold. A one-form field is linear in vector fields: if $\omega$ is a one-form field, $\mathsf{v}$ and $\mathsf{w}$ are vector fields, and $c$ is a manifold function, then

$$\begin{equation}
\omega(\mathsf{v} + \mathsf{w}) = \omega(\mathsf{v}) + \omega(\mathsf{w})
\notag
\end{equation}$$

and

$$\begin{equation}
\omega(\mathsf{cv}) = \mathsf{c} \omega(\mathsf{v}).
\notag
\end{equation}$$

Sums and scalar products of one-form fields on a manifold have the following properties. If $\omega$ and $\theta$ are one-form fields, and if $\mathsf{f}$ is a real-valued manifold function, then:

$$\begin{equation}
\begin{align}
(\omega + \theta)(\mathsf{v}) &= \omega(\mathsf{v}) + \theta(\mathsf{v}), \\
(\mathsf{f} \omega) (\mathsf{v}) &= \mathsf{f} \omega(\mathsf{v}).
\end{align}
\notag
\end{equation}$$

#####  Coordinate-Basis One-Form Fields

Given a coordinate function $\chi$, we define the coordinate-basis one-form fields $\tilde{\mathsf{X}}^i$ by

$$\begin{equation}
\tilde{\mathsf{X}}^i (\mathsf{v}) (\mathsf{m}) = \mathsf{v}(\chi^i)(\mathsf{m})
\end{equation}$$

or collectively

$$\begin{equation}
\begin{align}
\tilde{\mathsf{X}}(\mathsf{v})(\mathsf{m}) = \mathsf{v}(\chi) (\mathsf{m}).
\end{align}
\end{equation}$$

With this definition the coordinate-basis one-form fields are dual to the coordinate-basis vector fields in the following sense (see equation 3.15):#Footnote(10)

$$\begin{equation}
\begin{align}
\tilde{\mathsf{X}}^i (\mathsf{X}_j)(\mathsf{m})
= \mathsf{X}_j (\chi^i)(\mathsf{m})
= \partial_j (\chi^i \circ \chi^{-1}) (\chi(\mathsf{m}))
&= \delta^i_j.
\end{align}
\end{equation}$$

The tuple of basis one-form fields $\tilde{X}(\mathsf{v})(\mathsf{m})$ is an up structure like that of $\chi$.

The general one-form field $\omega$ is a linear combination of coordinate-basis one-form fields:

$$\begin{equation}
\omega(\mathsf{v}) = (a \circ \chi) \tilde{\mathsf{X}}(\mathsf{v})
\end{equation}$$

with coefficient-function tuple $a(x)$, for $x = \chi(\mathsf{m})$. We can write this more simply as

$$\begin{equation}
\omega(\mathsf{v}) = (a \circ \chi) \tilde{\mathsf{X}}(\mathsf{v}),
\end{equation}$$

because everything is evaluated at $\mathsf{m}$.

The coefficient tuple can be recovered from the one-form field:#Footnote(11)

$$\begin{equation}
a_i(x) = \omega(\tilde{X}_i)(\chi^{-1}(x)).
\end{equation}$$

This follows from the dual relationship (3.41). We can see this as a program:#Footnote(12)
```Scheme
(define omega
 (components->1form-field
  (down (literal-function ’a
                            0 R2->R)
        (literal-function ’a
                            1 R2->R))
  R2-rect))
```
```Scheme
((omega (down d/dx d/dy)) R2-rect-point)
;;       (down (a_0 (up x0 y0)) (a_1 (up x0 y0)))
```
We provide a shortcut for this construction:
```Scheme
(define omega (literal-1form-field 'a R2-rect))
```

A differential can be expanded in a coordinate basis:

$$\begin{equation}
\mathsf{df}(\mathsf{v}) = \sum_i \mathsf{c}_i \tilde{\mathsf{X}}^i (\mathsf{v}).
\end{equation}$$

The coefficients $\mathsf{c}_i = \mathsf{df}(\mathsf{X}_i) = \mathsf{X}_i(\mathsf{f}) = \partial_i(\mathsf{f} \circ \chi^{-1}) \circ \chi$ are the partial derivatives of the coordinate representation of $\mathsf{f}$ in the coordinate system of the basis:
```Scheme
(((d (literal-manifold-function 'f-rect R2-rect))
 (coordinate-system->vector-basis R2-rect))
R2-rect-point)
;;(down (((partial 0) f-rect) (up x0 y0))
;;      (((partial 1) f-rect) (up x0 y0)))
```
However, if the coordinate system of the basis differs from the coordinates of the representation of the function, the result is complicated by the chain rule:

```Scheme
(((d (literal-manifold-function 'f-polar R2-polar))
 (coordinate-system->vector-basis R2-rect))
((point R2-polar) (up 'r 'theta)))
;;(down (- (* (((partial 0) f-polar) (up r theta)) (cos theta))
;;         (/ (* (((partial 1) f-polar) (up r theta))
;;               (sin theta))
;;            r))
;;      (+ (* (((partial 0) f-polar) (up r theta)) (sin theta))
;;         (/ (* (((partial 1) f-polar) (up r theta))
;;               (cos theta))
;;            r))
```)
The coordinate-basis one-form fields can be used to find the coefficients of vector fields in the corresponding coordinate vector-field basis:

$$\begin{equation}
\tilde{\mathsf{X}}^i(\mathsf{v}) = \mathsf{v}(\chi^i) = b^i \circ χ
\end{equation}$$

or collectively,

$$\begin{equation}
\tilde{\mathsf{X}}(\mathsf{v}) = \mathsf{v}(\chi) = b \circ χ.
\end{equation}$$

A coordinate-basis one-form field is often written $\mathsf{dx}^i$. This traditional notation for the coordinate-basis one-form fields is justified by the relation:

$$\begin{equation}
\mathsf{dx}^i = \tilde{\mathsf{X}}^i = \mathsf{d} (\chi^i).
\end{equation}$$

The #Code(define-coordinates) procedure also makes the basis one-form fields with these traditional names inherited from the coordinates.

We can illlustrate the duality of the coordinate-basis vector fields and the coordinate-basis one-form fields:
```Scheme
(define-coordinates (up x y) R2-rect)
```
```Scheme
((dx d/dy) R2-rect-point)
;; 0
```
```Scheme
((dx d/dx) R2-rect-point)
;; 0
```
We can use the coordinate-basis one-form fields to extract the coefficients of #Code(circular) on the rectangular vector basis:

```Scheme
((dx circular) R2-rect-point)
;; (* -1 y0)
```
```Scheme
((dy circular) R2-rect-point)
;; x0
```
But we can also find the coefficients on the polar vector basis:
```Scheme
((dr circular) R2-rect-point)
;; 0
```
```Scheme
((dtheta circular) R2-rect-point)
;; 1
```
So #Code(circular) is the same as #Code(d/dtheta), as we can see by applying them both to the general function #Code(f):
```Scheme
(define f (literal-manifold-function ’f-rect R2-rect))
(((- circular d/dtheta) f) R2-rect-point)
0
```
#####  Not All One-Form Fields Are Differentials

Although all one-form fields can be constructed as linear combinations of basis one-form fields, not all one-form fields are differentials of functions.

The coefficients of a differential are (see equation 3.45):

$$\begin{equation}
\mathsf{c}_i = \mathsf{X}_i(\mathsf{f}) = \mathsf{df} (\mathsf{X}_i)
\end{equation}$$

and partial derivatives of functions commute

$$\begin{equation}
\mathsf{X}_i(\mathsf{X}_j(\mathsf{f})) = \mathsf{X}_j(\mathsf{X}_i(\mathsf{f})).
\end{equation}$$

As a consequence, the coefficients of a differential are constrained

$$\begin{equation}
\mathsf{X}_i(\mathsf{c}_j) = \mathsf{X}_j(\mathsf{c}_i),
\end{equation}$$

but a one-form field can be constructed with arbitrary coefficient functions. For example:

$$\begin{equation}
\mathsf{xdx} + \mathsf{xdy}
\end{equation}$$

is not a differential of any function. This is why we started with the basis one-form fields and built the general one-form fields in terms of them.

#####  Coordinate Transformations

Consider a coordinate change from the chart $\chi$ to the chart $\chi'$.

$$\begin{equation}
\begin{align}
\tilde{\mathsf{X}}(\mathsf{v})
&= \mathsf{v}(\chi) \nonumber \\
&= \mathsf{v}(\chi \circ (\chi')^{-1} \circ \chi') \nonumber \\
&= (D(\chi \circ (\chi')^{-1}) \circ \chi') \mathsf{v}(\chi') \\
&= (D(\chi \circ (\chi')^{-1}) \circ \chi') \circ \tilde{\mathsf{X}}'(v),
\end{align}
\end{equation}$$

where the third line follows from the chain rule for vector fields.

One-form fields are independent of coordinates. So,

$$\begin{equation}
\omega(v) = (a \circ \chi) \tilde{\mathsf{X}}(v) = (a' \circ \chi') \tilde{\mathsf{X}}'(v).
\end{equation}$$

Eqs. (3.54) and (3.53) require that the coefficients transform under coordinate transformations as follows:

$$\begin{equation}
a(\chi(\mathsf{m})) D(\chi \circ (\chi')^{-1})(\chi'(\mathsf{m})) = a'(\chi'(\mathsf{m})),
\end{equation}$$

or

$$\begin{equation}
a(\chi(\mathsf{m})) = a'(\chi'(\mathsf{m})) (D(\chi \circ (\chi')^{-1})(\chi'(\mathsf{m}gt)))^{-1}.
\end{equation}$$

The coefficient tuple $a(x)$ is a down structure compatible for contraction with $b(x)$. Let $\mathsf{v}$ be the vector with  coefficient tuple $b(x)$, and $\omega$ be the one-form with coefficient tuple $a(x)$. Then, by equation (3.43),

$$\begin{equation}
\omega(\mathsf{v}) = (a \circ \chi) (b \circ \chi).
\end{equation}$$

As a program:
```Scheme
(define omega (literal-1form-field 'a R2-rect))
```
```Scheme
(define v (literal-vector-field 'b R2-rect))
```
```Scheme
((omega v) R2-rect-point)
;; (+ (* (b^0 (up x y)) (a_0 (up x0 y0)))
;;    (* (b^1 (up x y)) (a_1 (up x0 y0))))
```

Comparing equation (3.56) with equation (3.23) we see that one-form components and vector components transform oppositely,
so that

$$\begin{equation}
a(x) b(x) = a'(x') b'(x'),
\end{equation}$$

as expected because $\omega(\mathsf{v})(\mathsf{m})$ is independent of coordinates.

### Exercise 3.2: Verification

Verify that the coefficients of a one-form field transform as described in equation (3.56). You should use equation (3.44) in your derivation.

### Exercise 3.3: Hill Climbing

The topography of a region on the Earth can be specified by a manifold function $\mathsf{h}$ that gives the altitude at each point on the manifold. Let $\mathsf{v}$ be a vector field on the manifold, perhaps specifying a direction and rate of walking at every point on the manifold.

*a. Form an expression that gives the power that must be expended to follow the vector field at each point.

*b. Write this as a computational expression.

----
### Footnotes


#FootnoteRef(1) In multiple dimensions the derivative $Df(x)$ is a down tuple structure of the partial derivatives and the increment $\Delta x$ is an up tuple structure, so the indicated product is to be interpreted as a contraction. (See equation B.8.)

#FootnoteRef(2) An operator is just like a procedure except that multiplication is interpreted as composition. For example, the derivative procedure is made into an operator #Code(D) so that we can say #Code((expt D 2)) and expect it to compute the second derivative. The procedure #Code(procedure->vector-field) makes a vector-field operator.

#FootnoteRef(3) The #Code(make-operator) procedure takes a procedure and returns an operator.

#FootnoteRef(4) If $\mathsf{f}$ has structured output then $\mathsf{v}(\mathsf{f})$ is the structure resulting from $\mathsf{v}$ being applied to each component of $\mathsf{f}$.

#FootnoteRef(5) A set of vector fields, $\{\mathsf{v}_i\}$, is linearly independent with respect to manifold functions if we cannot find nonzero manifold functions, $\{\mathsf{a}_i\}$, such that

$$\begin{equation}
\sum_i \mathsf{a}_i \mathsf{v}_i(\mathsf{f}) = \mathsf{0} (\mathsf{f}),
\end{equation}$$

where $\mathsf{0}$ is the vector field such that $\mathsf{0}(\mathsf{f})(\mathsf{m}) = 0$ for all $\mathsf{f}$ and $\mathsf{m}$.

#FootnoteRef(6) This notation helps one remember the transformation rule:

$$\begin{equation}
\frac{\partial f}{\partial x^i}
= \sum_j \frac{\partial f}{\partial x^{'j}} \frac{\partial x^{'j}}{\partial x^i},
\end{equation}$$

which is the relation in the usual Leibniz notation. As Spivak pointed out in /Calculus on Manifolds/, p.45, $f$ means something different on each side of the equation.

#FootnoteRef(7) For coordinate paths $q$ and $q'$ related by $q(t) = (\chi \circ (\chi')^{-1})(q'(t))$ the velocities are related by $Dq(t) = D(\chi \circ (\chi')^{-1})(q'(t))Dq'(t)$. Abstracting off paths, we get $v = D(\chi \circ (\chi')^{-1})(x')v'$.

#FootnoteRef(8) In this expression #Code(d/dx) and #Code(d/dy) are vector fields that take directional derivatives of manifold functions and evaluate them at manifold points; #Code(x) and #Code(y) are manifold functions. #Code(define-coordinates) was used to create these operators and functions, see page 27.

Note that \textsf{circular} is an operator---a property inherited from #Code(d/dx) and #Code(d/dy).

#FootnoteRef(9) The differential of a manifold function will turn out to be a special case of the exterior derivative, which will be introduced later.

#FootnoteRef(10) The Kronecker delta $\delta^i_j$ is one if $i = j$ and zero otherwise.

#FootnoteRef(11) The analogous recovery of coefficient tuples from vector fields is equation
(3.3): $b^i_{\chi, \mathsf{v}} = \mathsf{v}(\chi^i) \circ \chi^{-1}$.

#FootnoteRef(12) The procedure #Code(components->1form-field) is analogous to the procedure #Code(components->vector-field) introduced earlier.

#FootnoteEnd
