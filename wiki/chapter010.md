!!Polyglot
## Hodge Star and Electrodynamics
The vector space of p-form fields on an n-dimensional manifold has dimension n!/((n−p)!p!). This is the same dimension as the space of (n − p)-form fields. So these vector spaces are isomorphic. If we have a metric there is a natural isomorphism: for each p-form field ω on an n-dimensional manifold there is an (n − p)-form field g∗ω, called its Hodge dual.1 The Hodge dual should not be confused with the duality of vector bases and one-form bases, which is defined without reference to a metric. The Hodge dual is useful for the elegant formalization of electrodynamics.
In Euclidean 3-space, if we think of a one-form as a foliation of the space, then the dual is a two-form, which can be thought of as a pack of square tubes, whose axes are perpendicular to the leaves of the foliation. The original one-form divides these tubes up into volume elements. For example, the dual of the basis oneform dx is the two-form g∗dx = dy ∧ dz. We may think of dx as a set of planes perpendicular to the xˆ-axis. Then g∗dx is a set of tubes parallel to the xˆ-axis. In higher-dimensional spaces the visualization is more complicated, but the basic idea is the same. The Hodge dual of a two-form in four dimensions is a twoform that is perpendicular to the given two-form. However, if the metric is indefinite (e.g., the Lorentz metric) there is an added complication with the signs.
The Hodge dual is a linear operator, so it can be defined by its action on the basis elements. Let {∂/∂x0, . . . , ∂/∂xn−1} be an orthonormal basis of vector fields2 and let {dx0, . . . , dxn−1} be the ordinary dual basis for the one-forms. Then the (n − p)-form g∗ω that is the Hodge dual of the p-form ω can be defined by its coefficients with respect to the basis, using indices, as
(g∗ω)jp...jn−1 =
1ωi0...ip−1gi0j0 ...gip−1jp−1εj0...jn−1, (10.1) p!
 i0 ...ip−1 j0 ...jp−1
1The traditional notion is to just use an asterisk; we use g∗ to emphasize that
 this duality depends on the choice of metric g.
2We have a metric, so we can define “orthonormal” and use it to construct an orthonormal basis given any basis. The Gram-Schmidt procedure does the job.

#page(154)
 where gij are the coefficients of the inverse metric and εj0...jn−1 is either −1 or +1 if the permutation {0...n−1} → {j0 ...jn−1} is odd or even, respectively.
Relationship to Vector Calculus
In 3-dimensional Euclidean space the traditional vector derivative operations are gradient, curl, and divergence. If xˆ, yˆ, ˆz are the usual orthonormal rectangular vector basis, f a function on the space, and ⃗v a vector field on the space, then
g r a d ( f ) = ∂ f xˆ + ∂ f yˆ + ∂ f ˆz , ∂x ∂y ∂z
   ∂v ∂v  ∂v ∂v  ∂v ∂v  curl(⃗v)= z− y xˆ+ x− z yˆ+ y− x xˆ,
      ∂y ∂z ∂z ∂x ∂x ∂y
d i v ( ⃗v ) = ∂ v x + ∂ v y + ∂ v z . ∂x ∂y ∂z
Recall the meaning of the traditional vector operations. Traditionally we assume that there is a metric that allows us to determine distances between locations and angles between vectors. Such a metric establishes local scale factors relating coordinate increments to actual distances. The vector gradient, grad(f), points in the direction of steepest increase in the function with respect to actual distances. By contrast, the gradient one-form, df, does not depend on a metric, so there is no concept of distance built in to it. Nevertheless, the concepts are related. The gradient one-form is given by
∂ ∂ ∂
df= ∂xf dx+ ∂yf dy+ ∂zf dz. (10.2)
The traditional gradient vector field is then just the raised gradient one-form (see equation 9.8). So
grad(f) = g♯(df) (10.3) is computed by
(define (gradient metric basis) (compose (raise metric basis) d))
      
Chapter 10 Hodge Star and Electrodynamics
155
 Let θ be a one-form field: θ = θxdx + θydy + θzdz.
We compute
(10.4)
(10.5)
∂θ
dθ = z −
∂θ  y
∂θ ∂z
∂θ  z
∂x
    ∂y ∂z
