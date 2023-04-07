!!Polyglot
## Integration
We know how to integrate real-valued functions of a real variable. We want to extend this idea to manifolds, in such a way that the integral is independent of the coordinate system used to compute it.
The integral of a real-valued function of a real variable is the limit of a sum of products of the values of the function on subintervals and the lengths of the increments of the independent variable in those subintervals:
b b f =
f(x)dx = lim f (xi)Δxi. (5.1) Δxi→0 i
a
a
If we change variables (x = g(y)), then the form of the integral changes:
b b
f= f(x)dx
a a g−1(b)
=
=
g−1(a)  g−1(b)
g−1(a)
f (g(y))Dg(y)dy (f ◦ g)Dg.
(5.2)
We can make a coordinate-independent notion of integration in the following way. An interval of the real line is a 1-dimensional manifold with boundary. We can assign a coordinate chart χ to this manifold. Let x = χ(m). The coordinate basis is associated with a coordinate-basis vector field, here ∂/∂x. Let ω be a oneform on this manifold. The application of ω to ∂/∂x is a realvalued function on the manifold. If we compose this with the inverse chart, we get a real-valued function of a real variable. We can then write the usual integral of this function
b a
I =
ω(∂/∂x) ◦ χ−1. (5.3)
#page(56)
 It turns out that the value of this integral is independent of the coordinate chart used in its definition. Consider a different coordinate chart x′ = χ′(m), with associated basis vector field ∂/∂x′. Let g = χ′ ◦ χ−1. We have
 b′
ω (∂/∂x′) ◦ χ′−1
a′
 b′ a′
b′ a′
b′ a′
b
a b
a
= = = = =
ω ∂/∂x D χ◦χ′−1 ◦χ′
ω(∂/∂x)D χ ◦ χ′−1 ω(∂/∂x) ◦ χ′−1 D
◦χ′−1 ◦ χ′−1
ω(∂/∂x)◦χ−1 D χ◦χ′−1 ◦g Dg ω(∂/∂x) ◦ χ−1,
◦ χ′
χ ◦ χ′−1
where we have used the rule for coordinate transformations of basis vectors (equation 3.19), linearity of forms in the first two lines, and the rule for change-of-variables under an integral in the last line.1
Because the integral is independent of the coordinate chart, we can write simply

I= ω, (5.5) M
where M is the 1-dimensional manifold with boundary corresponding to the interval.
We are exploiting the fact that coordinate basis vectors in different coordinate systems are related by a Jacobian (see equation 3.19), which cancels the Jacobian that appears in the changeof-variables formula for integration (see equation 5.2).
1 Note (D(χ◦χ′−1)◦(χ′ ◦χ−1))D(χ′ ◦χ−1) = 1. With g = χ′ ◦χ−1 this is (D(g−1) ◦ g) (Dg) = 1.
(5.4)
 
#page(57)
 5.1 Higher Dimensions
We have seen that we can integrate one-forms on 1-dimensional manifolds. We need higher-rank forms that we can integrate on higher-dimensional manifolds in a coordinate-independent manner.
Consider the integral of a real-valued function, f : Rn → R, over
a region U in Rn. Under a coordinate transformation g : Rn → Rn,
we have2 
f = (f ◦ g) det (Dg) . (5.6) U g−1(U)
A rank n form field takes n vector field arguments and produces a real-valued manifold function: ω (v, w, . . . , u) (m). By analogy with the 1-dimensional case, higher-rank forms are linear in each argument. Higher-rank forms must also be antisymmetric under interchange of any two arguments in order to make a coordinatefree definition of integration analogous to equation (5.3).
Consider an integral in the coordinate system χ: 
ω (X0, X1, . . .) ◦ χ−1. (5.7) χ(U)
Under coordinate transformations g = χ ◦ χ′−1, the integral becomes

ω(X0,X1,...)◦χ′−1 det(Dg).
χ′ (U)
Using the change-of-basis formula, equation (3.19): X(f)=X′(f)(D(χ′◦χ−1))◦χ=X′(f) D g−1 ◦χ. If we let M = (D (g−1)) ◦ χ then
(ω(X0,X1,...)◦χ′−1) det(Dg)
= (ω (X′M0, X′M1, . . .) ◦ χ′−1) det (Dg)
= (ω(X′0,X′1,...)◦χ′−1)α(M0,M1,...) det(Dg),
(5.8)
(5.9)
(5.10)
 2The determinant is the unique function of the rows of its argument that i) is linear in each row, ii) changes sign under any interchange of rows, and iii) is one when applied to the identity multiplier.

