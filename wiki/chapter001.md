!!Polyglot
## Introduction
#Quote(Philosophy is written in that great book which ever lies before our eyes—I mean the Universe—but we cannot understand it if we do not learn the language and grasp the symbols in which it is written. This book is written in the mathematical language, and the symbols are triangles, circles, and other geometrical figures without whose help it is impossible to comprehend a single word of it, without which one wanders in vain through a dark labyrinth.)
#Caption Galileo Galilei [8](references!bib_8)

Differential geometry is a mathematical language that can be used to express physical concepts. In this introduction we show a typical use of this language. Do not panic! At this point we do not expect you to understand the details of what we are showing. All will be explained as needed in the text. The purpose is to get the flavor of this material.

At the North Pole inscribe a line in the ice perpendicular to the Greenwich Meridian. Hold a stick parallel to that line and walk down the Greenwich Meridian keeping the stick parallel to itself as you walk. (The phrase “parallel to itself” is a way of saying that as you walk you keep its orientation unchanged. The stick will be aligned East-West, perpendicular to your direction of travel.) When you get to the Equator the stick will be parallel to the Equator. Turn East, and walk along the Equator, keeping the stick parallel to the Equator. Continue walking until you get to the 90◦E meridian. When you reach the 90◦E meridian turn North and walk back to the North Pole keeping the stick parallel to itself. Note that the stick is perpendicular to your direction of travel. When you get to the Pole note that the stick is perpendicular to the line you inscribed in the ice. But you started with that stick parallel to that line and you kept the stick pointing in the same direction on the Earth throughout your walk — how did it change orientation?
#page(2)
The answer is that you walked a closed loop on a curved surface. As seen in three dimensions the stick was actually turning as you walked along the Equator, because you always kept the stick parallel to the curving surface of the Earth. But as a denizen of a 2-dimensional surface, it seemed to you that you kept the stick parallel to itself as you walked, even when making a turn. Even if you had no idea that the surface of the Earth was embedded in a 3-dimensional space you could use this experiment to conclude that the Earth was not flat. This is a small example of intrinsic geometry. It shows that the idea of parallel transport is not simple. For a general surface it is necessary to explicitly define what we mean by parallel.

If you walked a smaller loop, the angle between the starting orientation and the ending orientation of the stick would be smaller. For small loops it would be proportional to the area of the loop you walked. This constant of proportionality is a measure of the curvature. The result does not depend on how fast you walked, so this is not a dynamical phenomenon.

Denizens of the surface may play ball games. The balls are constrained to the surface; otherwise they are free particles. The paths of the balls are governed by dynamical laws. This motion is a solution of the Euler-Lagrange equations#Footnote(1) for the free-particle Lagrangian with coordinates that incorporate the constraint of living in the surface. There are coefficients of terms in the EulerLagrange equations that arise naturally in the description of the behavior of the stick when walking loops on the surface, connecting the static shape of the surface with the dynamical behavior of the balls. It turns out that the dynamical evolution of the balls may be viewed as parallel transport of the ball’s velocity vector in the direction of the velocity vector. This motion by parallel transport of the velocity is called geodesic motion.

So there are deep connections between the dynamics of particles and the geometry of the space that the particles move in. If we understand this connection we can learn about dynamics by studying geometry and we can learn about geometry by studying dynamics. We enter dynamics with a Lagrangian and the associated Lagrange equations. Although this formulation exposes many important features of the system, such as how symmetries relate to
#page(3)
conserved quantities, the geometry is not apparent. But when we express the Lagrangian and the Lagrange equations in differential geometry language, geometric properties become apparent. In the case of systems with no potential energy the Euler-Lagrange equations are equivalent to the geodesic equations on the configuration manifold. In fact, the coefficients of terms in the Lagrange equations are Christoffel coefficients, which define parallel transport on the manifold. Let’s look into this a bit.

Lagrange Equations

We write the Lagrange equations in functional notation#Footnote(2) as follows:
D(∂2L◦Γ[q])−∂1L◦Γ[q] = 0.
In SICM [19](references!bib_19), Section 1.6.3, we showed that a Lagrangian describing the free motion of a particle subject to a coordinate dependent constraint can be obtained by composing a free-particle Lagrangian with a function that describes how dynamical states transform given the coordinate transformation that describes the constraints.

