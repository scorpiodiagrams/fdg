!!Polyglot
#page(0)

# Chapter 6: Over a Map

To deal with motion on manifolds we need to think about paths on manifolds and vectors along these paths. Tangent vectors along paths are not vector fields on the manifold because they are defined only on the path. And the path may even cross itself, which would give more than one vector at a point. Here we introduce the concept of a /vector field over a map/.#Footnote(1) A vector field over a map assigns a vector to each image point of the map. In general the map may be a function from one manifold to another. If the domain of the map is the manifold of the real line, the range of the map is a 1-dimensional path on the target manifold. One possible way to define a vector field over a map is to assign a tangent vector to each image point of a path, allowing us to work with tangent vectors to paths. A /one-form field over the map/ allows us to extract the components of a vector field over the map.

###  Vector Fields Over a Map

Let μ be a map from points $\mathsf{n}$ in the manifold $\mathsf{N}$ to points $\mathsf{m}$ in the manifold $\mathsf{M}$. A vector over the map μ takes directional derivatives of functions on $\mathsf{M}$ at points $\mathsf{m} = \mu(\mathsf{n})$. The vector over the map applied to the function on $\mathsf{M}$ is a function on $\mathsf{N}$.

### Restricted Vector Fields

One way to make a vector field over a map is to restrict a vector field on $\mathsf{M}$ to the image of $\mathsf{N}$ over μ, as illustrated in figure 6.1.

Let $\mathsf{v}$ be a vector field on $\mathsf{M}$, and $\mathsf{f}$ a function on $\mathsf{M}$. Then

$$\begin{equation}
\mathsf{v}_{\mu}(\mathsf{f}) = \mathsf{v}(\mathsf{f}) \circ \mu,
\end{equation}$$

is a vector over the map μ. Note that $\mathsf{v}_{\mu}(\mathsf{f})$ is a function on $\mathsf{N}$, not $\mathsf{M}$:

$$\begin{equation}
\mathsf{v}_{\mu}(\mathsf{f})(\mathsf{n}) = \mathsf{v}(\mathsf{f})(\mu(\mathsf{n})).
\end{equation}$$

We can implement this definition as:

```Scheme
(define ((vector-field->vector-field-over-map mu:N->M) v-on-m)
(procedure->vector-field
(lambda (f-on-M)
(compose (v-on-M f-on-M) mu:N->M))))
```

### Differential of a Map

Another way to construct a vector field over a map μ is to transport a vector field from the source manifold $\mathsf{N}$ to the target manifold $\mathsf{M}$ with the /differential/ of the map

$$\begin{equation}
d\mu(\mathsf{v})(\mathsf{f})(\mathsf{n}) = \mathsf{v}(\mathsf{f}\circ\mu)(\mathsf{n}),
\end{equation}$$

which takes its argument in the source manifold $\mathsf{N}$. The differential of a map μ applied to a vector field $\mathsf{v}$ on $\mathsf{N}$ is a vector field over the map. A procedure to compute the differential is:

```Scheme
(define (((differential mu) v) f)
(v (compose f mu)))
```

The nomenclature of this subject is confused. The "differential of a map between manifolds," $d\mu$, takes one more argument than the "differential of a real-valued function on a manifold," $\mathsf{d}\mathsf{f}$, but when the target manifold of μ is the reals and $I$ is the identity function on the reals,

$$\begin{equation}
d\mu(\mathsf{v})(I)(\mathsf{n}) = (\mathsf{v}(I\circ\mu))(\mathsf{n}) = (\mathsf{v}(\mu))(\mathsf{n}) =  \mathsf{d}\mu(\mathsf{v})(\mathsf{n}).
\end{equation}$$

We avoid this problem in our notation by distinguishing $d$ and $\mathsf{d}$. In our programs we encode $d$ as differential and $\mathsf{d}$ as d.

### Velocity at a Time

Let μ be the map from the time line to the manifold $\mathsf{M}$, and ${\partial}/{\partial\mathsf{t}}$ be a basis vector on the time line. Then $d\mu({\partial}/{\partial\mathsf{t}})$ is the vector over the map μ that computes the rate of change of functions on $\mathsf{M}$ along the path that is the image of μ. This is the velocity vector. We can use the differential to assign a velocity vector to each moment, solving the problem of multiple vectors at a point if the path crosses itself.

### One-Form Fields Over a Map

Given a one-form ω on the manifold $\mathsf{M}$, the one-form over the map $\mu:\mathsf{N} \to \mathsf{M}$ is constructed as follows:

$$\begin{equation}
\omega^{\mu}(\mathsf{v}_{\mu})(\mathsf{n}) = \omega(\mathsf{u})(\mu(\mathsf{n})) \text{, where } \mathsf{u}(\mathsf{f})(\mathsf{m}) = \mathsf{v}_{\mu}(\mathsf{f})(\mathsf{n}).
\end{equation}$$

The object $\mathsf{u}$ is not really a vector field on $\mathsf{M}$ even though we have given it that shape so that the dual vector can apply to it; $\mathsf{u}(\mathsf{f})$ is evaluated only at images $\mathsf{m} = \mu(\mathsf{n})$ of points $\mathsf{n}$ in $\mathsf{N}$. If we were defining $\mathsf{u}$ as a vector field we would need the inverse of μ to find the point $\mathsf{n} = \mu^{-1}(\mathsf{m})$, but this is not required to define the object $\mathsf{u}$ in a context where there is already an $\mathsf{m}$ associated with the $\mathsf{n}$ of interest. To extend this idea to $k$-forms, we carry each vector argument over the map.

The procedure that constructs a $k$-form over the map from a $k$-form is:

```Scheme
(define ((form-field->form-field-over-map mu:N->M) w-on-M)
(define (make-fake-vector-field V-over-mu n)
(define ((u f) m)
((V-over-mu f) n))
(procedure->vector-field u))
(procedure->nform-field
(lambda vectors-over-map
(lambda (n)
((apply w-on-M
      (map (lambda (V-over-mu)
             (make fake-vector-field V-over-mu n))
           vectors-over-map))
(mu:N->M n))))
(get-rank w-on-M)))
```

The internal procedure make-fake-vector-field counterfeits a vector field $\mathsf{u}$ on $\mathsf{M}$ from the vector field over the map $\mu:\mathsf{N} \to \mathsf{M}$. This works here because the only value that is ever passed as m is (mu:N->M n).

### Basis Fields Over a Map

Let $\mathsf{e}$ be a tuple of basis vector fields, and $\tilde{\mathsf{e}}$ be the tuple of basis one-forms that is dual to $\mathsf{e}$:

$$\begin{equation}
\tilde{\mathsf{e}}^{i}(\mathsf{e}_{j})(\mathsf{m}) = \delta^{i}_{j}.
\end{equation}$$

The /basis vectors/ over the map, $\mathsf{e}^{\mu}$, are particular cases of vectors over a map:

$$\begin{equation}
\mathsf{e}^{\mu}(\mathsf{f}) = \mathsf{e}(\mathsf{f}) \circ \mu .
\end{equation}$$

And the elements of the /dual basis over the map/,
$\tilde{\mathsf{e}}_{\mu}$, are particular cases of one-forms over the map. The basis and dual basis over the map satisfy

$$\begin{equation}
\tilde{\mathsf{e}}^{i}_{\mu}(\mathsf{e}^{\mu}_{j})(\mathsf{n}) = \delta^{i}_{j}.
\end{equation}$$

### Walking on a Sphere

For example, let $\mu$ map the time line to the unit sphere.#Footnote(2) We use colatitude $\theta$ and longitude $\phi$ as coordinates on the sphere:

```Scheme
(define S2 (make-manifold S^2 2 3))
(define S2-spherical
(coordinate-system at 'spherical 'north-pole S2))
(define-coordinates (up theta phi) S2-spherical)
(define S2-basis (coordinate-system->basis S2-spherical))
```

A general path on the sphere is:#Footnote(3)

```Scheme
(define mu
(compose (point S2-spherical)
  (up (literal-function 'theta)
      (literal-function 'phi))
  (chart R1-rect)))
```

The basis over the map is constructed from the basis on the sphere:

```Scheme
(define S2-basis-over-mu
(basis->basis-over-map mu S2-basis))

(define h
(literal-manifold-function 'h-spherical S2-spherical))
```

```Scheme
(((basis->vector-basis S2-basis-over-mu) h)
((point R1-rect) 't0))
;; (down
;;  (((partial 0) h-spherical) (up (theta t0) (phi t0)))
;;  (((partial 1) h-spherical) (up (theta t0) (phi t0))))
```

The basis vectors over the map compute derivatives of the function $h$ evaluated on the path at the given time.

We can check that the dual basis over the map does the correct thing:

```Scheme
(((basis->1form-basis S2-basis-over-mu)
(basis->vector-basis S2-basis-over-mu))
((point R1-rect) 't0))
;; (up (down 1 0) (down 0 1))
```

### Components of the Velocity