#page(58)
 using the multilinearity of ω, where Mi is the ith column of M. The function α is multilinear in the columns of M. To make a coordinate-independent integration we want the expression (5.10) to be the same as the integrand in

χ′ (U)
For this to be the case, α(M0,M1,...) must be (det(Dg))−1 = det(M). So α is an antisymmetric function, and thus so is ω.
Thus higher-rank form fields must be antisymmetric multilinear functions from vector fields to manifold functions. So we have a coordinate-independent definition of integration of form fields on a manifold and we can write

I′ =
ω(X′0,X′1,...)◦χ′−1. (5.11)
I = I′ =
Wedge Product
U
ω. (5.12)
There are several ways we can construct antisymmetric higherrank forms. Given two one-form fields ω and τ we can form a two-form field ω ∧ τ as follows:
(ω ∧ τ )(v, w) = ω(v)τ (w) − ω(w)τ (v). (5.13)
More generally we can form the wedge of higher-rank forms. Let ω be a k-form field and τ be an l-form field. We can form a (k + l)-form field ω ∧ τ as follows:
ω∧τ = (k+l)!Alt(ω⊗τ) k! l!
where, if η is a function on m vectors, Alt(η)(v0, . . . , vm−1)
(5.14)
(5.15)
(5.16)
 = 1 m!
σ∈Perm(m) and where
Parity(σ)η(vσ(0) , . . . , vσ(m−1) ),
 ω ⊗ τ (v0, . . . , vk−1, vk, . . . , vk+l−1)
= ω(v0,...,vk−1)τ(vk,...,vk+l−1).

5.1 Higher Dimensions
59
   u(m)
A(u, v)(m)
v(m)
 m
Figure 5.1 The area of the parallelogram in the (x,y) coordinate plane is given by A (u, v) (m).
The wedge product is associative, and thus we need not specify the order of a multiple application. The factorial coefficients of these formulas are chosen so that
(dx∧dy∧...)(∂/∂x,∂/∂y,...) = 1.
This is true independent of the coordinate system. Equation (5.17) gives us

U
(5.17)
(5.18)
dx ∧ dy ∧ . . . = Volume(U)
where Volume(U) is the ordinary volume of the region corresponding to U in the Euclidean space of Rn with the orthonormal coordinate system (x,y,...).3
An example two-form (see figure 5.1) is the oriented area of a parallelogram in the (x,y) coordinate plane at the point m spanned by two vectors u = u0∂/∂x + u1∂/∂y and v = v0∂/∂x + v1∂/∂y, which is given by
A (u, v) (m) = u0 (m) v1 (m) − v0 (m) u1 (m) . (5.19)
3By using the word “orthonormal” here we are assuming that the range of the coordinate chart is an ordinary Euclidean space with the usual Euclidean metric. The coordinate basis in that chart is orthonormal. Under these conditions we can usefully use words like “length,” “area,” and “volume” in the coordinate space.
 
#page(60)
 Note that this is the area of the parallelogram in the coordinate plane, which is the range of the coordinate function. It is not the area on the manifold. To define that, we need more structure—the metric. We will put a metric on the manifold in Chapter 9.
