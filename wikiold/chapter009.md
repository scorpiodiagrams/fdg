!!Polyglot
## Metrics
We often want to impose further structure on a manifold to allow us to define lengths and angles. This is done by generalizing the idea of the Euclidean dot product, which allows us to compute lengths of vectors and angles between vectors in traditional vector algebra.

For vectors ⃗u = u x xˆ + u y yˆ + u z zˆ and ⃗v = v x xˆ + v y yˆ + v z zˆ the dot product is ⃗u · ⃗v = uxvx + uyvy + uzvz. The generalization is to provide coefficients for these terms and to include cross terms, consistent with the requirement that the function of two vectors is symmetric. This symmetric, bilinear, real-valued function of two vector fields is called a metric field.
For example, the natural metric on a sphere of radius R is

$$g(u, v) = R2(dθ(u)dθ(v) + (sin θ)2dφ(u)dφ(v)), (9.1)$$

and the Minkowski metric on the 4-dimensional space of special relativity is
g(u,v) = dx(u)dx(v)+dy(u)dy(v)+dz(u)dz(v)−c2dt(u)dt(v).(9.2)
Although these examples are expressed in terms of a coordinate basis, the value of the metric on vector fields does not depend on the coordinate system that is used to specify the metric.
Given a metric field g and a vector field v the scalar field g(v, v) is the squared length of the vector at each point of the manifold.
Metric Music
The metric can be used to construct a one-form field ωu from a vector field u, such that for any vector field v we have
ωu(v) = g(v, u). (9.3)
The operation of constructing a one-form field from a vector field using a metric is called “lowering” the vector field. It is sometimes notated as
ωu = g♭(u). (9.4)

#page(134)
 There is also an inverse metric that takes two one-form fields. It is defined by the relation
g−1( ̃ei, ̃ej)g(ej,ek), (9.5)
where e and  ̃e are any basis and its dual basis.
The inverse metric can be used to construct a vector field vω
from a one-form field ω, such that for any one-form field τ we have
τ(vω) = g−1(ω,τ). (9.6)
This definition is implicit, but the vector field can be explicitly computed from the one-form field with respect to a basis as follows:
δki =
j
g−1(ω, ̃ei)ei. (9.7)
The operation of constructing a vector field from a one-form field using a metric is called “raising” the one-form field. It is sometimes notated
vω = g♯(ω). (9.8)
The raising and lowering operations allow one to interchange the vector fields and the one-form fields. However they should not be confused with the dual operation that allows one to construct a dual one-form basis from a vector basis or construct a vector basis from a one-form basis. The dual operation that interchanges bases is defined without assigning a metric structure on the space.
Lowering a vector field with respect to a metric is a simple program:
```Scheme
(define ((lower metric) u) (define (omega v) (metric v u)) (procedure->1form-field omega))
```
But raising a one-form field to make a vector field is a bit more complicated:
vω =
i

#page(135)
```Scheme
(define (raise metric basis)
(let ((gi (metric:invert metric basis)))
(lambda (omega)
(contract (lambda (e i w^i)
(* (gi omega w^i) e i)) basis))))
```
where contract is the trace over a basis of a two-argument function that takes a vector field and a one-form field as its arguments.1
```Scheme
(define (contract proc basis)
(let ((vector-basis (basis->vector-basis basis))
(1form-basis (basis->1form-basis basis))) (s:sigma/r proc
               vector-basis
               1form-basis)))
```
9.1 Metric Compatibility
A connection is said to be compatible with a metric g if the covariant derivative for that connection obeys the “product rule”:
∇X(g(Y, Z)) = g(∇X(Y), Z) + g(Y, ∇X(Z)). (9.9)
For a metric there is a unique torsion-free connection that is compatible with it. The Christoffel coefficients of the first kind are computed from the metric by the following:
Γ ̄ijk = 1(ek(g(ei,ej)) + ej(g(ei,ek)) − ei(g(ej,ek))), (9.10) 2
for the coordinate basis e. We can then construct the Christoffel coefficients of the second kind (the ones used previously to define a connection) by “raising the first index.” To do this we define a function of three vectors, with a weird currying:
   Γ ̃(v,w)(u) =
