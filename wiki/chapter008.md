!!Polyglot
#page(0)

# Chapter 8: Curvature

If the intrinsic curvature of a manifold is not zero, a vector parallel-transported around a small loop will end up different from the vector that started. We saw the consequence of this before, on page 1 and on page 93. The Riemann tensor encapsulates this idea.

The Riemann curvature operator is

$$\begin{equation}
\mathcal{R}(\mathsf{w}, \mathsf{v}) = [\nabla_\mathsf{w}, \nabla_\mathsf{v}] - \nabla_{[\mathsf{w}, \mathsf{v}]}.
\end{equation}$$

The traditional Riemann tensor is#Footnote(1)

$$\begin{equation}
\mathcal{R}(\boldsymbol{\omega}, \mathsf{u}, \mathsf{v}, \mathsf{w}) \
\boldsymbol{\omega}\left(\left(\mathcal{R}(\mathsf{w}, \mathsf{v})\right)(\mathsf{u})\right),
\end{equation}$$

where $\boldsymbol{\omega}$ is a one-form field that measures the incremental change in the vector field $\mathsf{u}$ caused by parallel-transporting it around the loop defined by the vector fields $\mathsf{w}$ and $\mathsf{v}$. $\mathsf{R}$ allows us to compute the /intrinsic curvature/ of a manifold at a point.

The Riemann curvature is computed by

```Scheme
(define ((Riemann-curvature nabla) w v)
(- (commutator (nabla w) (nabla v))
(nabla (commutator w v))))
```

The #Code(Riemann-curvature) procedure is parameterized by the relevant covariant-derivative operator #Code(nabla), which implements $\nabla$. The #Code(nabla) is itself dependent on the connection, which provides the details of the local geometry. The same #Code(Riemann-curvature) procedure works for ordinary covariant derivatives and for covariant derivatives over a map. Given two vector fields, the result of =((Riemann-curvature nabla) w v)= is a procedure that takes a vector field and produces a vector field so we can implement the Riemann tensor as

```Scheme
(define ((Riemann nabla) omega u w v)
(omega (((Riemann-curvature nabla) w v) u)))
```

So, for example,#Footnote(2)

```Scheme
(((Riemann (covariant-derivative sphere-Cartan))
dphi d/dtheta d/dphi d/dtheta)
((point S2-spherical) (up 'theta0 'phi0)))
;; 1
```

Here we have computed the φ component of the result of carrying a $\partial / \partial \theta$ basis vector around the parallelogram defined by $\partial / \partial \phi$ and $\partial / \partial \theta$. The result shows a net rotation in the $\phi$ direction.

Most of the sixteen coefficients of the Riemann tensor for the sphere are zero. The following are the nonzero coefficients:

$$\begin{equation}
\begin{aligned}
\mathsf{R}\left(\mathsf{d}\theta, \frac{\partial}{\partial \phi}, \frac{\partial}{\partial \theta}, \frac{\partial}{\partial \phi} \right) \
\left(\chi^{-1} \left(q^\theta, q^\phi \right) \right) &= \left(\sin \left(q^\theta \right) \right)^2, \\
\mathsf{R}\left(\mathsf{d}\theta, \frac{\partial}{\partial \phi}, \frac{\partial}{\partial \phi}, \frac{\partial}{\partial \theta} \right) \
\left(\chi^{-1} \left(q^\theta, q^\phi \right) \right) &= -\left(\sin \left(q^\theta \right) \right)^2, \\
\mathsf{R}\left(\mathsf{d}\phi, \frac{\partial}{\partial \theta}, \frac{\partial}{\partial \theta}, \frac{\partial}{\partial \phi} \right) \
\left(\chi^{-1} \left(q^\theta, q^\phi \right) \right) &= -1, \\
\mathsf{R}\left(\mathsf{d}\phi, \frac{\partial}{\partial \theta}, \frac{\partial}{\partial \phi}, \frac{\partial}{\partial \theta} \right) \
\left(\chi^{-1} \left(q^\theta, q^\phi \right) \right) &= 1.
\end{aligned}
\end{equation}$$

### Explicit Transport

We will show that the result of the Riemann calculation of the change in a vector, as we traverse a loop, is what we get by explicitly calculating the transport. The coordinates of the vector to be transported are governed by the differential equations (see equation 7.72)

$$\begin{equation}
D u^i(t)=-\sum_j \varpi_j^i(\mathsf{v})\left(\chi^{-1}(\sigma(t))\right) u^j(t)
\end{equation}$$

and the coordinates as a function of time, $\sigma = \chi \circ \gamma \circ \chi_\mathsf{R}^{-1}$, of the path $\gamma$, are governed by the differential equations#Footnote(3)

