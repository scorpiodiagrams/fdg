!!Polyglot
## Curvature
If the intrinsic curvature of a manifold is not zero, a vector paralleltransported around a small loop will end up different from the vector that started. We saw the consequence of this before, on page 1 and on page 93. The Riemann tensor encapsulates this idea.
The Riemann curvature operator is
R(w, v) = [∇w, ∇v] − ∇[w,v]. (8.1) The traditional Riemann tensor is1
R(ω, u, w, v) = ω((R(w, v))(u)), (8.2)
where ω is a one-form field that measures the incremental change in the vector field u caused by parallel-transporting it around the loop defined by the vector fields w and v. R allows us to compute the intrinsic curvature of a manifold at a point.
The Riemann curvature is computed by
```Scheme
(define ((Riemann-curvature nabla) w v) (- (commutator (nabla w) (nabla v))
(nabla (commutator w v))))
```
The Riemann-curvature procedure is parameterized by the relevant covariant-derivative operator nabla, which implements ∇. The nabla is itself dependent on the connection, which provides the details of the local geometry. The same Riemann-curvature procedure works for ordinary covariant derivatives and for covariant derivatives over a map. Given two vector fields, the result of ((Riemann-curvature nabla) w v) is a procedure that takes a vector field and produces a vector field so we can implement the Riemann tensor as
1 [11](references!bib_11), [4](references!bib_4), and [14](references!bib_14) use our definition. [20](references!bib_20) uses a different convention for the order of arguments and a different sign. See Appendix C for a definition of tensors.
 
#page(116)
Curvature
```Scheme
(define ((Riemann nabla) omega u w v)
(omega (((Riemann-curvature nabla) w v) u)))
So, for example,2
(((Riemann (covariant-derivative sphere-Cartan)) dphi d/dtheta d/dphi d/dtheta)
((point S2-spherical) (up ’theta0 ’phi0)))
1
```
Here we have computed the φ component of the result of carrying a ∂/∂θ basis vector around the parallelogram defined by ∂/∂φ and ∂/∂θ. The result shows a net rotation in the φ direction.
Most of the sixteen coefficients of the Riemann tensor for the sphere are zero. The following are the nonzero coefficients:
∂∂∂ 2 dθ, ∂φ, ∂θ, ∂φ (χ−1(qθ,qφ)) = sin(qθ)
R R R R
8.1 Explicit Transport
, ∂∂∂ 2
   dθ, ∂φ, ∂φ, ∂θ ∂∂∂ dφ, ∂θ, ∂θ, ∂φ ∂∂∂ dφ, ∂θ, ∂φ, ∂θ
(χ−1(qθ,qφ)) = − sin(qθ) , (χ−1(qθ,qφ)) = −1, (χ−1(qθ,qφ)) = 1.
(8.3)

We will show that the result of the Riemann calculation of the change in a vector, as we traverse a loop, is what we get by explicitly calculating the transport. The coordinates of the vector to be transported are governed by the differential equations (see equation 7.72)
πij(v)(χ−1(σ(t)))uj(t) (8.4) 2The connection specified by sphere-Cartan is defined on page 107.
Dui(t) = −
j
 
#page(117)
 and the coordinates as a function of time, σ = χ ◦ γ ◦ χ−1, of the path γ, are governed by the differential equations3 R
Dσ(t) = v(χ)(χ−1(σ(t))).
(8.5)
We have to integrate these equations (8.4, 8.5) together to transport the vector over the map uγ a finite distance along the vector field v.
Let s(t) = (σ(t),u(t)) be a state tuple, combining σ the coordinates of γ, and u the coordinates of uγ. Then
Ds(t) = (Dσ(t), Du(t)) = g(s(t)), (8.6)
where g is the tuple of right-hand sides of equations (8.4, 8.5). The differential equations describing the evolution of a function
h of state s along the state path are
D(h ◦ s) = (Dh ◦ s)(g ◦ s) = Lgh ◦ s, (8.7)
defining the operator Lg.
Exponentiation gives a finite evolution:4
h(s(t + ε)) = (eεLg h)(s(t)). (8.8) The finite parallel transport of the vector with components u is u(t + ε) = (eεLg U )(s(t)), (8.9)
where the selector U(σ,u) = u, and the initial state is s(t) = (σ(t), u(t)).
Consider parallel-transporting a vector u around a parallelogram defined by two coordinate-basis vector fields w and v. The vector u is really a vector over a map, where the map is the parametric curve describing our parallelogram. This map is implicitly defined in terms of the vector fields w and v. Let gw and gv be the right-hand sides of the differential equations for parallel transport
3The map γ takes points on the real line to points on the target manifold. The chart χ gives coordinates of points on the target manifold while χR gives a time coordinate on the real line.
4The series may not converge for large increments in the independent variable. In this case it is appropriate to numerically integrate the differential equations directly.
 