Let χ be a tuple of coordinates on $\mathsf{M}$, with associated basis vectors $\mathsf{X}_{i}$, and dual basis elements $\mathsf{d}\mathsf{x}^{i}$. The vector basis and dual basis over the map μ are $\mathsf{X}^{\mu}_{i}$ and $\mathsf{d}\mathsf{x}^{i}_{\mu}$. The components of the velocity (rates of change of coordinates along the path μ) are obtained by applying the dual basis over the map to the velocity

$$\begin{equation}
v^{i}(t) = \mathsf{d}\mathsf{x}^{i}_{\mu}(d\mu({\partial}/{\partial\mathsf{t}}))(\mathsf{t}),
\end{equation}$$

where $t$ is the coordinate for the point $\mathsf{t}$.

For example, the coordinate velocities on a sphere are

```Scheme
(((basis->1form-basis S2-basis-over-mu)
((differential mu) d/dt))
((point R1-rect) 't0))
;; (up ((D theta) t0) ((D phi) t0)))
```

as expected.

### Pullbacks and Pushforwards

Maps from one manifold to another can also be used to relate the vector fields and one-form fields on one manifold to those on the other. We have introduced two such relations: restricted vector fields and the differential of a function. However, there are other ways to relate the vector fields and form fields on different manifolds that are connected by a map.

### Pullback and Pushforward of a Function

The /pullback/ of a function $\mathsf{f}$ on $\mathsf{M}$ over the map μ is defined as

$$\begin{equation}
\mu^{*} \mathsf{f} = \mathsf{f} \circ \mu .
\end{equation}$$

This allows us to take a function defined on $\mathsf{M}$ and use it to define a new function on $\mathsf{N}$.

For example, the integral curve of $\mathsf{v}$ evolved for time $t$ as a function of the initial manifold point $\mathsf{m}$ generates a map $\phi^{\mathsf{v}}_{t}$ of the manifold onto itself. This is a simple currying#Footnote(4) of the integral curve of $\mathsf{v}$ from $\mathsf{m}$ as a a function of time: $\phi^{\mathsf{v}}_{t}(\mathsf{m}) = \gamma^{\mathsf{v}}_{\mathsf{m}}(t)$. The evolution of the function $\mathsf{f}$ along an integral curve, equation (3.33), can be written in terms of the pullback over $\phi^{\mathsf{v}}_{t}$:

$$\begin{equation}
(\mathsf{E}_{t,\mathsf{v}}\mathsf{f})(\mathsf{m}) = \mathsf{f}(\phi^{\mathsf{v}}_{t}(\mathsf{m})) = ((\phi^{\mathsf{v}}_{t})^{*}\mathsf{f})(\mathsf{m}).
\end{equation}$$

This is implemented as:

```Scheme
(define ((pullback-function mu:N->M) f-on-m)
(compose f-on-M mu:N->M))
```

A vector field over the map that was constructed by restriction (equation 6.1) can be seen as the pullback of the function constructed by application of the vector field to a function:

$$\begin{equation}
\mathsf{v}_{\mu}(\mathsf{f}) = \mathsf{v}(\mathsf{f}) \circ \mu = \mu^{*} (\mathsf{v}(\mathsf{f})).
\end{equation}$$

A vector field over the map that was constructed by a differential (equation 6.3) can be seen as the vector field applied to the pullback of the function:

$$\begin{equation}
d\mu(\mathsf{v})(\mathsf{f})(\mathsf{n}) = \mathsf{v}(\mathsf{f}\circ\mu)(\mathsf{n}) = \mathsf{v}(\mu^{*} \mathsf{f})(\mathsf{n}).
\end{equation}$$

If we have an inverse for the map μ we can also define a /push-forward/ of the function $\mathsf{g}$, defined on the source manifold of the map:#Footnote(5)

$$\begin{equation}
\mu_{*}\mathsf{g} = \mathsf{g} \circ \mu^{-1}.
\end{equation}$$

### Pushforward of a Vector Field

We can also define the /pushforward/ of a vector field over the map μ. The pushforward takes a vector field $\mathsf{v}$ defined on $\mathsf{N}$. The result takes directional derivatives of functions on $\mathsf{M}$ at a place determined by a point in $\mathsf{M}$:

$$\begin{equation}
\mu_{*}\mathsf{v}(\mathsf{f})(\mathsf{m}) = \mathsf{v}(\mu^{*} \mathsf{f})(\mu^{-1}(\mathsf{m})) = \mathsf{v}(\mathsf{f}\circ\mu)(\mu^{-1}(\mathsf{m})),
\end{equation}$$

or