1Notice that raise and lower are not symmetrical. This is because vector fields and form fields are not symmetrical: a vector field takes a manifold function as its argument, whereas a form field takes a vector field as its argument. This asymmetry is not apparent in traditional treatments based on index notation.
ijk
Γ ̄ijk ̃ei(u) ̃ej(v) ̃ek(w). (9.11)
 
#page(136)
 This function takes two vector fields and produces a one-form field. We can use it with equation (9.7) to construct a new function that takes two vector fields and produces a vector field:
Γˆ(v,w) =
We can now construct the Christoffel coefficients of the second
i
kind:
Γijk =  ̃ei(Γˆ(ej,ek)) =
g−1(Γ ̃(v,w), ̃ei)ei. (9.12)
Γ ̄mjkg−1( ̃em, ̃ei). The Cartan forms are then just
(9.13)
(9.14)
πij = Γijk ̃ek =  ̃ei(Γˆ(ej,ek)) ̃ek. kk
m
So, for example, we can compute the Christoffel coefficients for the sphere from the metric for the sphere. First, we need the metric:
```Scheme
(define ((g-sphere R) u v) (* (square R)
(+ (* (dtheta u) (dtheta v))
(* (compose (square sin) theta)
           (dphi u)
           (dphi v)))))
```
The Christoffel coefficients of the first kind are a complex structure with all three indices down:
```Scheme
((Christoffel->symbols
(metric->Christoffel-1 (g-sphere ’R) S2-basis))
((point S2-spherical) (up ’theta0 ’phi0)))
(down
 (down (down 0 0)
       (down 0 (* (* (cos theta0) (sin theta0)) (expt R 2))))
 (down (down 0 (* (* (cos theta0) (sin theta0)) (expt R 2)))
       (down (* (* -1 (cos theta0) (sin theta0)) (expt R 2))
0)))
```
And the Christoffel coefficients of the second kind have the innermost index up:

#page(137)
```Scheme
((Christoffel->symbols
(metric->Christoffel-2 (g-sphere ’R) S2-basis))
((point S2-spherical) (up ’theta0 ’phi0)))
(down (down (up 0 0)
            (up 0 (/ (cos theta0) (sin theta0))))
      (down (up 0 (/ (cos theta0) (sin theta0)))
            (up (* -1 (cos theta0) (sin theta0)) 0)))
```
### Exercise  9.1: Metric Compatibility
The connections constructed from a metric by equation (9.13) are “metric compatible,” as described in equation (9.9). Demonstrate that this is true for a literal metric, as described on page 6, in R4. Your program should produce a zero.
9.2 Metrics and Lagrange Equations
In the Introduction (Chapter 1) we showed that the Lagrange equations for a free particle constrained to a 2-dimensional surface are equivalent to the geodesic equations for motion on that surface. We illustrated that in detail in Section 7.4 for motion on a sphere.
Here we expand this understanding to show that the Christoffel symbols can be derived from the Lagrange equations. Specifically, if we solve the Lagrange equations for the acceleration (the highest-order derivatives) we find that the Christoffel symbols are the symmetrized coefficients of the quadratic velocity terms.
Consider the Lagrange equations for a free particle, with Lagrangian
L2(t, x, v) = 1 g(x)(v, v). (9.15) 2
If we solve the Lagrange equations for the accelerations, the accelerations can be expressed with the geodesic equations (7.79):
 D2qi +
We can verify this computationally. Given a metric, we can construct a Lagrangian where the kinetic energy is the metric applied to the velocity twice: The kinetic energy is proportional to the squared length of the velocity vector.
jk
(Γijk ◦ χ−1 ◦ q)DqjDqk = 0. (9.16)