#page(118)
 along w and v respectively. Then evolution along w for interval ε, then along v for interval ε, then reversing w, and reversing v, brings σ back to where it started to second order in ε.
The state s = (σ, u) after transporting s0 around the loop is5 (e−εLgv I) ◦ (e−εLgw I) ◦ (eεLgv I) ◦ (eεLgw I)(s0)
= (eεLgw eεLgv e−εLgw e−εLgv I)(s0) = (eε2[Lgw ,Lgv ]+···I)(s0).
So the lowest-order change in the transported vector is
ε2U(([Lgw ,Lgv ]I)(s0)),
(8.10)
(8.11)
where U(σ,u) = u.
However, if w and v do not commute, the indicated loop does
not bring σ back to the starting point, to second order in ε. We must account for the commutator. (See figure 4.2.) In the general case the lowest order change in the transported vector is
ε2U((([Lgw ,Lgv ] − Lg[w,v] )I)(s0)). (8.12) This is what the Riemann tensor computation gives, scaled by ε2.
Verification in Two Dimensions
We can verify this in two dimensions. We need to make the structure representing a state:
```Scheme
(define (make-state sigma u) (vector sigma u)) (define (Sigma state) (ref state 0))
(define (U-select state) (ref state 1))
```
5 The parallel-transport operators are evolution operators, and therefore descend into composition:
eA(F ◦ G) = F ◦ (eAG),
for any state function G and any compatible F. As a consequence, we have
the following identity:
eAeBI = eA((eBI) ◦ I) = (eBI) ◦ (eAI), where I is the identity function on states.
 
#page(119)
 And now we get to the meat of the matter: First we find the rate of change of the components of the vector u as we carry it along the vector field v.6