A Lagrangian for a free particle of mass m and velocity v is just its kinetic energy, $mv^2/2$. The procedure Lfree implements the free Lagrangian:#Footnote(3)
```Scheme
(define ((Lfree mass) state)
(* 1/2 mass (square (velocity state))))
```
For us the dynamical state of a system of particles is a tuple of time, coordinates, and velocities. The free-particle Lagrangian depends only on the velocity part of the state.
For motion of a point constrained to move on the surface of a sphere the configuration space has two dimensions. We can describe the position of the point with the generalized coordinates colatitude and longitude. If the sphere is embedded in 3-dimensional space the position of the point in that space can be
#page(4)
given by a coordinate transformation from colatitude and longitude to three rectangular coordinates.
For a sphere of radius R the procedure sphere->R3 implements the transformation of coordinates from colatitude θ and longitude φ on the surface of the sphere to rectangular coordinates in the embedding space. (The zˆ axis goes through the North Pole, and the Equator is in the plane z = 0.)
```Scheme
(define ((sphere->R3 R) state) (let ((q (coordinate state)))
(let ((theta (ref q 0)) (phi (ref q 1)))
(up (* R (sin theta) (cos phi)) (* R (sin theta) (sin phi)) (* R (cos theta))))))
; x ; y ; z
```
The coordinate transformation maps the generalized coordinates on the sphere to the 3-dimensional rectangular coordinates. Given this coordinate transformation we construct a corresponding transformation of velocities; these make up the state transformation. The procedure F->C implements the derivation of a transformation of states from a coordinate transformation:
```Scheme
(define ((F->C F) state) (up (time state)
(F state)
(+ (((partial 0) F) state)
(* (((partial 1) F) state) (velocity state)))))
```
A Lagrangian governing free motion on a sphere of radius R is then the composition of the free Lagrangian with the transformation of states.
```Scheme
(define (Lsphere m R)
(compose (Lfree m) (F->C (sphere->R3 R))))
```
So the value of the Lagrangian at an arbitrary dynamical state is:
```Scheme
((Lsphere ’m ’R)
(up ’t (up ’theta ’phi) (up ’thetadot ’phidot)))
(+ (* 1/2 m (expt R 2) (expt thetadot 2))
   (* 1/2 m (expt R 2) (expt (sin theta) 2) (expt phidot 2)))
```
#page(5)
 or, in infix notation:
$$1mR2θ ̇2 + 1mR2 (sin (θ))2 φ ̇2. (1.1)$$

22 The Metric
Let’s now take a step into the geometry. A surface has a metric which tells us how to measure sizes and angles at every point on the surface. (Metrics are introduced in Chapter 9.)
The metric is a symmetric function of two vector fields that gives a number for every point on the manifold. (Vector fields are introduced in Chapter 3). Metrics may be used to compute the length of a vector field at each point, or alternatively to compute the inner product of two vector fields at each point. For example, the metric for the sphere of radius R is
g(u, v) = R2dθ(u)dθ(v) + R2(sin θ)2dφ(u)dφ(v), (1.2)
where u and v are vector fields, and dθ and dφ are one-form fields that extract the named components of the vector-field argument. (One-form fields are introduced in Chapter 3.) We can think of dθ(u) as a function of a point that gives the size of the vector field u in the θ direction at the point. Notice that g(u, u) is a weighted sum of the squares of the components of u. In fact, if we identify
dθ(v) = θ ̇ dφ(v) = φ ̇,
then the coefficients in the metric are the same as the coefficients in the value of the Lagrangian, equation (1.1), apart from a factor of m/2.
We can generalize this result and write a Lagrangian for free motion of a particle of mass m on a manifold with metric g:
  1mgij(x)vivj. (1.3) 2