#page(138)
```Scheme
(define (metric->Lagrangian metric coordsys) (define (L state)
(let ((q (ref state 1)) (qd (ref state 2))) (define v
(components->vector-field (lambda (m) qd) coordsys)) ((* 1/2 (metric v v)) ((point coordsys) q))))
L)
```
The following code compares the Christoffel symbols with the coefficients of the terms of second order in velocity appearing in the accelerations, determined by solving the Lagrange equations forthehighest-orderderivative.2 Weextractthesetermsbytaking two partials with respect to the structure of velocities. Because the elementary partials commute we get two copies of each coefficient, requiring a factor of 1/2.
```Scheme
(let* ((metric (literal-metric ’g R3-rect)) (q (typical-coords R3-rect))
(L2 (metric->Lagrangian metric R3-rect))) (+ (* 1/2
(((expt (partial 2) 2) (Lagrange-explicit L2)) (up ’t q (corresponding-velocities q))))
((Christoffel->symbols (metric->Christoffel-2 metric
(coordinate-system->basis R3-rect))) ((point R3-rect) q))))
(down (down (up 0 0 0) (up 0 0 0) (up 0 0 0))
      (down (up 0 0 0) (up 0 0 0) (up 0 0 0))
      (down (up 0 0 0) (up 0 0 0) (up 0 0 0)))
```
We get a structure of zeros, demonstrating the correspondence between Christoffel symbols and coefficients of the Lagrange equations.
Thus, if we have a metric specifying an inner product, the geodesic equations are equivalent to the Lagrange equations for
2The procedure Lagrange-explicit produces the accelerations of the coordinates. In this code the division operator (/) multiplies its first argument on the left by the inverse of its second argument.
```Scheme
(define (Lagrange-explicit L) (let ((P ((partial 2) L))
(F ((partial 1) L)))
(/ (- F (+ ((partial 0) P) (* ((partial 1) P) velocity)))
((partial 2) P))))
```
#page(139)
the Lagrangian that is equal to the inner product of the generalized velocities with themselves.
Kinetic Energy or Arc Length
A geodesic is a path of stationary length with respect to variations in the path that keep the endpoints fixed. On the other hand, the solutions of the Lagrange equations are paths of stationary action that keep the endpoints fixed. How are these solutions related?
The integrand of the traditional action is the Lagrangian, which is in this case the Lagrangian L2, the kinetic energy. The integrand of the arc length is

L1(t, x, v) = g(x)(v, v) = and the path length is
L2(t, x, v) = 1 (L1(t, x, v))2, 2
and the Lagrange operator for L2 is3 E[L2] = Dt∂2L2 − ∂1L2,
we find
E[L2] = L1E[L1] + ∂2L1DtL1.
  2L2(t, x, v)
τ =
If we compute the Lagrange equations for L2 we get the La-
 t2 t1
L1(t,q(t),Dq(t))dt.
grange equations for L1 with a correction term. Since
(9.18)
L2 is the kinetic energy. It is conserved along solution paths, since there is no explicit time dependence. Because of the relation between L1 and L2, L1 is also a conserved quantity. Let L1 take the constant value a on the geodesic coordinate path q we are
3E is the Euler-Lagrange operator, which gives the residuals of the Lagrange equations for a Lagrangian. Γ extends a configuration-space path q to make a state-space path, with as many terms as needed: Γ[q](t) = (t, q(t), Dq(t), · · ·) The total time derivative Dt is defined by DtF ◦ Γ[q] = D(F ◦ Γ[q]) for any state function F and path q. The Lagrange equations are E[L] ◦ Γ[q] = 0. See [19](references!bib_19) for more details.
(9.17)
(9.19)
(9.20)
  
#page(140)
 considering. Then τ = a(t2 − t1). Since L1 is conserved, (DtL1) ◦ Γ[q] = 0 on the geodesic path q, and both E[L1] ◦ Γ[q] = 0 and E[L2] ◦ Γ[q] = 0, as required by equation (9.20).