3-Dimensional Euclidean Space
Let’s specialize to 3-dimensional Euclidean space. Following equation (5.18) we can write the coordinate-area two-form in another way: A = dx∧dy. As code:
```Scheme
(define-coordinates (up x y z) R3-rect)
(define u (+ (* ’u^0 d/dx) (* ’u^1 d/dy))) (define v (+ (* ’v^0 d/dx) (* ’v^1 d/dy)))
(((wedge dx dy) u v) R3-rect-point)
(+ (* uˆ0 vˆ1) (* -1 uˆ1 vˆ0))
```
If we use cylindrical coordinates and define cylindrical vector fields we get the analogous answer in cylindrical coordinates:
```Scheme
(define-coordinates (up r theta z) R3-cyl)
(define a (+ (* ’a^0 d/dr) (* ’a^1 d/dtheta))) (define b (+ (* ’b^0 d/dr) (* ’b^1 d/dtheta)))
(((wedge dr dtheta) a b) ((point R3-cyl) (up ’r0 ’theta0 ’z0)))
(+ (* aˆ0 bˆ1) (* -1 aˆ1 bˆ0))
```
The moral of this story is that this is the area of the parallelogram in the coordinate plane. It is not the area on the manifold!
There is a similar story with volumes. The wedge product of the elements of the coordinate basis is a three-form that measures our usual idea of coordinate volumes in R3 with a Euclidean metric:
#page(61)
```Scheme
(define u (+ (* ’u^0 d/dx) (* ’u^1 d/dy) (* ’u^2 d/dz))) (define v (+ (* ’v^0 d/dx) (* ’v^1 d/dy) (* ’v^2 d/dz))) (define w (+ (* ’w^0 d/dx) (* ’w^1 d/dy) (* ’w^2 d/dz)))
(((wedge dx dy dz) u v w) R3-rect-point)
(+ (* uˆ0 vˆ1 wˆ2)
(* -1 uˆ0 vˆ2 wˆ1) (* -1 uˆ1 vˆ0 wˆ2) (* uˆ1 vˆ2 wˆ0)
(* uˆ2 vˆ0 wˆ1)
(* -1 uˆ2 vˆ1 wˆ0))
```
This last expression is the determinant of a 3 × 3 matrix:
```Scheme
(- (((wedge dx dy dz) u v w) R3-rect-point) (determinant
(matrix-by-rows (list ’u^0 ’u^1 ’u^2) (list ’v^0 ’v^1 ’v^2)
(list ’w^0 ’w^1 ’w^2))))
0
```
If we did the same operations in cylindrical coordinates we would get the analogous formula, showing that what we are computing is volume in the coordinate space, not volume on the manifold.
Because of antisymmetry, if the rank of a form is greater than the dimension of the manifold then the form is identically zero. The k-forms on an n-dimensional manifold form a module of dimension nk . We can write a coordinate-basis expression for a k-form as
(5.20)
n
i0 ,...,ik−1 =0
ωi0,...,ik−1dxi0 ∧...∧dxik−1.
ω=
The antisymmetry of the wedge product implies that
ωiσ(0),...,iσ(k−1) = Parity(σ)ωi0,...,ik−1, of ω.
Exercise 5.1: Wedge Product
Pick a coordinate system and use the computer to verify that
a. the wedge product is associative for forms in your coordinate system; b. formula (5.17) is true in your coordinate system.
(5.21) from which we see that there are only nk independent components

#page(62)
 5.2 Exterior Derivative
The intention of introducing the exterior derivative is to capture all of the classical theorems of “vector analysis” into one unified Stokes’s Theorem, which asserts that the integral of a form on the boundary of a manifold is the integral of the exterior derivative of the form on the interior of the manifold:4

ω= dω. (5.22) ∂M M
As we have seen in equation (3.34), the differential of a function on a manifold is a one-form field. If a function on a manifold is considered to be a form field of rank zero,5 then the differential operator increases the rank of the form by one. We can generalize this to k-form fields with the exterior derivative operation.
Consider a one-form ω. We define6
dω(v1, v2) = v1(ω(v2)) − v2(ω(v1)) − ω([v1, v2]). (5.23)
More generally, the exterior derivative of a k-form field is a k + 1- form field, given by:7
dω(v0,...,vk) = (5.24)
k ((−1)ivi(ω(v0, . . . , vi−1, vi+1, . . . , vk))+ i=0
k
(−1)i+j ω([vi,vj],v0,...,vi−1,vi+1,...,vj−1,vj+1,...,vk))}.
j =i+1
This formula is coordinate-system independent. This is the way
we compute the exterior derivative in our software.
4This is a generalization of the Fundamental Theorem of Calculus. 5A manifold function f induces a form field ˆf of rank 0 as follows: ˆf()(m) = f(m).
6The definition is chosen to make Stokes’s Theorem pretty. 7See Spivak, Differential Geometry, Volume 1, p.289.
 
5.2 Exterior Derivative
63
 If the form field ω is represented in a coordinate basis n−1
ai0,...,ik−1dxi0 ∧···∧dxik−1
ω=
then the exterior derivative can be expressed as
(5.25)
dai0,...,ik−1 ∧dxi0 ∧···∧dxik−1.
the result is independent of the choice of coordinate system.
i0 =0,...,ik−1 =0
n−1
i0 =0,...,ik−1 =0
dω=
Though this formula is expressed in terms of a coordinate basis,
Computing Exterior Derivatives
We can test that the computation indicated by equation (5.24) is equivalent to the computation indicated by equation (5.26) in three dimensions with a general one-form field:
```Scheme
(define a (literal-manifold-function ’alpha R3-rect)) (define b (literal-manifold-function ’beta R3-rect)) (define c (literal-manifold-function ’gamma R3-rect))
(define theta (+ (* a dx) (* b dy) (* c dz)))
The test will require two arbitrary vector fields
(define X (literal-vector-field ’X-rect R3-rect)) (define Y (literal-vector-field ’Y-rect R3-rect))
(((- (d theta)
(+ (wedge (d a) dx)
(wedge (d b) dy)
(wedge (d c) dz))) X Y)
 R3-rect-point)
0
```
We can also try a general two-form field in 3-dimensional space: Let
ω = ady∧dz+bdz∧dx+cdx∧dy, (5.27)
where a = α ◦ χ, b = β ◦ χ, c = γ ◦ χ, and α, β, and γ are real-valued functions of three real arguments. As a program,
(5.26)