$$\begin{equation}
\mu_{*}\mathsf{v}(\mathsf{f}) = \mu_{*}(\mathsf{v}(\mu^{*} \mathsf{f})).
\end{equation}$$

Here we expressed the pushforward of the vector field in terms of pullbacks and pushforwards of functions. Note that the pushforward requires the inverse of the map.

If the map is from time to some configuration manifold and represents the time evolution of a process, we can think of the pushforward of a vector field as a velocity measured at a point on the trajectory in the configuration manifold. By contrast, the differential of the map applied to the vector field gives us the velocity vector at each moment in time. Because a trajectory may cross itself, the pushforward is not defined at any point where the crossing occurs, but the differential is always defined.

### Pushforward Along Integral Curves

We can push a vector field forward over the map generated by an integral curve of a vector field $\mathsf{w}$, because the inverse is always available.#Footnote(6)

$$\begin{equation}
((\phi^{\mathsf{w}}_{t})_{*}\mathsf{v})(\mathsf{f})(\mathsf{m}) = \mathsf{v}((\phi^{\mathsf{w}}_{t})^{*}\mathsf{f})(\phi^{\mathsf{w}}_{-t}(\mathsf{m})) = \mathsf{v}(\mathsf{f}\circ\phi^{\mathsf{w}}_{t})(\phi^{\mathsf{w}}_{-t}(\mathsf{m})).
\end{equation}$$

This is implemented as:

```Scheme
(define ((pushforward-vector mu:N->M mu^-1:M->N) v-on-N)
(procedure->vector-field
(lambda (f)
(compose (v-on-N (compose f mu:N->M)) mu^-1:M->N))))
```

### Pullback of a Vector Field

Given a vector field $\mathsf{v}$ on a manifold $\mathsf{M}$ we can pull the vector field back through the map $\mu:\mathsf{N}\to\mathsf{M}$ as follows:

$$\begin{equation}
\mu^{*}\mathsf{v}(\mathsf{f})(\mathsf{n}) = (\mathsf{v}(\mathsf{f}\circ\mu^{-1}))(\mu(\mathsf{n}))
\end{equation}$$

or

$$\begin{equation}
\mu^{*}\mathsf{v}(\mathsf{f}) = \mu^{*}(\mathsf{v}(\mu_{*}\mathsf{f})).
\end{equation}$$

This may be useful when the map is invertible, as in the flow generated by a vector field.

This is implemented as:

```Scheme
(define (pullback-vector-field mu:N->M mu^-1:M->N)
(pushforward-vector mu^-1:M->N mu:N->M))
```

### Pullback of a Form Field

We can also pull back a one-form field ω defined on $\mathsf{M}$, but an honest definition is rarely written. The pullback of a one-form field applied to a vector field is intended to be the same as the one-form field applied to the pushforward of the vector field.

The pullback of a one-form field is often described by the relation

$$\begin{equation}
\mu^{*}\omega(\mathsf{v}) = \omega(\mu_{*}\mathsf{v}),
\end{equation}$$

but this is wrong, because the two sides are not functions of points in the same manifold. The one-form field ω applies to a vector field on the manifold $\mathsf{M}$, which takes a directional derivative of a function defined on $\mathsf{M}$ and is evaluated at a point on $\mathsf{M}$, but the left-hand side is evaluated at a point on the manifold $\mathsf{N}$.

A more precise description would be

$$\begin{equation}
\mu^{*}\omega(\mathsf{v})(\mathsf{n}) = \omega(\mu_{*}\mathsf{v})(\mathsf{\mu}(\mathsf{n}))
\end{equation}$$

or

$$\begin{equation}
\mu^{*}\omega(\mathsf{v}) = \mu^{*}(\omega(\mu_{*}\mathsf{v})).
\end{equation}$$

Although this is accurate, it may not be effective, because computing the pushforward requires the inverse of the map μ. But the inverse is available when the map is the flow generated by a vector field.

In fact it is possible to compute the pullback of a one-form field without having the inverse of the map. Instead we can use form-field->form-field-over-map to avoid needing the inverse:

$$\begin{equation}
\mu^{*}\omega(\mathsf{v})(\mathsf{n}) = \omega^{\mu}(d\mu(\mathsf{v}))(n).
\end{equation}$$

The pullback of a $k$-form generalizes equation 6.21:

$$\begin{equation}
\mu^{*}\omega(\mathsf{u},\mathsf{v},\ldots)(\mathsf{n}) = \omega(\mu_{*},\mathsf{u},\mu_{*},\mathsf{v},\ldots)(\mu(\mathsf{n})).
\end{equation}$$