$$\begin{equation}
D\sigma(t) = \mathsf{v}(\chi)\left(\chi^{-1}(\sigma(t))\right).
\end{equation}$$

We have to integrate these equations (8.4, 8.5) together to transport the vector over the map $\mathsf{u}_\gamma$ a finite distance along the vector field $\mathsf{v}$.

Let $s(t)=(\sigma(t), u(t))$ be a state tuple, combining $\sigma$ the coordinates of $\gamma$, and $u$ the coordinates of $\mathsf{u}_\gamma$. Then

$$\begin{equation}
Ds(t) = \left(D\sigma(t), Du(t)\right) = g(s(t)),
\end{equation}$$

where $g$ is the tuple of right-hand sides of equations (8.4, 8.5).

The differential equations describing the evolution of a function $h$ of state $s$ along the state path are

$$\begin{equation}
D(h \circ s) = (Dh \circ s)(g \circ s) = L_g h \circ s,
\end{equation}$$

defining the operator $L_g$.

Exponentiation gives a finite evolution:#Footnote(4)

$$\begin{equation}
h(s(t+\epsilon)) = \left(e^{\epsilon L_g} h\right)\left(s(t)\right).
\end{equation}$$

The finite parallel transport of the vector with components $u$ is

$$\begin{equation}
u(t+\epsilon) = \left(e^{\epsilon L_g} U\right)\left(s(t)\right),
\end{equation}$$

where the selector $U(\sigma, u) = u$, and the initial state is $s(t) =
(\sigma(t), u(t))$.

Consider parallel-transporting a vector $\mathsf{u}$ around a parallelogram defined by two coordinate-basis vector fields $\mathsf{w}$ and $\mathsf{v}$. The vector $\mathsf{u}$ is really a vector over a map, where the map is the parametric curve describing our parallelogram. This map is implicitly defined in terms of the vector fields $\mathsf{w}$ and $\mathsf{v}$. Let $g_w$ and $g_v$ be the right-hand sides of the differential equations for parallel transport along $\mathsf{w}$ and $\mathsf{v}$ respectively. Then evolution along $\mathsf{w}$ for interval $\epsilon$, then along $\mathsf{v}$ for interval $\epsilon$, then reversing $\mathsf{w}$, and reversing $\mathsf{v}$, brings $\sigma$ back to where it started to second order in $\epsilon$.

The state $s = (σ, u)$ after transporting $s_0$ around the loop is#Footnote(5)

$$\begin{equation}
\begin{aligned}
&\left(e^{-\epsilon L_{g_v}} I\right) \circ \left(e^{-\epsilon L_{g_w}} I\right) \circ \left(e^{\epsilon L_{g_v}} I\right) \circ \left(e^{\epsilon L_{g_w}} I \right) \left(s_0 \right) \\ 
&\quad=\left(e^{\epsilon L_{g_w}} e^{\epsilon L_{g_v}} e^{-\epsilon L_{g_w}} e^{-\epsilon L_{g_v}} I\right)\left(s_0\right) \\
&\quad=\left(e^{\epsilon^2\left[L_{g_w}, L_{g_v}\right]+\cdots} I\right)\left(s_0\right).
\end{aligned}
\end{equation}$$

So the lowest-order change in the transported vector is

$$\begin{equation}
\epsilon^2 U \left(\left(\left[L_{g_w}, L_{g_v}\right] I\right)(s_0)\right),
\end{equation}$$

where $U(\sigma, u) = u$.

However , if $\mathsf{w}$ and $\mathsf{v}$ do not commute, the indicated loop does not bring $\sigma$ back to the starting point, to second order in $\epsilon$. We must account for the commutator. (See figure 4.2.) In the general case the lowest order change in the transported vector is

$$\begin{equation}
\epsilon^2 U \left(\left(\left(\left[L_{g_w}, L_{g_v}\right] - L_{g_{[w, v]}}\right) I\right)(s_0)\right),
\end{equation}$$

This is what the Riemann tensor computation gives, scaled by $\epsilon^2$.

#### Verification in Two Dimensions

We can verify this in two dimensions. We need to make the structure representing a state:

```Scheme
(define (make-state sigma u) (vector sigma u))
(define (Sigma state) (ref state 0))
(define (U-select state) (ref state 1))
```

And now we get to the meat of the matter: First we find the rate of change of the components of the vector $\mathsf{u}$ as we carry it along the vector field $\mathsf{v}$.#Footnote(6)

```Scheme
(define ((Du v) state)
(let ((CF (Cartan->forms general-Cartan-2)))
(* -1
((CF v) (Chi-inverse (Sigma state)))
(U-select state))))
```