dy ∧ dz +
x −
dz ∧ dx
∂θ ∂θ
+ y − x dx∧dy.
  ∂x ∂y
So the exterior-derivative expression corresponding to the vector-
calculus curl is:
∂θ ∂θ  ∂θ ∂θ  g∗(dθ)= z− y dx+ x− z dy
∂x
    ∂y ∂z ∂z
∂θ ∂θ
+ y− x dz.
(10.6)
(10.7)
  ∂x ∂y
Thus, the curl of a vector field v is
curl(v) = g♯(g∗(d(g♭(v)))), which can be computed with
(define (curl metric orthonormal-basis)
(let ((star (Hodge-star metric orthonormal-basis))
(sharp (raise metric orthonormal-basis))
(flat (lower metric))) (compose sharp star d flat)))
Also, we compute
∂θ ∂θ ∂θ 
d(g∗θ)= x + y + z dx∧dy∧dz.
(10.8)
   ∂x ∂y ∂z
So the exterior-derivative expression corresponding to the vector-
calculus div is
g∗d(g∗θ) = ∂θx + ∂θy + ∂θz . (10.9)
   ∂x ∂y ∂z
Thus, the divergence of a vector field v is
div(v) = g∗(d(g∗(g♭(v)))). (10.10)

#page(156)
 It is easily computed:
```Scheme
(define (divergence metric orthonormal-basis)
(let ((star (Hodge-star metric orthonormal-basis))
(flat (lower metric))) (compose star d star flat)))
```
The divergence is defined even if we don’t have a metric, but have only a connection. In that case the divergence can be computed with
```Scheme
(define (((divergence Cartan) v) point) (let ((basis (Cartan->basis Cartan))
(nabla (covariant-derivative Cartan))) (contract
(lambda (ei wi)
((wi ((nabla ei) v)) point))
basis)))
```
If the Cartan form is derived from a metric these programs yield the same answer.
The Laplacian is, as expected, the composition of the divergence and the gradient:
```Scheme
(define (Laplacian metric orthonormal-basis) (compose (divergence metric orthonormal-basis) (gradient metric orthonormal-basis)))
```
Spherical Coordinates
We can illustrate these by computing the formulas for the vectorcalculus operators in spherical coordinates. We start with a 3-dimensional manifold, and we set up the conditions for spherical coordinates.
```Scheme
(define spherical R3-rect)
(define-coordinates (up r theta phi) spherical)
(define R3-spherical-point
((point spherical) (up ’r0 ’theta0 ’phi0)))
```
The geometry is specified by the metric:

#page(157)
```Scheme
(define (spherical-metric v1 v2) (+ (* (dr v1) (dr v2))
(* (square r)
(+ (* (dtheta v1) (dtheta v2))
(* (expt (sin theta) 2) (dphi v1) (dphi v2))))))
```
We also need an orthonormal basis for the spherical coordinates. The coordinate basis is orthogonal but not normalized.
```Scheme
(define e 0 d/dr)
(define e 1 (* (/ 1 r) d/dtheta))
(define e 2 (* (/ 1 (* r (sin theta))) d/dphi))
(define orthonormal-spherical-vector-basis (down e0 e1 e2))
(define orthonormal-spherical-1form-basis (vector-basis->dual orthonormal-spherical-vector-basis
spherical))
(define orthonormal-spherical-basis
(make-basis orthonormal-spherical-vector-basis
orthonormal-spherical-1form-basis))
```
The components of the gradient of a scalar field are obtained using the dual basis:
```Scheme
((orthonormal-spherical-1form-basis
((gradient spherical-metric orthonormal-spherical-basis)
(literal-manifold-function ’f spherical))) R3-spherical-point)
(up (((partial 0) f) (up r0 theta0 phi0))
    (/ (((partial 1) f) (up r0 theta0 phi0))
       r0)
    (/ (((partial 2) f) (up r0 theta0 phi0))
       (* r0 (sin theta0))))
```
To get the formulas for curl and divergence we need a vector field with components with respect to the normalized basis.
```Scheme
(define v
(+ (* (literal-manifold-function ’v^0 spherical) e 0)
(* (literal-manifold-function ’v^1 spherical) e 1) (* (literal-manifold-function ’v^2 spherical) e 2)))
```         
#page(158)
 The curl is a bit complicated:
```Scheme
((orthonormal-spherical-1form-basis
((curl spherical-metric orthonormal-spherical-basis) v))
 R3-spherical-point)
(up
 (/ (+ (* (sin theta0)
(((partial 1) vˆ2) (up r0 theta0 phi0)))
(* (cos theta0) (vˆ2 (up r0 theta0 phi0)))
(* -1 (((partial 2) vˆ1) (up r0 theta0 phi0))))
    (* r0 (sin theta0)))
 (/ (+ (* -1 r0 (sin theta0)
(((partial 0) vˆ2) (up r0 theta0 phi0))) (* -1 (sin theta0) (vˆ2 (up r0 theta0 phi0))) (((partial 2) vˆ0) (up r0 theta0 phi0)))
(* r0 (sin theta0)))
(/ (+ (* r0 (((partial 0) vˆ1) (up r0 theta0 phi0)))
(vˆ1 (up r0 theta0 phi0))
(* -1 (((partial 1) vˆ0) (up r0 theta0 phi0)))) r0))
```
But the divergence and Laplacian are simpler
```Scheme
(((divergence spherical-metric orthonormal-spherical-basis) v) R3-spherical-point)
(+ (((partial 0) vˆ0) (up r0 theta0 phi0))
(/ (* 2 (vˆ0 (up r0 theta0 phi0))) r0)
(/ (((partial 1) vˆ1) (up r0 theta0 phi0)) r0) (/ (* (vˆ1 (up r0 theta0 phi0)) (cos theta0))
      (* r0 (sin theta0)))
   (/ (((partial 2) vˆ2) (up r0 theta0 phi0))
      (* r0 (sin theta0))))
(((Laplacian spherical-metric orthonormal-spherical-basis) (literal-manifold-function ’f spherical))
 R3-spherical-point)
(+ (((partial 0) ((partial 0) f)) (up r0 theta0 phi0))
   (/ (* 2 (((partial 0) f) (up r0 theta0 phi0)))
      r0)
   (/ (((partial 1) ((partial 1) f)) (up r0 theta0 phi0))
      (expt r0 2))
   (/ (* (cos theta0) (((partial 1) f) (up r0 theta0 phi0)))
      (* (expt r0 2) (sin theta0)))
   (/ (((partial 2) ((partial 2) f)) (up r0 theta0 phi0))
      (* (expt r0 2) (expt (sin theta0) 2))))
```
#page(159)
 10.1 The Wave Equation
The kinematics of special relativity can be formulated on a flat 4-dimensional spacetime manifold.
```Scheme
(define SR R4-rect)
(define-coordinates (up ct x y z) SR)
(define an-event ((point SR) (up ’ct0 ’x0 ’y0 ’z0)))
(define a-vector
(+ (* (literal-manifold-function ’v^t SR) d/dct)
(* (literal-manifold-function ’v^x SR) d/dx) (* (literal-manifold-function ’v^y SR) d/dy) (* (literal-manifold-function ’v^z SR) d/dz)))
```
The Minkowski metric is3 g(u, v) =
(10.11) −c2dt(u) dt(v) + dx(u) dx(v) + dy(u) dy(v) + dz(u) dz(v).
As a program:
```Scheme
(define (g-Minkowski u v) (+ (* -1 (dct u) (dct v))
     (* (dx u) (dx v))
     (* (dy u) (dy v))
     (* (dz u) (dz v))))
```
The length of a vector is described in terms of the metric:
σ = g(v, v).
(10.12)
If σ is positive the vector is spacelike and its square root is the
proper length of the vector. If σ is negative the vector is timelike and the square root of its negation is the proper time of the vector. If σ is zero the vector is lightlike or null.
```Scheme
((g-Minkowski a-vector a-vector) an-event)
(+ (* -1 (expt (vˆt (up ct0 x0 y0 z0)) 2)) (expt (vˆx (up ct0 x0 y0 z0)) 2)
(expt (vˆy (up ct0 x0 y0 z0)) 2)
(expt (vˆz (up ct0 x0 y0 z0)) 2))
```
3The metric in relativity is not positive definite, so nonzero vectors can have zero length.
 