This is implemented as follows:#Footnote(7)

```Scheme
(define ((pullback-form mu:N->M) omega-on-M)
(let ((k (get-rank omega-on-M)))
(if (= k 0)
((pullback function mu:N->M) omega-on-M)
(procedure->nform-field
(lambda vectors-on-N
  (apply ((form-field->form-field-over-map mu:N->M)
          omega-on-M)
         (map (differential mu:N->M) vectors-on-N)))
k))))
```

### Properties of Pullback

The pullback through a map has many nice properties: it distributes through addition and through wedge product:

$$\begin{equation}
\mu^{*}(\theta + \phi) = \mu^{*}\theta + \mu^{*}\phi ,
\end{equation}$$

$$\begin{equation}
\mu^{*}(\theta \wedge \phi) = \mu^{*}\theta \wedge \mu^{*}\phi .
\end{equation}$$

The pullback also commutes with the exterior derivative:

$$\begin{equation}
\mathsf{d}(\mu^{*}\theta) = \mu^{*}(\mathsf{d}\theta),
\end{equation}$$

for $\theta$ a function or $k$-form field.

We can verify this by computing an example. Let $\mu$ map the rectangular plane to rectangular 3-space:

```Scheme
(define mu (literal-manifold-map 'MU R2-rect R3-rect))
```

First, let's compare the pullback of the exterior derivative of a function with the exterior derivative of the pullback of the function:

```Scheme
(define f (literal-manifold-function 'f-rect R3-rect))
(define X (literal-vector-field 'X-rect R2-rect))
```

```Scheme
(((- ((pullback mu) (d f)) (d ((pull back mu) f))) X)
((point R2-rect) (up 'x0 'y0)))
;; 0
```

More generally, we can consider what happens to a form field. For a one-form field the result is as expected:

```Scheme
(define theta (literal-1form-field 'THETA R3-rect))
(define Y (literal-vector-field 'Y-rect R2-rect))
```

```Scheme
(((- ((pullback mu) (d theta)) (d ((pullback mu) theta))) X Y)
((point R2-rect) (up 'x0 'y0)))
;; 0
```

### Pushforward of a Form Field

By symmetry, it is possible to define the pushforward of a one-form field as

$$\begin{equation}
\mu_{*}\omega(\mathsf{v}) = \mu_{*}(\omega(\mu^{*}v)),
\end{equation}$$

but this is rarely useful.

### Exercise 6.1: Velocities on a Globe

We can use manifold functions, vector fields, and one-forms over a map to understand how paths behave.

a. Suppose that a vehicle is traveling east on the Earth at a given rate of change of longitude. What is the actual ground speed of the vehicle?

b. Stereographic projection is useful for navigation because it is conformal (it preserves angles). For the situation of part a, what is the speed measured on a stereographic map? Remember that the stereographic projection is implemented with S2-Riemann.

----
### Footnotes


#FootnoteRef(1) See Bishop and Goldberg, /Tensor Analysis on Manifolds/ [3](references!bib_3).

#FootnoteRef(2) We execute #Code((define-coordinates t R1-rect#)) to make #Code(t) the coordinate function of the real line.

#FootnoteRef(3) We provide a shortcut to make literal manifold maps:

```Scheme
(define mu (literal-manifold-map 'mu R1-rect S2-spherical))
```

But if we used this shortcut, the component functions would be named mu^0 and mu^1. Here we wanted to use more mnemonic names for the component functions.

#FootnoteRef(4) A function of two arguments may be seen as a function of one argument whose value is a function of the other argument. This can be done in two different ways, depending on which argument is supplied first. The general process of specifying a subset of the arguments to produce a new function of the others is called /currying/ the function, in honor of the logician Haskell Curry (1900-1982) who, with Moses Schönfinkel (1889-1942), developed combinatory logic.

#FootnoteRef(5) Notation note: superscript asterisk indicates pullback, subscript asterisk indicates pushforward. Pullbacks and pushforwards are tightly binding operators, so, for example $\mu^{*}f(\mathsf{n})=(\mu^{*}f)(\mathsf{n})$.

#FootnoteRef(6) The map $\phi^{\mathsf{w}}_{t}$ is always invertible: $(\phi^{\mathsf{w}}_{t})^{-1} = \phi^{\mathsf{w}}_{-t}$ because of the uniqueness of the solutions of the initial-value problem for ordinary differential equations.

#FootnoteRef(7) There is a generic pullback procedure that operates on any kind of manifold object. However, to pull a vector field back requires providing the inverse map.