Since L2 is homogeneous of degree 2 in the velocities, L1 is homogeneous of degree 1. So we cannot solve for the highest-order derivative in the Lagrange-Euler equations derived from L1: The Lagrange equations of the Lagrangian L1 are dependent. But although they do not uniquely specify the evolution, they do specify the geodesic path.
On the other hand, we can solve for the highest-order derivative in E[L2]. This is because L1E[L1] is homogeneous of degree 2. So the equations derived from L2 uniquely determine the time evolution along the geodesic path.
For Two Dimensions
We can show this is true for a 2-dimensional system with a general metric. We define the Lagrangians in terms of this metric:
```Scheme
(define L2
(metric->Lagrangian (literal-metric ’m R2-rect)
R2-rect))
(define (L1 state)
(sqrt (* 2 (L2 state))))
Although the mass matrix of L2 is nonsingular
(determinant
(((partial 2) ((partial 2) L2))
(up ’t (up ’x ’y) (up ’vx ’vy))))
(+ (* (m 00 (up x y)) (m 11 (up x y))) (* -1 (expt (m 01 (up x y)) 2)))
```
the mass matrix of L1 has determinant zero
```Scheme
(determinant
(((partial 2) ((partial 2) L1))
(up ’t (up ’x ’y) (up ’vx ’vy))))
0
```
showing that these Lagrange equations are dependent.
We can show this dependence explicitly, for a simple system. Consider the simplest possible system, a geodesic (straight line)
in a plane:
   
#page(141)
```Scheme
(define (L1 state)
(sqrt (square (velocity state))))
(((Lagrange-equations L1)
(up (literal-function ’x) (literal-function ’y)))
’t)
(down
 (/ (+ (* (((expt D 2) x) t) (expt ((D y) t) 2))
       (* -1 ((D x) t) ((D y) t) (((expt D 2) y) t)))
    (expt (+ (expt ((D x) t) 2) (expt ((D y) t) 2)) 3/2))
 (/ (+ (* -1 (((expt D 2) x) t) ((D x) t) ((D y) t))
       (* (expt ((D x) t) 2) (((expt D 2) y) t)))
    (expt (+ (expt ((D x) t) 2) (expt ((D y) t) 2)) 3/2)))
```
These residuals must be zero; so the numerators must be zero.4 They are:
D2x(Dy)2 =DxDyD2y D2x Dx Dy = (Dx)2 D2y
Note that the only constraint is D2x Dy = Dx D2y, so the resulting Lagrange equations are dependent.
This is enough to determine that the result is a straight line, without specifying the rate along the line. Suppose y = f(x), for path (x(t),y(t)). Then
Dy = Df (x) Dx and D2y = D2f (x) Dx + Df (x) D2x. Substituting, we get
Df(x)DxD2x = Dx(D2f(x)Dx+Df(x)D2x)
or
Df(x)D2x = D2f(x)Dx+Df(x)D2x,
so D2f(x) = 0. Thus f is a straight line, as required.
Reparameterization
More generally, a differential equation system F[q](t) = 0 is said to be reparameterized if the coordinate path q is replaced with a
4We cheated: We hand-simplified the denominator to make the result more obvious.
 
#page(142)
 new coordinate path q ◦ f . For example, we may change the scale of the independent variable. The system F [q ◦ f ] = 0 is said to be independent of the parameterization if and only if F [q] ◦ f = 0. So the differential equation system is satisfied by q ◦ f if and only if it is satisfied by q.
The Lagrangian L1 is homogeneous of degree 1 in the velocities; so
E[L1]◦Γ[q◦f]−(E[L1]◦Γ[q]◦f)Df =0. (9.21)
We can check this in a simple case. For two dimensions q = (x, y), the condition under which a reparameterization f of the geodesic paths with coordinates q satisfies the Lagrange equations for L1 is:
```Scheme
(let ((x (literal-function ’x))
(y (literal-function ’y))
(f (literal-function ’f))
(E1 (Euler-Lagrange-operator L1)))
((- (compose E1
(Gamma (up (compose x f)
                          (compose y f))
                      4))
(* (compose E1
(Gamma (up x y) 4)
f) (D f)))
’t))
(down 0 0)
```
This residual is identically satisfied, showing that the Lagrange equations for L1 are independent of the parameterization of the independent variable.
The Lagrangian L2 is homogeneous of degree 2 in the velocities; so
E[L2][q ◦ f] − (E[L2][q] ◦ f)(Df)2 = (∂2L2 ◦ Γ[q] ◦ f)(D2f).(9.22)
Although the Euler-Lagrange equations for L1 are invariant under an arbitrary reparameterization (Df ̸= 0), the Euler-Lagrange equations for L2 are invariant only for a restricted set of f. The conditions under which a reparameterization f of geodesic paths with coordinates q satisfies the Lagrange equations for L2 are:

#page(143)
```Scheme
(let ((q (up (literal-function ’x) (literal-function ’y))) (f (literal-function ’f)))
((- (compose (Euler-Lagrange-operator L2) (Gamma (compose q f) 4))
(* (compose (Euler-Lagrange-operator L2) (Gamma q 4)
                  f)
         (expt (D f) 2)))
’t))
(down
 (* (+ (* ((D x) (f t))
       (* ((D y) (f t))
    (((expt D 2) f) t))
 (* (+ (* ((D x) (f t))
       (* ((D y) (f t))
    (((expt D 2) f) t)))
(m 00 (up (x (f t)) (y (f t))))) (m01 (up (x (f t)) (y (f t))))))
(m 01 (up (x (f t)) (y (f t))))) (m11 (up (x (f t)) (y (f t))))))
```
We see that if these expressions must be zero, then D2f = 0. This tells us that f is at most affine int: f(t)=at+b.
### Exercise  9.2: SO(3) Geodesics
We have derived a basis for SO(3) in terms of incremental rotations around the rectangular axes. See equations (4.29, 4.30, 4.31). We can use the dual basis to define a metric on SO(3).
```Scheme
(define (SO3-metric v1 v2) (+ (* (e^x v1) (e^x v2)) (* (e^y v1) (e^y v2))
(* (e^z v1) (e^z v2))))
```
This metric determines a connection. Show that uniform rotation about an arbitrary axis traces a geodesic on SO(3).
### Exercise  9.3: Curvature of a Spherical Surface
The 2-dimensional surface of a 3-dimensional sphere can be embedded in three dimensions with a metric that depends on the radius:
```Scheme
(define M (make-manifold S^2-type 2 3)) (define spherical
(coordinate-system-at ’spherical ’north-pole M)) (define-coordinates (up theta phi) spherical)
(define spherical-basis (coordinate-system->basis spherical))
(define ((spherical-metric r) v1 v2) (* (square r)
(+ (* (dtheta v1) (dtheta v2)) (* (square (sin theta))
           (dphi v1) (dphi v2)))))
```
#page(144)
 If we raise one index of the Ricci tensor (see equation 8.20) by contracting it with the inverse of the metric tensor we can further contract it to obtain a scalar manifold function:
g( ̃ei,  ̃ej )R(ei, ej ). (9.23)
The trace2down procedure converts a tensor that takes two vector fields into a tensor that takes a vector field and a one-form field, and then it contracts the result over a basis to make a trace. It is useful for getting the Ricci scalar from the Ricci tensor, given a metric and a basis.
```Scheme
(define ((trace2down metric basis) tensor) (let ((inverse-metric-tensor
(metric:invert metric-tensor basis))) (contract
     (lambda (v1 w1)
       (contract
(lambda (v w)
(* (inverse-metric-tensor w1 w)
             (tensor v v1)))
        basis))
basis)))
```
Evaluate the Ricci scalar for a sphere of radius r to obtain a measure of its intrinsic curvature. You should obtain the answer 2/r2.
### Exercise  9.4: Curvature of a Pseudosphere
Compute the scalar curvature of the pseudosphere (see exercise 8.2). You should obtain the value −2.
9.3 General Relativity
By analogy to Newtonian mechanics, relativistic mechanics has two parts. There are equations of motion that describe how particles move under the influence of “forces” and there are field equations that describe how the forces arise. In general relativity the only force considered is gravity. However, gravity is not treated as a force. Instead, gravity arises from curvature in the spacetime, and the equations of motion are motion along geodesics of that space.
The geodesic equations for a spacetime with the metric
R =
ij

9.3 General Relativity
 2V g(v1,v2)=−c2 1+ c2