We also need to determine the rate of change of the coordinates of the integral curve of $\mathsf{v}$.

```Scheme
(define ((Dsigma v) state)
((v Chi) (Chi-inverse (Sigma state))))
```

Putting these together to make the derivative of the state vector

```Scheme
(define ((g v) state)
(make-state ((Dsigma v) state) ((Du v) state)))
```

gives us just what we need to construct the differential operator for evolution of the combined state:

```Scheme
(define (L v)
(define ((l h) state)
(* ((D h) state) ((g v) state)))
(make-operator l))
```

So now we can demonstrate that the lowest-order change resulting from explicit parallel transport of a vector around an infinitesimal loop is what is computed by the Riemann curvature.

```Scheme
(let ((U (literal-vector-field 'U-rect R2-rect))
(W (literal-vector-field 'W-rect R2-rect))
(V (literal-vector-field 'V-rect R2-rect))
(sigma (up 'sigma0 'sigma1)))
(let ((nabla (covariant-derivative general-Cartan-2))
(m (Chi-inverse sigma)))
(let ((s (make-state sigma ((U Chi) m))))
(- (((- (commutator (L V) (L W))
(L (commutator V W)))
U-select)
s)
(((((Riemann-curvature nabla) W V) U) Chi) m)))))
;; (up 0 0)
```

#### Geometrically

The explicit transport above was done with differential equations operating on a state consisting of coordinates and components of the vector being transported. We can simplify this so that it is entirely built on manifold objects, eliminating the state. After a long algebraic story we find that

$$\begin{equation}
\begin{aligned}
&((\mathcal{R}(\mathsf{w}, \mathsf{v}))(\mathsf{u}))(\mathsf{f}) \\
&\quad=\mathsf{e}(\mathsf{f})\{(\mathsf{w}(\varpi(\mathsf{v}))-\mathsf{v}(\varpi(\mathsf{w}))-\varpi([\mathsf{w}, \mathsf{v}])) \tilde{\mathsf{e}}(\mathsf{u}) \\
&\quad+\varpi(\mathsf{w}) \varpi(\mathsf{v}) \tilde{\mathsf{e}}(\mathsf{u})-\varpi(\mathsf{v}) \varpi(\mathsf{w}) \tilde{\mathsf{e}}(\mathsf{u})\}
\end{aligned}
\end{equation}$$

or as a program:

```Scheme
(define ((((curvature-from-transport Cartan) w v) u) f)
(let* ((CF (Cartan->forms Cartan))
(basis (Cartan->basis Cartan))
(fi (basis->1form-basis basis))
(ei (basis->vector-basis basis)))
(* (ei f)
(+ (* (- (- (w (CF v)) (v (CF w)))
(CF (commutator w v)))
(fi u))
(- (* (CF w) (* (CF v) (fi u)))
(* (CF v) (* (CF w) (fi u))))))))
```

This computes the same operator as the traditional Riemann curvature operator:

```Scheme
(define (test coordsys Cartan)
(let ((m (typical-point coordsys))
(u (literal-vector-field 'u-coord coordsys))
(w (literal-vector-field 'w-coord coordsys))
(v (literal-vector-field 'v-coord coordsys))
(f (literal-manifold-function 'f-coord coordsys)))
(let ((nabla (covariant-derivative Cartan)))
(- (((((curvature-from-transport Cartan) w v) u) f) m)
(((((Riemann-curvature nabla) w v) u) f) m)))))
```

```Scheme
(test R2-rect general-Cartan-2)
;; 0
```

```Scheme
(test R2-polar general-Cartan-2)
;; 0
```

#### Terms of the Riemann Curvature

Since the Riemann curvature is defined as in equation (8.1),

$$\begin{equation}
\mathcal{R}(\mathsf{w}, \mathsf{v}) = [\nabla_\mathsf{w}, \nabla_\mathsf{v}] - \nabla_{[\mathsf{w}, \mathsf{v}]},
\end{equation}$$

it is natural#Footnote(7) to identify these terms with the corresponding terms in

$$\begin{equation}
\left(\left(\left[L_{g_w}, L_{g_v}\right] - L_{g_{[w,v]}}\right)U\right)(s_0).
\end{equation}$$

Unfortunately, this does not work, as demonstrated below:

```Scheme
(let ((U (literal-vector-field 'U-rect R2-rect))
(V (literal-vector-field 'V-rect R2-rect))
(W (literal-vector-field 'W-rect R2-rect))
(nabla (covariant-derivative general-Cartan-2))
(sigma (up 'sigma0 'sigma1)))
(let ((m (Chi-inverse sigma)))
(let ((s (make-state sigma ((U Chi) m))))
(- (((commutator (L W) (L V)) U-select) s)
((((commutator (nabla W) (nabla V)) U) Chi)
m)))))
;; a nonzero mess
```