#page(160)
 As an example of vector calculus in four dimensions, we can compute the wave equation for a scalar field in 4-dimensional spacetime.
We need an orthonormal basis for the spacetime:
```Scheme
(define SR-vector-basis (coordinate-system->vector-basis SR))
```
We check that it is orthonormal with respect to the metric:
```Scheme
((g-Minkowski SR-vector-basis SR-vector-basis) an-event)
(down (down -1 0 0 0) (down 0100) (down 0010)
(down 0001))
So, the Laplacian of a scalar field is the wave equation!
(define p (literal-manifold-function ’phi SR))
(((Laplacian g-Minkowski SR-basis) p) an-event)
(+ (((partial 0) ((partial 0) phi)) (up ct0 x0 y0 z0))
   (* -1 (((partial 1) ((partial 1) phi)) (up ct0 x0 y0 z0)))
   (* -1 (((partial 2) ((partial 2) phi)) (up ct0 x0 y0 z0)))
   (* -1 (((partial 3) ((partial 3) phi)) (up ct0 x0 y0 z0))))
```
10.2 Electrodynamics
Using Hodge duals we can represent electrodynamics in an elegant way. Maxwell’s electrodynamics is invariant under Lorentz transformations. We use 4-dimensional rectangular coordinates for the flat spacetime of special relativity.
In this formulation of electrodynamics the electric and magnetic fields are represented together as a two-form field, the Faraday tensor. Under Lorentz transformations the individual components are mixed. The Faraday tensor is:4
```Scheme
(define (Faraday Ex Ey Ez Bx By Bz) (+ (* Ex (wedge dx dct))
(* Ey (wedge dy dct)) (* Ez (wedge dz dct)) (* Bx (wedge dy dz)) (* By (wedge dz dx)) (* Bz (wedge dx dy))))
```
4This representation is from Misner, Thorne, and Wheeler, Gravitation, p.108.

#page(161)
 The Hodge dual of the Faraday tensor exchanges the electric and magnetic fields, negating the components that will involve time. The result is called the Maxwell tensor:
```Scheme
(define (Maxwell Ex Ey Ez Bx By Bz) (+ (* -1 Bx (wedge dx dct))
(* -1 By (wedge dy dct))
(* -1 Bz (wedge dz dct))
(* Ex (wedge dy dz)) (* Ey (wedge dz dx)) (* Ez (wedge dx dy))))
```
We make a Hodge dual operator for this situation:
```Scheme
(define SR-star (Hodge-star g-Minkowski SR-basis))
```
And indeed, it transforms the Faraday tensor into the Maxwell tensor:
```Scheme
(((- (SR-star (Faraday ’Ex ’Ey ’Ez ’Bx ’By ’Bz)) (Maxwell ’Ex ’Ey ’Ez ’Bx ’By ’Bz))
(literal-vector-field ’u SR)
(literal-vector-field ’v SR)) an-event)
0
```
One way to get electric fields is to have charges; magnetic fields can arise from motion of charges. In this formulation we combine the charge density and the current to make a one-form field:
```Scheme
(define (J charge-density Ix Iy Iz)
(- (* (/ 1 :c) (+ (* Ix dx) (* Iy dy) (* Iz dz)))
(* charge-density dct)))
```
The coefficient (/ 1 :c) makes the components of the one-form uniform with respect to units.
To develop Maxwell’s equations we need a general Faraday field and a general current-density field:
```Scheme
(define F
  (Faraday
(literal-manifold-function ’Ex SR) (literal-manifold-function ’Ey SR) (literal-manifold-function ’Ez SR) (literal-manifold-function ’Bx SR) (literal-manifold-function ’By SR) (literal-manifold-function ’Bz SR)))
(define 4-current
(J (literal-manifold-function ’rho SR)
(literal-manifold-function ’Ix SR) (literal-manifold-function ’Iy SR) (literal-manifold-function ’Iz SR)))
```
#page(162)
Maxwell’s Equations
Maxwell’s equations in the language of differential forms are
dF = 0, (10.13)
d(g∗ F) = 4π g∗ J. (10.14) The first equation gives us what would be written in vector nota-
tion as
divB⃗ = 0, (10.15)
⃗ 1dB⃗
curlE = − c dt . (10.16)
The second equation gives us what would be written in vector notation as
  divE⃗ = 4πρ, (10.17) ⃗ 1 d E⃗ 4 π ⃗