+ dx(v1)dx(v2) + dy(v1)dy(v2) + dz(v1)dz(v2)
145
 dt(v1)dt(v2)
 are Newton’s equations to lowest order in V/c2: D2⃗x(t) = −gradV (⃗x(t)).
### Exercise  9.5: Newton’s Equations
(9.24)
(9.25)
Verify that Newton’s equations (9.25) are indeed the lowest-order terms of the geodesic equations for the metric (9.24).
Einstein’s field equations tell how the local energy-momentum distribution determines the local shape of the spacetime, as described by the metric tensor g. The equations are traditionally written
Rμν − 1 Rgμν + Λgμν = 8πG Tμν (9.26) 2 c4
where Rμν are the components of the Ricci tensor (equation 8.20), R is the Ricci scalar (equation 9.23),5 and Λ is the cosmological constant.
Tμν are the components of the stress-energy tensor describing the energy-momentum distribution. Equivalently, one can write
8πG 1 
Rμν= c4 Tμν−2Tgμν −Λgμν (9.27)
where T = Tμνgμν.6
5The tensor with components Gμν = Rμν − 1Rgμν is called the Einstein
2
tensor. In his search for an appropriate field equation for gravity, Einstein demanded general covariance (independence of coordinate system) and local Lorentz invariance (at each point transformations must preserve the line element). These considerations led Einstein to look for a tensor equation (see Appendix C).
6Start with equation (9.26). Raise one index of both sides, and then contract. Notice that the trace gμ = 4, the dimension of spacetime. This gets R = −(8πG/c4)T, from which we can deduce equation (9.27).
      
#page(146)
 Einstein’s field equations arise from a heuristic derivation by analogy to the Poisson equation for a Newtonian gravitational field:
Lap(V ) = 4πGρ (9.28)
where V is the gravitational potential field at a point, ρ is the mass density at that point, and Lap is the Laplacian operator.
The time-time component of the Ricci tensor derived from the metric (9.24) is the Laplacian of the potential, to lowest order.
```Scheme
(define (Newton-metric M G c V) (let ((a
(+ 1 (* (/ 2 (square c))
(compose V (up x y z))))))
(define (g v1 v2)
(+ (* -1 (square c) a (dt v1) (dt v2))
         (* (dx v1) (dx v2))
         (* (dy v1) (dy v2))
         (* (dz v1) (dz v2))))
g))
(define (Newton-connection M G c V) (Christoffel->Cartan
(metric->Christoffel-2 (Newton-metric M G c V) spacetime-rect-basis)))
(define nabla
  (covariant-derivative
(Newton-connection ’M ’G ’:c
(literal-function ’V (-> (UP Real Real Real) Real)))))
(((Ricci nabla (coordinate-system->basis spacetime-rect)) d/dt d/dt)
((point spacetime-rect) (up ’t ’x ’y ’z)))
```
mess
The leading terms of the mess are
```Scheme
(+ (((partial 0) ((partial 0) V)) (up x y z)) (((partial 1) ((partial 1) V)) (up x y z)) (((partial 2) ((partial 2) V)) (up x y z)))
```
which is the Laplacian of V . The other terms are smaller by V/c2. Now consider the right-hand side of equation (9.27). In the Poisson equation the source of the gravitational potential is the density of matter. Let the time-time component of the stress-

#page(147)
 energy tensor T00 be the matter density ρ. Here is a program for the stress-energy tensor:
```Scheme
(define (Tdust rho) (define (T w1 w2)
(* rho (w1 d/dt) (w2 d/dt))) T)
If we evaluate the right-hand side expression we obtain7
(let ((g (Newton-metric ’M ’G ’:c V)))
(let ((T ij ((drop2 g spacetime-rect-basis) (Tdust ’rho))))
(let ((T ((trace2down g spacetime-rect-basis) T ij))) ((- (T ij d/dt d/dt) (* 1/2 T (g d/dt d/dt)))
   ((point spacetime-rect) (up ’t ’x ’y ’z))))))
(* 1/2 (expt :c 4) rho)
```
So, to make the Poisson analogy we get 8πG 1 
Rμν= c4 Tμν−2Tgμν −Λgμν
(9.29)
  as required.