The obvious identification does not work, but neither does the other one!

```Scheme
(let ((U (literal-vector-field 'U-rect R2-rect))
(V (literal-vector-field 'V-rect R2-rect))
(W (literal-vector-field 'W-rect R2-rect))
(nabla (covariant-derivative general-Cartan-2))
(sigma (up 'sigma0 'sigma1)))
(let ((m (Chi-inverse sigma)))
(let ((s (make-state sigma ((U Chi) m))))
(- (((commutator (L W) (L V)) U-select) s)
((((nabla (commutator W V)) U) Chi)
m)))))
;; a nonzero mess
```

Let's compute the two parts of the Riemann curvature operator and see how this works out. First, recall

$$\begin{equation}
\begin{aligned}
\nabla_\mathsf{v} \mathsf{u}(\mathsf{f}) &= \sum_i \mathsf{e}_i(\mathsf{f})\left(\mathsf{v}\left(\tilde{\mathsf{e}}^i(\mathsf{u})\right) \
+ \sum_j \varpi_j^i(\mathsf{v}) \tilde{\mathsf{e}}^j(\mathsf{u})\right) \\
&=\mathsf{e}(\mathsf{f})(\mathsf{v}(\tilde{\mathsf{e}}(\mathsf{u})) \
+ \varpi(\mathsf{v}) \tilde{\mathsf{e}}(\mathsf{u})),
\end{aligned}
\end{equation}$$

where the second form uses tuple arithmetic. Now let's consider the first part of the Riemann curvature operator:

$$\begin{equation}
\begin{aligned}
\left[\nabla_\mathsf{w}, \nabla_\mathsf{v} \right] \mathsf{u} \\
=& \nabla_\mathsf{w} \nabla_\mathsf{v} \mathsf{u} - \nabla_\mathsf{v} \nabla_\mathsf{w} \mathsf{u} \\
=& \mathsf{e}\{\mathsf{w}(\mathsf{v}(\tilde{\mathsf{e}}(\mathsf{u})) + \varpi(\mathsf{v}) \tilde{\mathsf{e}}(\mathsf{u})) \
+ \varpi(\mathsf{w})(\mathsf{v}(\tilde{\mathsf{e}}(\mathsf{u})) + \varpi(\mathsf{v}) \tilde{\mathsf{e}}(\mathsf{u}))\} \\
&-\mathsf{e}\{\mathsf{v}(\mathsf{w}(\tilde{\mathsf{e}}(\mathsf{u}))+\varpi(\mathsf{w}) \tilde{\mathsf{e}}(\mathsf{u})) \
+ \varpi(\mathsf{v})(\mathsf{w}(\tilde{\mathsf{e}}(\mathsf{u})) + \varpi(\mathsf{w}) \tilde{\mathsf{e}}(\mathsf{u}))\} \\
=& \mathsf{e}\{[\mathsf{w}, \mathsf{v}] \tilde{\mathsf{e}}(\mathsf{u})\\
&+\mathsf{w}(\varpi(\mathsf{v})) \tilde{\mathsf{e}}(\mathsf{u}) - \mathsf{v}(\varpi(\mathsf{w})) \tilde{\mathsf{e}}(\mathsf{u}) \\
&+\varpi(\mathsf{w}) \varpi(\mathsf{v}) \tilde{\mathsf{e}}(\mathsf{u})-\varpi(\mathsf{v}) \varpi(\mathsf{w}) \tilde{\mathsf{e}}(\mathsf{u})\}.
\end{aligned}
\end{equation}$$

The second term of the Riemann curvature operator is

$$\begin{equation}
\nabla_{[\mathsf{w}, \mathsf{v}]} \mathsf{u} = \mathsf{e}\left\{[\mathsf{w}, \mathsf{v}]\tilde{\mathsf{e}}(\mathsf{u}) \
+ \varpi\left([\mathsf{w}, \mathsf{v}]\right)\tilde{\mathsf{e}}(u)\right\}.
\end{equation}$$

The difference of these is the Riemann curvature operator. Notice that the first term in each cancels, and the rest gives equation (8.13).

#### Ricci Curvature

One measure of the curvature is the Ricci tensor, which is computed from the Riemann tensor by

$$\begin{equation}
R(\mathsf{u}, \mathsf{v})=\sum_i \mathsf{R}\left(\tilde{\mathsf{e}}^i, \mathsf{u}, \mathsf{e}_i, \mathsf{v}\right).
\end{equation}$$

Expressed as a program:

```Scheme
(define ((Ricci nabla basis) u v)
(contract (lambda (ei wi) ((Riemann nabla) wi u ei v))
basis))
```

Einstein's field equation (9.27) for gravity, which we will encounter later,
is expressed in terms of the Ricci tensor.

### Exercise 8.1: Ricci of a Sphere

Compute the components of the Ricci tensor of the surface of a sphere.

### Exercise 8.2: Pseudosphere

A pseudosphere is a surface in 3-dimensional space. It is a surface of revolution of a tractrix about its asymptote (along the $\hat{z}$-axis). We can make coordinates for the surface $(t, \theta)$ where $t$ is the coordinate along the asymptote and $\theta$ is the angle of revolution. We embed the pseudosphere in rectangular 3-dimensional space with

```Scheme
(define (pseudosphere q)
(let ((t (ref q 0)) (theta (ref q 1)))
(up (* (sech t) (cos theta))
(* (sech t) (sin theta))
(- t (tanh t)))))
```

The structure of Christoffel coefficients for the pseudosphere is

```Scheme
(down
(down (up (/ (+ (* 2 (expt (cosh t) 2) (expt (sinh t) 2))
 (* -2 (expt (sinh t) 4)) (expt (cosh t) 2)
 (* -2 (expt (sinh t) 2)))
(+ (* (cosh t) (expt (sinh t) 3))
 (* (cosh t) (sinh t))))
0)
(up 0
(/ (* -1 (sinh t)) (cosh t))))
(down (up 0
(/ (* -1 (sinh t)) (cosh t)))
(up (/ (cosh t) (+ (expt (sinh t) 3) (sinh t)))
0)))
```

Note that this is independent of $\theta$.

Compute the components of the Ricci tensor.

### Torsion

There are many connections that describe the local properties of any particular manifold. A connection has a property called /torsion/, which is computed as follows:

$$\begin{equation}
\mathcal{T}(\mathsf{u}, \mathsf{v})=\nabla_{\mathsf{u}} \mathsf{v}-\nabla_{\mathsf{v}} \mathsf{u}-[\mathsf{u}, \mathsf{v}].
\end{equation}$$

The torsion takes two vector fields and produces a vector field. The torsion depends on the covariant derivative, which is constructed from the connection.

We account for this dependency by parameterizing the program by #Code(nabla).

```Scheme
(define ((torsion-vector nabla) u v)
(- (- ((nabla u) v) ((nabla v) u))
(commutator u v)))

(define ((torsion nabla) omega u v)
(omega ((torsion-vector nabla) u v)))
```

The torsion for the connection for the 2-sphere specified by the Christoffel coefficients #Code(S2-Christoffel) above is zero. We demonstrate this by applying the torsion to the basis vector fields:

```Scheme
(for-each
(lambda (x)
(for-each
(lambda (y)
(print-expression
((((torsion-vector (covariant-derivative sphere-Cartan))
x y)
(literal-manifold-function 'f S2-spherical))
((point S2-spherical) (up 'theta0 'phi0)))))
(list d/dtheta d/dphi)))
(list d/dtheta d/dphi))
;; 0
;; 0
;; 0
;; 0
```

#### Torsion Doesn't Affect Geodesics

There are multiple connections that give the same geodesic curves. Among these connections there is always one with zero torsion. Thus, if you care about only geodesics, it is appropriate to use a torsion-free connection.

Consider a basis $\mathsf{e}$ and its dual $\tilde{\mathsf{e}}$. The components of the torsion are

$$\begin{equation}
\tilde{\mathsf{e}}\left(\mathsf{T}(\mathsf{e}_i, \mathsf{e}_j)\right) = \Gamma_{ij}^k + \Gamma_{ji}^k + \Gamma_{ij}^k,
\end{equation}$$

where $\mathsf{d}_{ij}^k$ are the structure constants of the basis. See equations (4.37, 4.38). For a commuting basis the structure constants are zero, and the components of the torsion are the antisymmetric part of $\Gamma$ with respect to the lower indices.

Recall the geodesic equation (7.79):