curlB = c dt + c I. (10.18)
To see how these work out, we evaluate each component of dF and d (g∗F) − 4π g∗ J. Since these are both two-form fields, their exterior derivatives are three-form fields, so we have to provide three basis vectors to get each component. Each component equation will yield one of Maxwell’s equations, written in coordinates, without vector notation. So, the purely spatial component dF(∂/∂x,∂/∂y,∂/∂z) of equation 10.13 is equation 10.15:
```Scheme
(((d F) d/dx d/dy d/dz) an-event)
(+ (((partial 1) Bx) (up ct0 x0 y0 z0))
   (((partial 2) By) (up ct0 x0 y0 z0))
   (((partial 3) Bz) (up ct0 x0 y0 z0)))
```
∂Bx + ∂By + ∂Bz = 0 ∂x ∂y ∂z
(10.19)
   
#page(163)
 The three mixed space and time components of equation 10.13 are equation 10.16:
```Scheme
(((d F) d/dct d/dy d/dz) an-event)
(+ (((partial 0) Bx) (up ct0 x0 y0 z0))
   (((partial 2) Ez) (up ct0 x0 y0 z0))
   (* -1 (((partial 3) Ey) (up ct0 x0 y0 z0))))
∂Ez − ∂Ey = 1 ∂Bx , ∂y ∂z c ∂t
(((d F) d/dct d/dz d/dx) an-event)
(+ (((partial 0) By) (up ct0 x0 y0 z0))
   (((partial 3) Ex) (up ct0 x0 y0 z0))
   (* -1 (((partial 1) Ez) (up ct0 x0 y0 z0))))
∂Ex − ∂Ez = 1 ∂By , ∂z ∂x c∂t
(((d F) d/dct d/dx d/dy) an-event)
(+ (((partial 0) Bz) (up ct0 x0 y0 z0))
   (((partial 1) Ey) (up ct0 x0 y0 z0))
   (* -1 (((partial 2) Ex) (up ct0 x0 y0 z0))))
(((- (d (SR-star F)) (* 4 :pi (SR-star 4-current))) d/dx d/dy d/dz)
an-event)
(+ (* -4 :pi (rho (up ct0 x0 y0 z0)))
   (((partial 1) Ex) (up ct0 x0 y0 z0))
   (((partial 2) Ey) (up ct0 x0 y0 z0))
   (((partial 3) Ez) (up ct0 x0 y0 z0)))
```
∂Ex + ∂Ey + ∂Ez = 4πρ. ∂x ∂y ∂z
(10.20)
(10.21)
(10.22) The purely spatial component of equation 10.14 is equation 10.17:
        ∂Ey − ∂Ex = 1 ∂Bz . ∂x ∂y c ∂t
    (10.23)
   
#page(164)
 And finally, the three mixed time and space components of equation 10.14 are equation 10.18:
```Scheme
(((- (d (SR-star F)) (* 4 :pi (SR-star 4-current))) d/dct d/dy d/dz)
an-event)
(+ (((partial 0) Ex) (up ct0 x0 y0 z0))
   (* -1 (((partial 2) Bz) (up ct0 x0 y0 z0)))
   (((partial 3) By) (up ct0 x0 y0 z0))
   (/ (* 4 :pi (Ix (up ct0 x0 y0 z0))) :c))
∂By −∂Bz =−1∂Ex −4πIx, ∂z ∂y c∂t c
(((- (d (SR-star F)) (* 4 :pi (SR-star 4-current))) d/dct d/dz d/dx)
an-event)
(+ (((partial 0) Ey) (up ct0 x0 y0 z0))
   (* -1 (((partial 3) Bx) (up ct0 x0 y0 z0)))
   (((partial 1) Bz) (up ct0 x0 y0 z0))
   (/ (* 4 :pi (Iy (up ct0 x0 y0 z0))) :c))
∂Bz −∂Bx =−1∂Ey −4πIy, ∂x ∂z c∂t c
(((- (d (SR-star F)) (* 4 :pi (SR-star 4-current))) d/dct d/dx d/dy)
an-event)
(+ (((partial 0) Ez) (up ct0 x0 y0 z0))
   (* -1 (((partial 1) By) (up ct0 x0 y0 z0)))
   (((partial 2) Bx) (up ct0 x0 y0 z0))
   (/ (* 4 :pi (Iz (up ct0 x0 y0 z0))) :c))
```
∂Bx −∂By =−1∂Ez −4πIz. ∂y ∂x c∂t c
Lorentz Force
(10.24)
     (10.25)
     (10.26)