### Exercise  9.6: Curvature of Schwarzschild Spacetime
In spherical coordinates around a nonrotating gravitating body the metric of Schwarzschild spacetime is given as:8
7The procedure trace2down is defined on page 144. This expression also uses drop2, which converts a tensor field that takes two one-form fields into a tensor field that takes two vector fields. Its definition is
```Scheme
(define ((drop2 metric-tensor basis) tensor) (lambda (v1 v2)
(contract (lambda (e1 w1)
(contract (lambda (e2 w2)
(* (metric-tensor v1 e1) (tensor w1 w2) (metric-tensor e2 v2))) basis))
basis)))
```
8The spacetime manifold is built from R4 with the addition of appropriate coordinate systems:
```Scheme
(define spacetime (make-manifold R^n 4)) (define spacetime-rect
(coordinate-system-at ’rectangular ’origin spacetime)) (define spacetime-sphere
(coordinate-system-at ’spacetime-spherical ’origin spacetime))
``` 
#page(148)
Metrics
```Scheme
(define-coordinates (up t r theta phi) spacetime-sphere)
(define (Schwarzschild-metric M G c)
(let ((a (- 1 (/ (* 2 G M) (* (square c) r)))))
(lambda (v1 v2)
(+ (* -1 (square c) a (dt v1) (dt v2))
(* (/ 1 a) (dr v1) (dr v2)) (* (square r)
(+ (* (dtheta v1) (dtheta v2)) (* (square (sin theta))
(dphi v1) (dphi v2))))))))
```
Show that the Ricci curvature of the Schwarzschild spacetime is zero. Use the definition of the Ricci tensor in equation (8.20).
### Exercise  9.7: Circular Orbits in Schwarzschild Spacetime
Test particles move along geodesics in spacetime. Now that we have a metric for Schwarzschild spacetime (page 147) we can use it to construct the geodesic equations and determine how test particles move. Consider circular orbits. For example, the circular orbit along a line of constant longitude is a geodesic, so it should satisfy the geodesic equations. Here is the equation of a circular path along the zero longitude line.
```Scheme
(define (prime-meridian r omega) (compose (point spacetime-sphere)
(lambda (t) (up t r (* omega t) 0)) (chart R1-rect)))
```
This equation will satisfy the geodesic equations for compatible values of the radius r and the angular velocity omega. If you substitute this into the geodesic equation and set the residual to zero you will obtain a constraint relating r and omega. Do it.
Surprise: You should find out that ω2r3 = GM—Kepler’s law! Exercise 9.8: Stability of Circular Orbits
In Schwarzschild spacetime there are stable circular orbits if the coordinate r is large enough, but below that value all orbits are unstable. The critical value of r is larger than the Schwarzschild horizon radius. Let’s find that value.
For example, we can consider a perturbation of the orbit of constant longitude. Here is the result of adding an exponential variation of size epsilon:
```Scheme
(define (prime-meridian+X r (compose
   (point spacetime-sphere)
   (lambda (t)
(up (+ (+ (+
         0))
   (chart R1-rect)))
epsilon X)
(ref X 0) (exp (* ’lambda t))))) (ref X 1) (exp (* ’lambda t)))))
t (* epsilon (*
r (* epsilon (*
(* (sqrt (/ (* ’G ’M) (expt r 3))) t)
(* epsilon (* (ref X 2) (exp (* ’lambda t)))))
```
#page(149)
 Plugging this into the geodesic equation yields a structure of residuals:
```Scheme
(define (geodesic-equation+X-residuals eps X) (let ((gamma (prime-meridian+X ’r eps X)))
(((((covariant-derivative Cartan gamma) d/dtau) ((differential gamma) d/dtau))
(chart spacetime-sphere)) ((point R1-rect) ’t))))
```
The characteristic equation in the eigenvalue lambda can be obtained as the numerator of the expression:
```Scheme
(determinant
(submatrix (((* (partial 1) (partial 0))
geodesic-equation+X-residuals) 0
             (up 0 0 0))
            0 3 0 3))
```
Show that the orbits are unstable if r < 6GM/c2.
### Exercise  9.9: Friedmann-Lemaˆıtre-Robertson-Walker
The Einstein tensor Gμν (see footnote 5) can be expressed as a program:
```Scheme
(define (Einstein coordinate-system metric-tensor)
(let* ((basis (coordinate-system->basis coordinate-system))
         (connection
          (Christoffel->Cartan
(metric->Christoffel-2 metric-tensor basis))) (nabla (covariant-derivative connection)) (Ricci-tensor (Ricci nabla basis)) (Ricci-scalar
((trace2down metric-tensor basis) Ricci-tensor)) ) (define (Einstein-tensor v1 v2)
(- (Ricci-tensor v1 v2)
(* 1/2 Ricci-scalar (metric-tensor v1 v2))))
    Einstein-tensor))
(define (Einstein-field-equation
coordinate-system metric-tensor Lambda stress-energy-tensor)
(let ((Einstein-tensor
(Einstein coordinate-system metric-tensor)))
(define EFE-residuals
(- (+ Einstein-tensor (* Lambda metric-tensor))
(* (/ (* 8 :pi :G) (expt :c 4)) stress-energy-tensor)))
    EFE-residuals))
```
One exact solution to the Einstein equations was found by Alexander Friedmann in 1922. He showed that a metric for an isotropic and homogeneous spacetime was consistent with a similarly isotropic and homogeneous stress-energy tensor in Einstein’s equations. In this case

#page(150)
the residuals of the Einstein equations gave ordinary differential equations for the time-dependent scale of the universe. These are called the Robertson-Walker equations. Friedmann’s metric is:
```Scheme
(define (FLRW-metric c k R)
(define-coordinates (let ((a (/ (square (b (square (* (define (g v1 v2)
(up t r theta phi) spacetime-sphere) (compose R t)) (- 1 (* k (square r))))) (compose R t) r))))
(+ (* -1 (square c) (dt v1) (dt v2)) (* a (dr v1) (dr v2))
(* b (+ (* (dtheta v1) (dtheta v2))
(* (square (sin theta)) (dphi v1) (dphi v2))))))
g))
```
Here c is the speed of light, k is the intrinsic curvature, and R is a length scale that is a function of time.
The associated stress-energy tensor is
```Scheme
(define (Tperfect-fluid rho p c metric)
(define-coordinates (up t r theta phi) spacetime-sphere) (let* ((basis (coordinate-system->basis spacetime-sphere))
(inverse-metric (metric:invert metric basis))) (define (T w1 w2)
(+ (* (+ (compose rho t)
(/ (compose p t) (square c)))
(w1 d/dt) (w2 d/dt))
(* (compose p t) (inverse-metric w1 w2))))
T))
```
where rho is the energy density, and p is the pressure in an ideal fluid model.
The Robertson-Walker equations are:
 DR(t) 2 kc2 Λc2 8πG R(t) + (R(t))2 − 3 = 3 ρ(t),
D2R(t) 2  ρ(t) p(t)
2 R(t) −3Λc2=−8πG 3 + c2 . (9.30)
Use the programs supplied to derive the Robertson-Walker equations.
### Exercise  9.10: Cosmology
For energy to be conserved, the stress-energy tensor must be constrained so that its covariant divergence is zero
∇ e μ T (  ̃e μ , ω ) = 0 ( 9 . 3 1 ) μ
for every one-form ω.
        
#page(151)
 a. Show that for the perfect fluid stress-energy tensor and the FLRW metric this constraint is equivalent to the differential equation
D(c2ρR3) + pD(R3) = 0. (9.32)
b. Assume that in a “matter-dominated universe” radiation pressure is negligible, so p = 0. Using the Robertson-Walker equations (9.30) and the energy conservation equation (9.32) show that the observation of an expanding universe is compatible with a negative curvature universe, a flat universe, or a positive curvature universe: k ∈ {−1, 0, +1}.