$$\begin{equation}
D^2 \sigma^i(t) = \sum_{jk} \Gamma_{jk}^i(\gamma(t))D\sigma^j(t)D\sigma^k(t = 0.
\end{equation}$$

Observe that the lower indices of $\Gamma$ are contracted with two copies of the velocity. Because the use of $\Gamma$ is symmetrical here, any asymmetry of $\Gamma$ in its lower indices is irrelevant to the geodesics. Thus one can study the geodesics of any connection by first symmetrizing the connection, eliminating torsion. The resulting equations will be simpler.

### Geodesic Deviation

Geodesics may converge and intersect (as in the lines of longitude on a sphere) or they may diverge (for example, on a saddle). To capture this notion requires some measure of the convergence or divergence, but this requires metrics (see Chapter 9). But even in the absence of a metric we can define a quantity, the /geodesic deviation/, that can be interpreted in terms of relative acceleration of neighboring geodesics from a reference geodesic.

Let there be a one-parameter family of geodesics, with parameter $s$, and let $\mathsf{T}$ be the vector field of tangent vectors to those geodesics:

$$\begin{equation}
\nabla_\mathsf{T} \mathsf{T} = 0.
\end{equation}$$

We can parameterize travel along the geodesics with parameter $t$: a geodesic curve $\gamma_s(t) = \phi_t^\mathsf{T}(\mathsf{m}_s)$ where

$$\begin{equation}
\mathsf{f} \circ \phi_t^\mathsf{T}(\mathsf{m}_s) = \left(e^{tT}\mathsf{f}\right)(\mathsf{m}_s).
\end{equation}$$

Let $U = \partial / \partial s$ be the vector field corresponding to the displacement of neighboring geodesics. Locally, $(t, s)$ is a coordinate system on the 2-dimensional submanifold formed by the family of geodesics. The vector fields $\mathsf{T}$ and $\mathsf{U}$ are a coordinate basis for this coordinate system, so $\left[\mathsf{T}, \mathsf{U}\right] = 0$.

The geodesic deviation vector field is defined as:

$$\begin{equation}
\nabla_\mathsf{T}(\nabla_\mathsf{T} \mathsf{U}).
\end{equation}$$

If the connection has zero torsion, the geodesic deviation can be related to the Riemann curvature:

$$\begin{equation}
\nabla_\mathsf{T}(\nabla_\mathsf{T} \mathsf{U}) = -\mathcal{R}(\mathsf{U}, \mathsf{T})(\mathsf{T}),
\end{equation}$$

as follows, using equation (8.21),

$$\begin{equation}
\nabla_\mathsf{T}(\nabla_\mathsf{T} \mathsf{U}) = \nabla_\mathsf{T}(\nabla_\mathsf{U} \mathsf{T}),
\end{equation}$$

because both the torsion is zero and $[\mathsf{T}, \mathsf{U}] = 0$. Continuing

$$\begin{equation}
\begin{aligned}
\nabla_\mathsf{T}(\nabla_\mathsf{T} \mathsf{U}) &= \nabla_\mathsf{T}(\nabla_\mathsf{U} \mathsf{T}) \\
&= \nabla_\mathsf{T}(\nabla_\mathsf{U} \mathsf{T}) + \nabla_\mathsf{U}(\nabla_\mathsf{T} \mathsf{T}) - \nabla_\mathsf{U}(\nabla_\mathsf{T} \mathsf{T}) \\
&= \nabla_\mathsf{U}(\nabla_\mathsf{T} \mathsf{T}) - \mathcal{R}(\mathsf{U}, \mathsf{T})(\mathsf{T}) \\
&= -\mathcal{R}(\mathsf{U}, \mathsf{T})(\mathsf{T}).
\end{aligned}
\end{equation}$$

In the last line the first term was dropped because $\mathsf{T}$ satisfies the geodesic equation (8.24).

The geodesic deviation is defined without using a metric, but it helps to have a metric (see Chapter 9) to interpret the geodesic deviation. Consider two neighboring geodesics, with parameters $s$ and $s + \Delta s$. Given a metric we can assume that $t$ is proportional to path length along each geodesic, and we can define a distance $\delta(s, t, \Delta s)$ between the geodesics at the same value of the parameter $t$. So the velocity of separation of the two geodesics is

$$\begin{equation}
(\nabla_\mathsf{T} \mathsf{U}) = \partial_1 \delta(s, t, \Delta s)\hat{s}
\end{equation}$$

where $\hat{s}$ is a unit vector in the direction of increasing $s$. So $\nabla_\mathsf{T} U$ is the factor of increase of velocity with increase of separation. Similarly, the geodesic deviation can be interpreted as the factor of increase of acceleration with increase of separation:

$$\begin{equation}
\nabla{\mathsf{T}}(\nabla_\mathsf{T} \mathsf{U}) = \partial_1 \partial_1 \delta(s, t, \Delta s)\hat{s}.
\end{equation}$$

#### Longitude Lines on a Sphere

Consider longitude lines on the unit sphere.#Footnote(8) Let #Code(theta) be colatitude and #Code(phi) be longitude. These are the parameters $s$ and $t$, respectively. Then let #Code(T) be the vector field #Code(d#/dtheta) that is tangent to the longitude lines.

We can verify that every longitude line is a geodesic:

```Scheme
((omega (((covariant-derivative Cartan) T) T)) m)
;; 0
```

where #Code(omega) is an arbitrary one-form field.

Now let #Code(U) be #Code(d#/dphi), then #Code(U) commutes with #Code(T):

```Scheme
(((commutator U T) f) m)
;; 0
```

The torsion for the usual connection for the sphere is zero:

```Scheme
(let ((X (literal-vector-field 'X-sphere S2-spherical))
(Y (literal-vector-field 'Y-sphere S2-spherical)))
((((torsion-vector nabla) X Y) f) m))
;; 0
```

So we can compute the geodesic deviation using #Code(Riemann)

```Scheme
((+ (omega ((nabla T) ((nabla T) U)))
((Riemann nabla) omega T U T))
m)
;; 0
```

confirming equation (8.29).

Lines of longitude are geodesics. How do the lines of longitude behave? As we proceed from the North Pole, the lines of constant longitude diverge. At the Equator they are parallel and they converge towards the South Pole.

Let's compute $\nabla_\mathsf{T} \mathsf{U}$ and $\nabla_\mathsf{T}\left(\nabla_\mathsf{T} \mathsf{U}\right)$. We know that the distance is purely in the $\phi$ direction, so

```Scheme
((dphi ((nabla T) U)) m)
;; (/ (cos theta0) (sin theta0))
```

```Scheme
((dphi ((nabla T) ((nabla T) U))) m)
;; -1
```

Let's interpret these results. On a sphere of radius $R$ the distance at colatitude $\theta$ between two geodesics separated by $\Delta \phi$ is $d(\phi, \theta, \Delta \phi) = R \sin(\theta) \Delta \phi$. Assuming that $\theta$ is uniformly increasing with time, the magnitude of the velocity is just the $\theta$-derivative of this distance:

```Scheme
(define ((delta R) phi theta Delta-phi)
(* R (sin theta) Delta-phi))
```

```Scheme
(((partial 1) (delta 'R)) 'phi0 'theta0 'Delta-phi)
;; (* Delta-phi R (cos theta0))
```

The direction of the velocity is the unit vector in the $\phi$ direction:

```Scheme
(define phi-hat
(* (/ 1 (sin theta)) d/dphi))
```

This comes from the fact that the separation of lines of longitude is proportional to the sine of the colatitude. So the velocity vector field is the product.

We can measure the $\phi$ component with $d\phi$:

```Scheme
((dphi (* (((partial 1) (delta 'R))
'phi0 'theta0 'Delta-phi)
phi-hat))
m)
;; (/ (* Delta-phi R (cos theta0)) (sin theta0))
```

This agrees with $\nabla_\mathsf{T} \mathsf{U} \Delta \phi$ for the unit sphere. Indeed, the lines of longitude diverge until they reach the Equator and then they converge.

Similarly, the magnitude of the acceleration is

```Scheme
(((partial 1) ((partial 1) (delta 'R)))
'phi0 'theta0 'Delta-phi)
;; (* -1 Delta-phi R (sin theta0))
```

and the acceleration vector is the product of this result with $\hat{\phi}$. Measuring this with $d\phi$ we get:

```Scheme
((dphi (* (((partial 1) ((partial 1) (delta 'R)))
'phi0 'theta0 'Delta-phi)
phi-hat))
m)
;; (* -1 Delta-phi R)
```

And this agrees with the calculation of $\nabla_\mathsf{T} \nabla_\mathsf{T} \mathsf{U} \Delta \phi$ for the unit sphere. We see that the separation of the lines of longitude are uniformly decelerated as they progress from pole to pole.

### Bianchi Identities

There are some important mathematical properties of the Riemann curvature. These identities will be used to constrain the possible geometries that can occur.

A system with a symmetric connection, $\Gamma_{jk}^i = \Gamma_{jk}^i$, is torsion free.#Footnote(9)

```Scheme
(define nabla
(covariant-derivative
(Christoffel->Cartan
(symmetrize-Christoffel
(literal-Christoffel-2 'C R4-rect)))))
```

```Scheme
(((torsion nabla) omega X Y)
(typical-point R4-rect))
;; 0
```

The Bianchi identities are defined in terms of a cyclic-summation operator, which is most easily described as a Scheme procedure:

```Scheme
(define ((cyclic-sum f) x y z)
(+ (f x y z)
(f y z x)
(f z x y)))
```

The first Bianchi identity is

$$\begin{equation}
\mathsf{R}(\omega, \mathsf{x}, \mathsf{y}, \mathsf{z}) \
+ \mathsf{R}(\omega, \mathsf{y}, \mathsf{z}, \mathsf{x}) \
+ \mathsf{R}(\omega, \mathsf{z}, \mathsf{x}, \mathsf{y}) = 0,
\end{equation}$$

or, as a program:

```Scheme
(((cyclic-sum
(lambda (x y z)
((Riemann nabla) omega x y z)))
X Y Z)
(typical-point R4-rect))
;; 0
```

The second Bianchi identity is

$$\begin{equation}
\nabla_\mathsf{x} \mathsf{R}(\omega, \mathsf{v}, \mathsf{y}, \mathsf{z}) \
+ \nabla_\mathsf{y} \mathsf{R}(\omega, \mathsf{v}, \mathsf{z}, \mathsf{x}) \
+ \nabla_\mathsf{z} \mathsf{R}(\omega, \mathsf{v}, \mathsf{x}, \mathsf{y}) = 0
\end{equation}$$

or, as a program:

```Scheme
(((cyclic-sum
(lambda (x y z)
(((nabla x) (Riemann nabla))
omega V y z)))
X Y Z)
(typical-point R4-rect))
;; 0
```

Things get more complicated when there is torsion. We can make a general connection, which has torsion:

```Scheme
(define nabla
(covariant-derivative
(Christoffel->Cartan
(literal-Christoffel-2 'C R4-rect))))

(define R (Riemann nabla))
(define T (torsion-vector nabla))

(define (TT omega x y)
(omega (T x y)))
```

The first Bianchi identity is now:#Footnote(10)

```Scheme
(((cyclic-sum
(lambda (x y z)
(- (R omega x y z)
(+ (omega (T (T x y) z))
(((nabla x) TT) omega y z)))))
X Y Z)
(typical-point R4-rect))
;; 0
```

and the second Bianchi identity for a general connection is

```Scheme
(((cyclic-sum
(lambda (x y z)
(+ (((nabla x) R) omega V y z)
(R omega V (T x y) z))))
X Y Z)
(typical-point R4-rect))
;; 0
```

----
### Footnotes


#FootnoteRef(1) [11](references!bib_11), [4](references!bib_4), and [14](references!bib_14) use our definition. [20](references!bib_20) uses a different convention for the order of arguments and a different sign. See Appendix C for a definition of tensors.

#FootnoteRef(2) The connection specified by #Code(sphere-Cartan) is defined on page 107.

#FootnoteRef(3) The map $\gamma$ takes points on the real line to points on the target manifold. The chart $\chi$ gives coordinates of points on the target manifold while $\chi_\mathsf{R}$ gives a time coordinate on the real line.

#FootnoteRef(4) The series may not converge for large increments in the independent variable. In this case it is appropriate to numerically integrate the differential equations directly.

#FootnoteRef(5) The parallel-transport operators are evolution operators, and therefore descend into composition:

$$\begin{equation}
e_A(F \circ G) = F \circ \left(e^A G\right),
\end{equation}$$

for any state function $G$ and any compatible $F$. As a consequence, we have the following identity:

$$\begin{equation}
e^A e^B I = e^A \left(\left(e^B I\right) \circ I\right) = \
\left(e^B I\right) \circ \left(e^A I\right),
\end{equation}$$

where $I$ is the identity function on states.


#FootnoteRef(6) The setup for this experiment is a bit complicated. We need to make a manifold with a general connection.

```Scheme
(define Chi-inverse (point R2-rect))
(define Chi (chart R2-rect))
```

We now make the Cartan forms from the most general 2-dimensional Christoffel coefficient structure:

```Scheme
(define general-Cartan-2
(Christoffel->Cartan
(literal-Christoffel-2 'Gamma R2-rect)))
```

#FootnoteRef(7) People often say "Geodesic evolution is exponentiation of the covariant derivative." But this is wrong. The evolution is by exponentiation of $L_g$.

#FootnoteRef(8) The setup for this example is:

```Scheme
(define-coordinates (up theta phi) S2-spherical)
(define T d/dtheta)
(define U d/dphi)
(define m ((point S2-spherical) (up 'theta0 'phi0)))
(define Cartan (Christoffel->Cartan S2-Christoffel))
(define nabla (covariant-derivative Cartan))
```

#FootnoteRef(9) Setup for this section:

```Scheme
(define omega (literal-1form-field 'omega-rect R4-rect))
(define X (literal-vector-field 'X-rect R4-rect))
(define Y (literal-vector-field 'Y-rect R4-rect))
(define Z (literal-vector-field 'Z-rect R4-rect))
(define V (literal-vector-field 'V-rect R4-rect))
```

#FootnoteRef(10) The Bianchi identities are much nastier to write in traditional mathematical notation than as Scheme programs.