The classical force on a charged particle moving in a electromagnetic field is
⃗⃗1⃗
f=q E+c⃗v×B . (10.27)
We can compute this in coordinates. We construct arbitrary E⃗ and B⃗ vector fields and an arbitrary velocity:
 
#page(165)
```Scheme
(define E
(up (literal-manifold-function ’Ex SR)
(literal-manifold-function ’Ey SR) (literal-manifold-function ’Ez SR)))
(define B
(up (literal-manifold-function ’Bx SR)
(literal-manifold-function ’By SR) (literal-manifold-function ’Bz SR)))
(define V (up ’Vx ’Vy ’Vz))
```
The 3-space force that results is a mess:
```Scheme
(* ’q (+ (E an-event) (cross-product V (B an-event))))
(up (+ (* q (Ex (up ct0 x0 y0 z0)))
(* q V y (Bz (up ct0 x0 y0 z0)))
(* -1 q V z (By (up ct0 x0 y0 z0))))
(+ (* q (Ey (up ct0 x0 y0 z0)))
(* -1 q V x (Bz (up ct0 x0 y0 z0))) (* q V z (Bx (up ct0 x0 y0 z0))))
(+ (* q (Ez (up ct0 x0 y0 z0)))
(* q V x (By (up ct0 x0 y0 z0)))
(* -1 q V y (Bx (up ct0 x0 y0 z0)))))
```
The relativistic Lorentz 4-force is usually written in coordinates as
         fν = −
where U is the 4-velocity of the charged particle, F is the Faraday tensor, and ηαν are the components of the inverse of the Minkowski metric. Here is a program that computes a component of the force in terms of the Faraday tensor. The desired component is specified by a one-form.
```Scheme
(define (Force charge F 4velocity component) (* -1 charge
(contract (lambda (a b)
(contract (lambda (e w)
```
α,μ
qUμFμαηαν, (10.28)
```Scheme
SOMETHING MISSING HERE
SR-basis)))
  (* (w 4velocity)
     (F e a)
(eta-inverse b component))) SR-basis))
```
#page(166)
 So, for example, the force in the xˆ direction for a stationary particle is
```Scheme
((Force ’q F d/dct dx) an-event)
(* q (Ex (up ct0 x0 y0 z0)))
```
Notice that the 4-velocity ∂/∂ct is the 4-velocity of a stationary particle!
If we give a particle a more general timelike 4-velocity in the xˆ direction we can see how the yˆ component of the force involves both the electric and magnetic field:
```Scheme
(define (Ux beta)
(+ (* (/ 1 (sqrt (- 1 (square beta)))) d/dct)
(* (/ beta (sqrt (- 1 (square beta)))) d/dx)))
((Force ’q F (Ux ’v/c) dy) an-event)
(/ (+ (* -1 q v/c (Bz (up ct0 x0 y0 z0)))
      (* q (Ey (up ct0 x0 y0 z0))))
   (sqrt (+ 1 (* -1 (expt v/c 2)))))
```
Exercise 10.1: Relativistic Lorentz Force
Compute all components of the 4-force for a general timelike 4-velocity.
a. Compare these components to the components of the nonrelativistic force given above. Interpret the differences.
b. What is the meaning of the time component? For example, consider:
```Scheme
((Force ’q F (Ux ’v/c) dct) an-event)
(/ (* q v/c (Ex (up ct0 x0 y0 z0))) (sqrt (+ 1 (* -1 (expt v/c 2)))))
```
c. Subtract the structure of components of the relativistic 3-space force from the structure of the spatial components of the 4-space force to show that they are equal.