ij
This is written using indexed variables to indicate components of the geometric objects expressed with respect to an unspecified coordinate system. The metric coefficients gij are, in general, a
L2(x,v) =
#page(6)
function of the position coordinates x, because the properties of the space may vary from place to place.
We can capture this geometric statement as a program:
```Scheme
(define ((L2 mass metric) place velocity)
(* 1/2 mass ((metric velocity velocity) place)))
```
This program gives the Lagrangian in a coordinate-independent, geometric way. It is entirely in terms of geometric objects, such as a place on the configuration manifold, the velocity at that place, and the metric that describes the local shape of the manifold. But to compute we need a coordinate system. We express the dynamical state in terms of coordinates and velocity components in the coordinate system. For each coordinate system there is a natural vector basis and the geometric velocity vectors can be constructed by contracting the basis with the components of the velocity. Thus, we can form a coordinate representation of the Lagrangian.
```Scheme
(define ((Lc mass metric coordsys) state)
(let ((x (coordinates state)) (v (velocities state))
(e (coordinate-system->vector-basis coordsys))) ((L2 mass metric) ((point coordsys) x) (* e v))))
```
The manifold point m represented by the coordinates x is given by (define m ((point coordsys) x)). The coordinates of m in a different coordinate system are given by ((chart coordsys2) m). The manifold point m is a geometric object that is the same point independent of how it is specified. Similarly, the velocity vector ev is a geometric object, even though it is specified using components v with respect to the basis e. Both v and e have as many components as the dimension of the space so their product is interpreted as a contraction.
Let’s make a general metric on a 2-dimensional real manifold:#Footnote(4)
```Scheme
(define the-metric (literal-metric ’g R2-rect))
```
#page(7)
The metric is expressed in rectangular coordinates, so the coordinatesystemisR2-rect.5 Thecomponentfunctionswillbelabeled as subscripted gs.
We can now make the Lagrangian for the system:
```Scheme
(define L (Lc ’m the-metric R2-rect))
```
And we can apply our Lagrangian to an arbitrary state:
```Scheme
(L (up ’t (up ’x ’y) (up ’vx ’vy)))
(+ (* 1/2 m (g 00 (up x y)) (expt vx 2)) (* m (g 01 (up x y)) vx vy)
(* 1/2 m (g11 (up x y)) (expt vy 2)))
```
Compare this result with equation (1.3).

Euler-Lagrange Residuals

The Euler-Lagrange equations are satisfied on realizable paths. Let γ be a path on the manifold of configurations. (A path is a map from the 1-dimensional real line to the configuration manifold. We introduce maps between manifolds in Chapter 6.) Consider an arbitrary path:#Footnote(6)
```Scheme
(define gamma (literal-manifold-map ’q R1-rect R2-rect))
```
The values of γ are points on the manifold, not a coordinate representation of the points. We may evaluate gamma only on points of the real-line manifold; gamma produces points on the R2 manifold. So to go from the literal real-number coordinate ’t to a point on the real line we use ((point R1-rect) ’t) and to go from a point m in R2 to its coordinate representation we use ((chart R2-rect) m). (The procedures point and chart are introduced in Chapter 2.) Thus

#page(8)
Introduction
```Scheme
 ((chart R2-rect) (gamma ((point R1-rect) ’t)))
(up (qˆ0 t) (qˆ1 t))
```
So, to work with coordinates we write:
```Scheme
(define coordinate-path
(compose (chart R2-rect) gamma (point R1-rect)))
(coordinate-path ’t)
(up (qˆ0 t) (qˆ1 t))
```
Now we can compute the residuals of the Euler-Lagrange equations, but we get a large messy expression that we will not show.7 However, we will save it to compare with the residuals of the geodesic equations.
```Scheme
(define Lagrange-residuals
(((Lagrange-equations L) coordinate-path) ’t))
```
Geodesic Equations
Now we get deeper into the geometry. The traditional way to write the geodesic equations is
∇vv = 0 (1.4)
where ∇ is a covariant derivative operator. Roughly, ∇vw is a directional derivative. It gives a measure of the variation of the vector field w as you walk along the manifold in the direction of v. (We will explain this in depth in Chapter 7.) ∇vv = 0 is intended to convey that the velocity vector is parallel-transported by itself. When you walked East on the Equator you had to hold the stick so that it was parallel to the Equator. But the stick is constrained to the surface of the Earth, so moving it along the Equator required turning it in three dimensions. The ∇ thus must incorporate the 3-dimensional shape of the Earth to provide a notion of “parallel” appropriate for the denizens of the surface of the Earth. This information will appear as the “Christoffel coefficients” in the coordinate representation of the geodesic equations.
The trouble with the traditional way to write the geodesic equations (1.4) is that the arguments to the covariant derivative are
#page(9)
 vector fields and the velocity along the path is not a vector field. A more precise way of stating this relation is:
$$\nabla γ∂/∂tdγ(∂/∂t) = 0. (1.5)$$
(We know that this may be unfamiliar notation, but we will explain it in Chapter 7.)
In coordinates, the geodesic equations are expressed
$$D2qi(t) + \Sigma jk Γijk(γ(t))Dqj(t)Dqk(t) = 0, (1.6)$$

where q(t) is the coordinate path corresponding to the manifold path γ, and Γijk(m) are Christoffel coefficients. The Γijk(m) describe the “shape” of the manifold close to the manifold point m. They can be derived from the metric g.
We can get and save the geodesic equation residuals by:
```Scheme
(define geodesic-equation-residuals (((((covariant-derivative Cartan gamma) d/dt)
((differential gamma) d/dt)) (chart R2-rect))
((point R1-rect) ’t)))
```
where $\frac{d}{dt}$ is a vector field on the real line#Footnote(8) and Cartan is a way of encapsulating the geometry, as specified by the Christoffel coefficients. The Christoffel coefficients are computed from the metric:
```Scheme
(define Cartan
  (Christoffel->Cartan
(metric->Christoffel-2 the-metric (coordinate-system->basis R2-rect))))
```
The two messy residual results that we did not show are related by the metric. If we change the representation of the geodesic equations by “lowering” them using the mass and the metric, we see that the residuals are equal:
#page(10)
```Scheme
(define metric-components (metric->components the-metric
(coordinate-system->basis R2-rect)))
(- Lagrange-residuals
(* (* ’m (metric-components (gamma ((point R1-rect) ’t))))
geodesic-equation-residuals))
(down 0 0)
```
This establishes that for a 2-dimensional space the Euler-Lagrange equations are equivalent to the geodesic equations. The Christoffel coefficients that appear in the geodesic equation correspond to coefficients of terms in the Euler-Lagrange equations. This analysis will work for any number of dimensions (but will take your computer longer in higher dimensions, because the complexity increases).

Exercise 1.1: Motion on a Sphere
The metric for a unit sphere, expressed in colatitude θ and longitude φ, is
g(u, v) = dθ(u)dθ(v) + (sin θ)2dφ(u)dφ(v).
Compute the Lagrange equations for motion of a free particle on the sphere and convince yourself that they describe great circles. For example, consider motion on the equator (θ = π/2) and motion on a line of longitude (φ is constant).

----
#FootnoteRef(1) It is customary to shorten “Euler-Lagrange equations” to “Lagrange equations.” We hope Leonhard Euler is not disturbed.
#FootnoteEnd
#FootnoteRef(2) A short introduction to our functional notation, and why we have chosen it, is given in the prologue: Programming and Understanding. More details can be found in Appendix B.
#FootnoteEnd
#FootnoteRef(3) An informal description of the Scheme programming language can be found in Appendix A.
#FootnoteEnd
#FootnoteRef(4) The procedure literal-metric provides a metric. It is a general symmetric function of two vector fields, with literal functions of the coordinates of the manifold points for its coefficients in the given coordinate system. The quoted symbol ’g is used to make the names of the literal coefficient functions. Literal functions are discussed in Appendix B.
#FootnoteEnd
#FootnoteRef(5) R2-rect is the usual rectangular coordinate system on the 2-dimensional real manifold. (See Section 2.1, page 13.) We supply common coordinate systems for n-dimensional real manifolds. For example, R2-polar is a polar coordinate system on the same manifold.
#FootnoteEnd
#FootnoteRef(6) The procedure literal-manifold-map makes a map from the manifold implied by its second argument to the manifold implied by the third argument. These arguments must be coordinate systems. The quoted symbol that is the first argument is used to name the literal coordinate functions that define the map.
#FootnoteEnd
#FootnoteRef(7) For an explanation of equation residuals see page xvi.
#FootnoteEnd
#FootnoteRef(8) We established t as a coordinate function on the rectangular coordinates of the real line by
```Scheme
(define-coordinates t R1-rect)
```
This had the effect of also defining $\frac{d}{dt}$ as a coordinate vector field and $dt$ as a one-form field on the real line.
#FootnoteEnd