```Scheme
(define ((Du v) state)
(let ((CF (Cartan->forms general-Cartan-2)))
(* -1
((CF v) (Chi-inverse (Sigma state))) (U-select state))))
```
We also need to determine the rate of change of the coordinates of the integral curve of v.
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
(* ((D h) state) ((g v) state))) (make-operator l))
```
So now we can demonstrate that the lowest-order change resulting from explicit parallel transport of a vector around an infinitesimal loop is what is computed by the Riemann curvature.
6 The setup for this experiment is a bit complicated. We need to make a manifold with a general connection.
```Scheme
(define Chi-inverse (point R2-rect)) (define Chi (chart R2-rect))
```
We now make the Cartan forms from the most general 2-dimensional Christoffel coefficient structure:
#page(120)
```Scheme
(define general-Cartan-2 (Christoffel->Cartan
(literal-Christoffel-2 ’Gamma R2-rect)))
(let ((U (literal-vector-field ’U-rect R2-rect)) (W (literal-vector-field ’W-rect R2-rect)) (V (literal-vector-field ’V-rect R2-rect)) (sigma (up ’sigma0 ’sigma1)))
(let ((nabla (covariant-derivative general-Cartan-2)) (m (Chi-inverse sigma)))
(let ((s (make-state sigma ((U Chi) m)))) (- (((- (commutator (L V) (L W))
(up 0 0)
(L (commutator V W))) U-select)
s)
(((((Riemann-curvature nabla) W V) U) Chi) m)))))
```
Geometrically
The explicit transport above was done with differential equations operating on a state consisting of coordinates and components of the vector being transported. We can simplify this so that it is entirely built on manifold objects, eliminating the state. After a long algebraic story we find that
((R(w, v))(u))(f)
= e(f) {(w(π(v)) − v(π(w)) − π([w, v])) ̃e(u)
+π(w)π(v) ̃e(u) − π(v)π(w) ̃e(u)}
or as a program:
```Scheme
(define ((((curvature-from-transport Cartan) w v) u) f) (let* ((CF (Cartan->forms Cartan))
(basis (Cartan->basis Cartan))
(8.13)
(fi
(ei (* (ei f)
(+ (*
(-
(basis->1form-basis basis)) (basis->vector-basis basis)))
(- (- (w (CF v)) (v (CF w))) (CF (commutator w v)))
(fi u))
(* (CF w) (* (CF v) (fi u)))
(* (CF v) (* (CF w) (fi u))))))))
```
This computes the same operator as the traditional Riemann curvature operator:

#page(121)
```Scheme
(define (test coordsys Cartan)
(let ((m (typical-point coordsys))
(u (literal-vector-field ’u-coord coordsys))
(w (literal-vector-field ’w-coord coordsys))
(v (literal-vector-field ’v-coord coordsys))
(f (literal-manifold-function ’f-coord coordsys)))
(let ((nabla (covariant-derivative Cartan)))
(- (((((curvature-from-transport Cartan) w v) u) f) m)
(((((Riemann-curvature nabla) w v) u) f) m))))) (test R2-rect general-Cartan-2)
0
(test R2-polar general-Cartan-2)
0
```
Terms of the Riemann Curvature
Since the Riemann curvature is defined as in equation (8.1),
R(w, v) = [∇w, ∇v] − ∇[w,v], (8.14)
it is natural7 to identify these terms with the corresponding terms in
(([Lgw , Lgv ] − Lg[w,v] )U )(s0 ).
Unfortunately, this does not work, as demonstrated below:
```Scheme
(let ((U (literal-vector-field ’U-rect R2-rect))
(V (literal-vector-field ’V-rect R2-rect))
(W (literal-vector-field ’W-rect R2-rect)) (nabla (covariant-derivative general-Cartan-2)) (sigma (up ’sigma0 ’sigma1)))
(let ((m (Chi-inverse sigma)))
(let ((s (make-state sigma ((U Chi) m))))
(- (((commutator (L W) (L V)) U-select) s) ((((commutator (nabla W) (nabla V)) U) Chi)
m)))))
```
a nonzero mess
(8.15)
7People often say “Geodesic evolution is exponentiation of the covariant derivative.” But this is wrong. The evolution is by exponentiation of Lg.

#page(122)
The obvious identification does not work, but neither does the other one!
```Scheme
(let ((U (literal-vector-field ’U-rect R2-rect))
(V (literal-vector-field ’V-rect R2-rect))
(W (literal-vector-field ’W-rect R2-rect)) (nabla (covariant-derivative general-Cartan-2)) (sigma (up ’sigma0 ’sigma1)))
(let ((m (Chi-inverse sigma)))
(let ((s (make-state sigma ((U Chi) m))))
(- (((commutator (L W) (L V)) U-select) s) ((((nabla (commutator W V)) U) Chi)
m)))))
```
a nonzero mess
Let’s compute the two parts of the Riemann curvature operator and see how this works out. First, recall

∇vu(f) = ei(f) v( ̃ei(u)) + πij(v) ̃ej(u) (8.16) ij
= e(f) (v( ̃e(u)) + π(v) ̃e(u)) , (8.17) where the second form uses tuple arithmetic. Now let’s consider
the first part of the Riemann curvature operator:
[∇w,∇v]u
= ∇w∇vu − ∇v∇wu
= e {w(v( ̃e(u)) + π(v) ̃e(u)) + π(w)(v( ̃e(u)) + π(v) ̃e(u))}
− e {v(w( ̃e(u)) + π(w) ̃e(u)) + π(v)(w( ̃e(u)) + π(w) ̃e(u))} = e {[w, v] ̃e(u)
+ w(π(v)) ̃e(u) − v(π(w)) ̃e(u)
+π(w)π(v) ̃e(u) − π(v)π(w) ̃e(u)} . (8.18)
The second term of the Riemann curvature operator is
∇[w,v]u = e {[w, v] ̃e(u) + π([w, v]) ̃e(u)} . (8.19)
The difference of these is the Riemann curvature operator. Notice that the first term in each cancels, and the rest gives equation (8.13).

#page(123)
 Ricci Curvature
One measure of the curvature is the Ricci tensor, which is computed from the Riemann tensor by
R( ̃ei,u,ei,v). (8.20)
R(u,v) =
Expressed as a program:
i
```Scheme
(define ((Ricci nabla basis) u v)
(contract (lambda (ei wi) ((Riemann nabla) wi u ei v))
basis))
```
Einstein’s field equation (9.27) for gravity, which we will encounter later, is expressed in terms of the Ricci tensor.
### Exercise  8.1: Ricci of a Sphere
Compute the components of the Ricci tensor of the surface of a sphere.
### Exercise  8.2: Pseudosphere
A pseudosphere is a surface in 3-dimensional space. It is a surface of revolution of a tractrix about its asymptote (along the zˆ-axis). We can make coordinates for the surface (t, θ) where t is the coordinate along the asymptote and θ is the angle of revolution. We embed the pseudosphere in rectangular 3-dimensional space with
```Scheme
(define (pseudosphere q)
(let ((t (ref q 0)) (theta (ref q 1)))
(up (* (sech t) (cos theta)) (* (sech t) (sin theta)) (- t (tanh t)))))
```
The structure of Christoffel coefficients for the pseudosphere is
```Scheme
(down
 (down (up (/ (+
(* 2 (expt (cosh t) 2) (expt (sinh t) 2)) (* -2 (expt (sinh t) 4)) (expt (cosh t) 2) (* -2 (expt (sinh t) 2)))
(+ (* (cosh t) (expt (sinh t) 3)) (* (cosh t) (sinh t))))
0) (up 0
           (/ (*
 (down (up 0
(up (/ 0)))
Note that this is independent of θ.
Compute the components of the Ricci tensor.
(/ (*
-1 (sinh t)) (cosh t))))
-1 (sinh t)) (cosh t)))
(cosh t) (+ (expt (sinh t) 3) (sinh t)))
```
#page(124)
 8.2 Torsion
There are many connections that describe the local properties of any particular manifold. A connection has a property called torsion, which is computed as follows:
T (u, v) = ∇uv − ∇vu − [u, v]. (8.21)
The torsion takes two vector fields and produces a vector field. The torsion depends on the covariant derivative, which is constructed from the connection.
We account for this dependency by parameterizing the program by nabla.
```Scheme
(define ((torsion-vector nabla) u v) (- (- ((nabla u) v) ((nabla v) u))
(commutator u v)))
(define ((torsion nabla) omega u v) (omega ((torsion-vector nabla) u v)))
```
The torsion for the connection for the 2-sphere specified by the Christoffel coefficients S2-Christoffel above is zero. We demonstrate this by applying the torsion to the basis vector fields:
```Scheme
(for-each
 (lambda (x)
   (for-each
    (lambda (y)
(print-expression
((((torsion-vector (covariant-derivative sphere-Cartan))
x y)
(literal-manifold-function ’f S2-spherical))
((point S2-spherical) (up ’theta0 ’phi0))))) (list d/dtheta d/dphi)))
(list d/dtheta d/dphi))
0 0 0 0
```
Torsion Doesn’t Affect Geodesics
There are multiple connections that give the same geodesic curves. Among these connections there is always one with zero torsion. Thus, if you care about only geodesics, it is appropriate to use a torsion-free connection.

#page(125)
 Consider a basis e and its dual  ̃e. The components of the torsion are
 ̃ek(T(ei,ej)) = Γkij − Γkji + dkij, (8.22)
where dkij are the structure constants of the basis. See equations (4.37, 4.38). For a commuting basis the structure constants are zero, and the components of the torsion are the antisymmetric part of Γ with respect to the lower indices.
Recall the geodesic equation (7.79):
D2σi(t) +
Observe that the lower indices of Γ are contracted with two copies of the velocity. Because the use of Γ is symmetrical here, any asymmetry of Γ in its lower indices is irrelevant to the geodesics. Thus one can study the geodesics of any connection by first symmetrizing the connection, eliminating torsion. The resulting equations will be simpler.
8.3 Geodesic Deviation
Geodesics may converge and intersect (as in the lines of longitude on a sphere) or they may diverge (for example, on a saddle). To capture this notion requires some measure of the convergence or divergence, but this requires metrics (see Chapter 9). But even in the absence of a metric we can define a quantity, the geodesic deviation, that can be interpreted in terms of relative acceleration of neighboring geodesics from a reference geodesic.
Let there be a one-parameter family of geodesics, with parameter s, and let T be the vector field of tangent vectors to those geodesics:
∇TT = 0. (8.24) We can parameterize travel along the geodesics with parameter t:
a geodesic curve γs(t) = φTt (ms) where
f ◦ φTt (ms) = (etT f)(ms). (8.25)
jk
Γijk(γ(t))Dσj(t)Dσk(t) = 0. (8.23)

#page(126)
 Let U = ∂/∂s be the vector field corresponding to the displacement of neighboring geodesics. Locally, (t,s) is a coordinate system on the 2-dimensional submanifold formed by the family of geodesics. The vector fields T and U are a coordinate basis for this coordinate system, so [T, U] = 0.
The geodesic deviation vector field is defined as:
∇T(∇TU). (8.26)
If the connection has zero torsion, the geodesic deviation can be related to the Riemann curvature:
∇T(∇TU) = −R(U, T)(T), (8.27) as follows, using equation (8.21),
∇T(∇TU) = ∇T(∇UT), (8.28) because both the torsion is zero and [T, U] = 0. Continuing
∇T(∇TU) = ∇T(∇UT)
= ∇T(∇UT) + ∇U(∇TT) − ∇U(∇TT)
= ∇U(∇TT) − R(U, T)(T)
= −R(U, T)(T). (8.29)
In the last line the first term was dropped because T satisfies the geodesic equation (8.24).
The geodesic deviation is defined without using a metric, but it helps to have a metric (see Chapter 9) to interpret the geodesic deviation. Consider two neighboring geodesics, with parameters s and s + Δs. Given a metric we can assume that t is proportional to path length along each geodesic, and we can define a distance δ(s, t, Δs) between the geodesics at the same value of the parameter t. So the velocity of separation of the two geodesics is
(∇TU)Δs = ∂1δ(s, t, Δs)sˆ (8.30)
where sˆ is a unit vector in the direction of increasing s. So ∇TU is the factor of increase of velocity with increase of separation. Similarly, the geodesic deviation can be interpreted as the factor of increase of acceleration with increase of separation:
∇T(∇TU)Δs = ∂1∂1δ(s, t, Δs)sˆ. (8.31)

#page(127)
 Longitude Lines on a Sphere
Consider longitude lines on the unit sphere.8 Let theta be colatitude and phi be longitude. These are the parameters s and t, respectively. Then let T be the vector field d/dtheta that is tangent to the longitude lines.
We can verify that every longitude line is a geodesic:
```Scheme
((omega (((covariant-derivative Cartan) T) T)) m)
0
```
where omega is an arbitrary one-form field. Now let U be d/dphi, then U commutes with T:
```Scheme
(((commutator U T) f) m)
0
```
The torsion for the usual connection for the sphere is zero:
```Scheme
(let ((X (literal-vector-field ’X-sphere S2-spherical)) (Y (literal-vector-field ’Y-sphere S2-spherical)))
((((torsion-vector nabla) X Y) f) m))
0
```
So we can compute the geodesic deviation using Riemann
```Scheme
((+ (omega ((nabla T) ((nabla T) U))) ((Riemann nabla) omega T U T))
m)
0
```
confirming equation (8.29).
Lines of longitude are geodesics. How do the lines of longi-
tude behave? As we proceed from the North Pole, the lines of constant longitude diverge. At the Equator they are parallel and they converge towards the South Pole.
Let’s compute ∇TU and ∇T(∇TU). We know that the distance is purely in the φ direction, so
8The setup for this example is:
```Scheme
(define-coordinates (up theta phi) S2-spherical) (define T d/dtheta)
(define U d/dphi)
(define m ((point S2-spherical) (up ’theta0 ’phi0))) (define Cartan (Christoffel->Cartan S2-Christoffel)) (define nabla (covariant-derivative Cartan))
((dphi ((nabla T) U)) m)
(/ (cos theta0) (sin theta0))
((dphi ((nabla T) ((nabla T) U))) m)
-1
```
#page(128)
Let’s interpret these results. On a sphere of radius R the distance at colatitude θ between two geodesics separated by Δφ is d(φ, θ, Δφ) = R sin(θ)Δφ. Assuming that θ is uniformly increasing with time, the magnitude of the velocity is just the θ-derivative of this distance:
```Scheme
(define ((delta R) phi theta Delta-phi) (* R (sin theta) Delta-phi))
(((partial 1) (delta ’R)) ’phi0 ’theta0 ’Delta-phi)
(* Delta-phi R (cos theta0))
```
The direction of the velocity is the unit vector in the φ direction: 
```Scheme
(define phi-hat
(* (/ 1 (sin theta)) d/dphi))
```
This comes from the fact that the separation of lines of longitude is proportional to the sine of the colatitude. So the velocity vector field is the product.
We can measure the φ component with dφ:
```Scheme
((dphi (* (((partial 1) (delta ’R)) ’phi0 ’theta0 ’Delta-phi)
phi-hat))
m)
(/ (* Delta-phi R (cos theta0)) (sin theta0))
```
This agrees with ∇TUΔφ for the unit sphere. Indeed, the lines of longitude diverge until they reach the Equator and then they converge.
Similarly, the magnitude of the acceleration is
```Scheme
(((partial 1) ((partial 1) (delta ’R))) ’phi0 ’theta0 ’Delta-phi)
(* -1 Delta-phi R (sin theta0))
```
and the acceleration vector is the product of this result with φˆ. Measuring this with dφ we get:
#page(129)
```Scheme
((dphi (* (((partial 1) ((partial 1) (delta ’R))) ’phi0 ’theta0 ’Delta-phi)
phi-hat))
m)
(* -1 Delta-phi R)
```
And this agrees with the calculation of ∇T∇TUΔφ for the unit sphere. We see that the separation of the lines of longitude are uniformly decelerated as they progress from pole to pole.
8.4 Bianchi Identities
There are some important mathematical properties of the Riemann curvature. These identities will be used to constrain the possible geometries that can occur.
A system with a symmetric connection, Γijk = Γikj, is torsion free.9
```Scheme
(define nabla
  (covariant-derivative
(Christoffel->Cartan (symmetrize-Christoffel
(literal-Christoffel-2 ’C R4-rect)))))
(((torsion nabla) omega X Y) (typical-point R4-rect))
0
```
The Bianchi identities are defined in terms of a cyclic-summation operator, which is most easily described as a Scheme procedure:
```Scheme
(define ((cyclic-sum f) x y z) (+ (f x y z)
     (f y z x)
     (f z x y)))