#page(64)
```Scheme
(define omega
(+ (* a (wedge dy dz))
(* b (wedge dz dx)) (* c (wedge dx dy))))
```
Here we need another vector field because our result will be a three-form field.
```Scheme
(define Z (literal-vector-field ’Z-rect R3-rect))
(((- (d omega)
     (+ (wedge
(d a) dy dz) (wedge (d b) dz dx)
(wedge (d c) dx dy))) X Y Z)
 R3-rect-point)
0
```
Properties of Exterior Derivatives
The exterior derivative of the wedge of two form fields obeys the graded Leibniz rule. It can be written in terms of the exterior derivatives of the component form fields:
d(ω∧τ)=dω∧τ +(−1)kω∧dτ, (5.28)
where k is the rank of ω.
A form field ω that is the exterior derivative of another form
field ω = dθ is called exact. A form field whose exterior derivative is zero is called closed.
Every exact form field is a closed form field: applying the exterior derivative operator twice always yields zero:
d2ω = 0. (5.29)
This is equivalent to the statement that partial derivatives with respect to different variables commute.8
It is easy to show equation (5.29) for manifold functions:
d2f(u, v) = d(df)(u, v)
= u(df(v)) − v(df(u)) − df([u, v])
= u(v(f)) − v(u(f)) − [u, v](f)
= 0 (5.30)
 8See Spivak, Calculus on Manifolds, p.92

#page(65)
 Consider the general one-form field θ defined on 3-dimensional rectangular space. Taking two exterior derivatives of θ yields a three-form field. It is zero:
```Scheme
(((d (d theta)) X Y Z) R3-rect-point)
0
```
Not every closed form field is an exact form field. Whether a closed form field is exact depends on the topology of a manifold.
5.3 Stokes’s Theorem
The proof of the general Stokes’s Theorem for n-dimensional orientable manifolds is quite complicated, but it is easy to see how it works for a 2-dimensional region M that can be covered with a single coordinate patch.9
Given a coordinate chart χ(m) = (x(m), y(m)) we can obtain a pair of coordinate-basis vectors ∂/∂x = X0 and ∂/∂y = X1.
The coordinate image of M can be divided into small rectangular areas in the (x,y) coordinate plane. The union of the rectangular areas gives the coordinate image of M. The clockwise integrals around the boundaries of the rectangles cancel on neighboring rectangles, because the boundary is traversed in opposite directions. But on the boundary of the coordinate image of M the boundary integrals do not cancel, yielding an integral on the boundary of M. Area integrals over the rectangular areas add to produce an integral over the entire coordinate image of M.
So, consider Stokes’s Theorem on a small patch P of the manifold for which the coordinates form a rectangular region (xmin < x < xmax and ymin < y < ymax). Stokes’s Theorem on P states 
ω= dω. (5.31) ∂P P
The area integral on the right can be written as an ordinary multidimensional integral using the coordinate basis vectors (recall
9We do not develop the machinery for integration on chains that is usually needed for a full proof of Stokes’s Theorem. This is adequately done in other books. A beautiful treatment can be found in Spivak, Calculus on Manifolds [17].
 
#page(66)
 that the integral is independent of the choice of coordinates):

dω (∂/∂x, ∂/∂y) ◦ χ−1 (5.32)
χ(P)
= (∂/∂x(ω(∂/∂y)) − ∂/∂y(ω(∂/∂x))) ◦ χ−1.
 xmax  ymax xmin ymin
We have used equation (5.23) to expand the exterior derivative. Consider just the first term of the right-hand side of equation (5.32). Then using the definition of basis vector field ∂/∂x
we obtain
 xmax  ymax
∂/∂x(ω(∂/∂y)) ◦ χ−1
xmin =
=
ymin
 xmax  ymax
X0(ω(∂/∂y)) ◦ χ−1 ∂0 (ω(∂/∂y)) ◦ χ−1 .
xmin xmax
xmin
ymin ymax
ymin
(5.33)
This integral can now be evaluated using the Fundamental Theorem of Calculus. Accumulating the results for both integrals