```
9Setup for this section:
```Scheme
(define omega (literal-1form-field ’omega-rect R4-rect)) (define X (literal-vector-field ’X-rect R4-rect)) (define Y (literal-vector-field ’Y-rect R4-rect)) (define Z (literal-vector-field ’Z-rect R4-rect)) (define V (literal-vector-field ’V-rect R4-rect))
``` 
#page(130)
Chapter 8
Curvature
 The first Bianchi identity is
R(ω,x,y,z) + R(ω,y,z,x) + R(ω,z,x,y) = 0,
or, as a program:
```Scheme
(((cyclic-sum
   (lambda (x y z)
((Riemann nabla) omega x y z))) X Y Z)
(typical-point R4-rect))
0
```
The second Bianchi identity is
∇xR(ω,v,y,z) + ∇yR(ω,v,z,x) + ∇zR(ω,v,x,y) = 0
or, as a program:
```Scheme
(((cyclic-sum
   (lambda (x y z)
(((nabla x) (Riemann nabla)) omega V y z)))
X Y Z)
(typical-point R4-rect))
0
```
(8.32)
Things get more complicated when there is torsion. We can make a general connection, which has torsion:
```Scheme
(define nabla
  (covariant-derivative
(Christoffel->Cartan (literal-Christoffel-2 ’C R4-rect))))
(define R (Riemann nabla)) (define T (torsion-vector nabla))
(define (TT omega x y) (omega (T x y)))
```
(8.33)

#page(131)
 The first Bianchi identity is now:10
```Scheme
(((cyclic-sum (lambda (x y z)
(- (R omega x y z)
(+ (omega (T (T x y) z))
(((nabla x) TT) omega y z))))) (typical-point R4-rect))
X Y Z)
0
```
and the second Bianchi identity for a general connection is
```Scheme
(((cyclic-sum (lambda (x y z)
(+ (((nabla x) R) omega V y z) (R omega V (T x y) z))))
X Y Z)
(typical-point R4-rect))
0
```
10The Bianchi identities are much nastier to write in traditional mathematical notation than as Scheme programs.