dω (∂/∂x, ∂/∂y) ◦ χ−1
χ(P)
= (ω(∂/∂x)) ◦ χ−1
 xmax xmin
(x, ymin)dx
ymax ymin
− −
(xmax, y)dy (ω(∂/∂x))◦χ−1 (x,ymax)dx
ymin = ω,

∂P
as was to be shown.
(ω(∂/∂y)) ◦ χ−1  xmax
xmin ymax
(ω(∂/∂y)) ◦ χ−1
(xmin, y)dy
(5.34)

#page(67)
 5.4 Vector Integral Theorems
Green’s Theorem states that for an arbitrary compact set M ⊂ R2, a 2-dimensional Euclidean space:

((α ◦ χ) dx + (β ◦ χ) dy) = ((∂0β − ∂1α) ◦ χ) dx ∧ dy.(5.35) ∂M M
We can test this. By Stokes’s Theorem, the integrands are related by an exterior derivative. We need some vectors to test our forms:
```Scheme
(define v (literal-vector-field ’v-rect R2-rect)) (define w (literal-vector-field ’w-rect R2-rect))
We can now test our integrands:10
(define alpha (literal-function ’alpha R2->R)) (define beta (literal-function ’beta R2->R))
(let ((dx
      (dy
  (((- (d
(ref (basis->1form-basis R2-rect-basis) 0)) (ref (basis->1form-basis R2-rect-basis) 1))) (+ (* (compose alpha (chart R2-rect)) dx)
(* (compose beta (chart R2-rect)) dy))) (compose (- ((partial 0) beta)
((partial 1) alpha)) (chart R2-rect))
(*
          (wedge dx dy)))
    v w)
   R2-rect-point))
```
0
We can also compute the integrands for the Divergence Theorem: For an arbitrary compact set M ⊂ R3 and a vector field w 
div(w)dV = w·ndA (5.36) M ∂M
where n is the outward-pointing normal to the surface ∂M. Again, the integrands should be related by an exterior derivative, if this is an instance of Stokes’s Theorem.
10Using (define R2-rect-basis (coordinate-system->basis R2-rect)). Here we extract dx and dy from R2-rect-basis to avoid globally installing
coordinates.
 
#page(68)
 Note that even the statement of this theorem cannot be made with the machinery we have developed at this point. The concepts “outward-pointing normal,” area A, and volume V on the manifold are not definable without using a metric (see Chapter 9). However, for orthonormal rectangular coordinates in R3 we can interpret the integrands in terms of forms.
Let the vector field describing the flow of stuff be
w=a∂ +b∂ +c∂. (5.37)
   ∂x ∂y ∂z
The rate of leakage of stuff through each element of the bound-
ary is w · n dA. We interpret this as the two-form
a dy ∧ dz + b dz ∧ dx + c dx ∧ dy, (5.38)
because any part of the boundary will have y-z, z-x, and x-y components, and each such component will pick up contributions from the normal component of the flux w. Formalizing this as code we have
```Scheme
(define a (literal-manifold-function ’a-rect R3-rect)) (define b (literal-manifold-function ’b-rect R3-rect)) (define c (literal-manifold-function ’c-rect R3-rect))
(define flux-through-boundary-element (+ (* a (wedge dy dz))
(* b (wedge dz dx)) (* c (wedge dx dy))))
```
The rate of production of stuff in each element of volume is div(w) dV . We interpret this as the three-form
∂∂∂
∂xa+ ∂yb+ ∂zc dx∧dy∧dz. (5.39)
or:
```Scheme
(define production-in-volume-element (* (+ (d/dx a) (d/dy b) (d/dz c))
(wedge dx dy dz)))
```
Assuming Stokes’s Theorem, the exterior derivative of the leakage of stuff per unit area through the boundary must be the rate of production of stuff per unit volume in the interior. We check this
   
#page(69)
 by applying the difference to arbitrary vector fields at an arbitrary point:
```Scheme
(define X (literal-vector-field ’X-rect R3-rect)) (define Y (literal-vector-field ’Y-rect R3-rect)) (define Z (literal-vector-field ’Z-rect R3-rect))
(((- production-in-volume-element
(d flux-through-boundary-element))
  X Y Z)
 R3-rect-point)
0
```
as expected.
Exercise 5.2: Graded Formula
Derive equation (5.28).
Exercise 5.3: Iterated Exterior Derivative
We have shown that the equation (5.29) is true for manifold functions. Show that it is true for any form field.
