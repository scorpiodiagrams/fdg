!!Polyglot
## Directional Derivatives
The vector field was a generalization of the directional derivative to functions on a manifold. When we want to generalize the directional derivative idea to operate on other manifold objects, such as directional derivatives of vector fields or of form fields, there are several useful choices. In the same way that a vector field applies to a function to produce a function, we will build directional derivatives so that when applied to any object it will produce another object of the same kind. All directional derivatives require a vector field to give the direction and scale factor.
We will have a choice of directional derivative operators that give different results for the rate of change of vector and form fields along integral curves. But all directional derivative operators must agree when computing rates of change of functions along integral curves. When applied to functions, all directional derivative operators give:
Dv(f) = v(f). (7.1)
Next we specify the directional derivative of a vector field u with respect to a vector field v. Let an integral curve of the vector field v be γ, parameterized by t, and let m = γ(t). Let u′ be a vector field that results from transporting the vector field u along γ for a parameter increment δ. How u is transported to make u′ determines the type of derivative. We formulate the method of transport by:
u ′ = F δv u . ( 7 . 2 )
We can assume without loss of generality that Fδvu is a linear transformation over the reals on u, because we care about its behavior only in an incremental region around δ = 0.
Let g be the comparison of the original vector field at a point with the transported vector field at that point:
g(δ) = u(f)(m) − (Fδvu) (f)(m). (7.3)

#page(84)
 So we can compute the directional derivative operator using only ordinary derivatives:
Dvu(f)(m) = Dg(0). (7.4)
The result Dvu is of type vector field.
The general pattern of constructing a directional derivative op-
erator from a transport operator is given by the following schema:1
(define (((((F->directional-derivative F) v) u) f) m) (define (g delta)
(- ((u f) m) (((((F v) delta) u) f) m))) ((D g) 0))
The linearity of transport implies that
Dv(αO + βP) = αDvO + βDvP,
(7.5)
for any real α and β and manifold objects O and P.
The directional derivative obeys superposition in its vector-field
argument:
Dv+w = Dv + Dw. (7.6)
The directional derivative is homogeneous over the reals in its vector-field argument:
Dαv = αDv, (7.7) foranyrealα.2 Thisfollowsfromthefactthatforevolutionalong
integral curves: when α is a real number,
φαv(m) = φv (m). (7.8)
t αt
When applied to products of functions, directional derivative operators satisfy Leibniz’s rule:
Dv(fg) = f (Dvg) + (Dvf) g. (7.9)
1The directional derivative of a vector field must itself be a vector field. Thus the real program for this must make the function of f into a vector field. However, we leave out this detail here to make the structure clear.
2For some derivative operators α can be a real-valued manifold function.
 
#page(85)
 The Leibniz rule is extended to applications of one-form fields to vector fields:
Dv(ω(y)) = ω (Dvy) + (Dvω) (y). (7.10)
The extension of the Leibniz rule, combined with the choice of transport of a vector field, determines the action of the directional derivative on form fields.3
7.1 Lie Derivative
The Lie derivative is one kind of directional derivative operator. We write the Lie derivative operator with respect to a vector field v as Lv.
Functions
The Lie derivative of the function f with respect to the vector field v is given by
Lvf = v(f). (7.11) The tangent vector v measures the rate of change of f along inte-
gral curves.
Vector Fields
For the Lie derivative of a vector field y with respect to a vector field v we choose the transport operator Fδvy to be the pushforward of y along the integral curves of v. Recall equation (6.15). So the Lie derivative of y with respect to v at the point m is
(Lvy) (f)(m) = Dg(0), (7.12)
where
g(δ) = y(f)(m) − ((φvδ)∗ y)(f)(m). (7.13)
We can construct a procedure that computes the Lie derivative of a vector field by supplying an appropriate transport operator
3The action on functions, vector fields, and one-form fields suffices to define the action on all tensor fields. See Appendix C.
 
#page(86)
 (F-Lie phi) for F in our schema F->directional-derivative. In this first stab at the Lie derivative, we introduce a coordinate system and we expand the integral curve to a given order. Because in the schema we evaluate the derivative of g at 0, the dependence on the order and the coordinate system disappears. They will not be needed in the final version.
(define (Lie-directional coordsys order) (let ((Phi (phi coordsys order)))
(F->directional-derivative (F-Lie Phi))))
(define (((F-Lie phi) v) delta)
(pushforward-vector ((phi v) delta) ((phi v) (- delta))))
(define ((((phi coordsys order) v) delta) m) ((point coordsys)
(series:sum (((exp (* delta v)) (chart coordsys)) m) order)))
Expand the quantities in equation (7.13) to first order in δ:
g(δ) = y(f)(m) − (φvδ∗y)(f)(m)
= y(f)(m) − y(f ◦ φvδ)(φv−δ(m))
= (y(f) − y(f + δv(f) + ···) + δv(y(f + δv(f) + ···)))(m) + ··· = (−δy(v(f)) + δv(y(f)))(m) + · · ·
=δ[v,y](f)(m)+O δ2 . (7.14)
So the Lie derivative of a vector field y with respect to a vector field v is a vector field that is defined by its behavior when applied to an arbitrary manifold function f:
(Lvy) (f) = [v, y] (f)
Verifying this computation
(let ((v (literal-vector-field ’v-rect R3-rect))
(w (literal-vector-field ’w-rect R3-rect))
(f (literal-manifold-function ’f-rect R3-rect)))
((- ((((Lie-directional R3-rect 2) v) w) f) ((commutator v w) f))
((point R3-rect) (up ’x0 ’y0 ’z0))))
0
(7.15)

#page(87)
 Although this is tested to second order, evaluating the derivative at zero ensures that first order is enough. So we can safely define:
(define ((Lie-derivative-vector V) Y) (commutator V Y))
We can think of the Lie derivative as the rate of change of the manifold function y(f) as we move in the v direction, adjusted to take into account that some of the variation is due to the variation of f:
(Lvy) (f) = [v, y] (f)
= v(y(f)) − y(v(f))
= v(y(f)) − y (Lv(f)) . (7.16)
The first term in the commutator, v(y(f)), measures the rate of change of the combination y(f) along the integral curves of v. The change in y(f) is due to both the intrinsic change in y along the curve and the change in f along the curve; the second term in the commutator subtracts this latter quantity. The result is the intrinsic change in y along the integral curves of v.
Additionally, we can extend the product rule, for any manifold function g and any vector field u:
Lv(gu)(f) = [v, gu](f)
= v(g)u(f) + g[v, u](f)
= (Lvg)u(f) + g(Lvu)(f).
An Alternate View
We can write the vector field
(7.17)
(7.18)
(7.19)
i
Lvy(f) =
(v(yi)ei(f) + yiLvei(f)).
yi ei(f).
y(f) =
By the extended product rule (equation 7.17) we get
i

#page(88)
 Because the Lie derivative of a vector field is a vector field, we can extract the components of Lvei using the dual basis. We define Δij(v) to be those components:
Δij(v) =  ̃ei (Lvej) =  ̃ei([v,ej]).
So the Lie derivative can be written

( L v y ) ( f ) = v ( y i ) + Δ ij ( v ) y j ij
e i ( f ) .
(7.20)
( 7 . 2 1 )
The components of the Lie derivatives of the basis vector fields are the structure constants for the basis vector fields. (See equation 4.37.) The structure constants are antisymmetric in the lower indices:
 ̃e i ( L e k e j ) =  ̃e i ( [ e k , e j ] ) = d ik j . ( 7 . 2 2 ) Resolving v into components and applying the product rule, we
(7.23)
get (Lvy)(f) =
vk[ek,y](f)−y(vk)ek(f) .
So Δij is related to the structure constants by
Δ ij ( v ) =  ̃e i ( L v e j )
= vk ̃ei([ek,ej])−ej(vk) ̃ei(ek)
k
k
k
= =
v k d ik j − e j ( v k ) δ ki vkdikj − ej(vi).
(7.24)
k
Note: Despite their appearance, the Δij are not form fields because Δij(fv) ̸= fΔij(v).

#page(89)
 Form Fields
We can also define the Lie derivative of a form field ω with respect to the vector field v by its action on an arbitrary vector field y, using the extended Leibniz rule (see equation 7.10):
(Lv(ω)) (y) ≡ v (ω(y)) − ω (Lvy) . (7.25)
The first term computes the rate of change of the combination ω(y) along the integral curve of v, while the second subtracts ω applied to the change in y. The result is the change in ω along the curve.
The Lie derivative of a k-form field ω with respect to a vector field v is a k-form field that is defined by its behavior when applied to k arbitrary vector fields w0, . . . , wk−1. We generalize equation (7.25):
Lvω(w0, . . . , wk−1)
= v(ω(w0, . . . , wk−1)) −
Uniform Interpretation
Consider abstracting the equations (7.16), (7.25), and (7.27). The Lie derivative of an object, a, that can apply to other objects, b, to produce manifold functions, a(b) : M → Rn, is
(Lva) (b) = v (a(b)) − a (Lvb) . (7.27)
The first term in this expression computes the rate of change of the compound object a(b) along integral curves of v, while the second subtracts the change in a due to the change in b along the curves. The result is a measure of the “intrinsic” change in a along integral curves of v, with b held “fixed.”
Properties of the Lie Derivative
As required by properties 7.7–7.5, the Lie derivative is linear in its arguments:
Lαv+βw = αLv + βLw, (7.28)
k−1 i=0
(7.26) ω(w0, . . . , Lvwi, . . . , wk−1).

#page(90)
 and
Lv (αa + βb) = αLva + βLvb,
(7.29)
with α, β ∈ R and vector fields or one-form fields a and b.
For any k-form field ω and any vector field v the exterior derivative commutes with the Lie derivative with respect to the vector
field:
Lv(dω) = d(Lvω). (7.30)
If ω is an element of surface then dω is an element of volume. The Lie derivative computes the rate of change of its argument under a deformation described by the vector field. The answer is the same whether we deform the surface before computing the volume or compute the volume and then deform it.
We can verify this in 3-dimensional rectangular space for a general one-form field:4
(((- ((Lie-derivative V) (d theta)) (d ((Lie-derivative V) theta)))
  X Y)
 R3-rect-point)
0
and for the general two-form field:
4In these experiments we need some setup.
(define a (literal-manifold-function ’alpha R3-rect)) (define b (literal-manifold-function ’beta R3-rect)) (define c (literal-manifold-function ’gamma R3-rect))
 (define-coordinates (up x y z) R3-rect)
(define theta (+ (* a dx) (* b dy) (* c
(define omega
(+ (* a (wedge dy dz))
(* b (wedge dz dx)) (* c (wedge dx dy))))
(define X (literal-vector-field ’X-rect (define Y (literal-vector-field ’Y-rect (define Z (literal-vector-field ’Z-rect (define V (literal-vector-field ’V-rect (define R3-rect-point
((point R3-rect) (up ’x0 ’y0 ’z0)))
dz)))
R3-rect))
R3-rect))
R3-rect))
R3-rect))

#page(91)
 (((- ((Lie-derivative V) (d omega)) (d ((Lie-derivative V) omega)))
  X Y Z)
 R3-rect-point)
0
The Lie derivative satisfies a another nice elementary relationship. If v and w are two vector fields, then
[Lv, Lw] = L[v,w]. (7.31) Again, for our general one-form field θ:
((((- (commutator (Lie-derivative X) (Lie-derivative Y)) (Lie-derivative (commutator X Y)))
theta) Z)
 R3-rect-point)
0
and for the two-form field ω:
((((- (commutator (Lie-derivative X) (Lie-derivative Y)) (Lie-derivative (commutator X Y)))
omega) Z V)
 R3-rect-point)
0
Exponentiating Lie Derivatives
The Lie derivative computes the rate of change of objects as they are advanced along integral curves. The Lie derivative of an object produces another object of the same type, so we can iterate Lie derivatives. This gives us Taylor series for objects along the curve.
The operator etLv = 1 + tLv + t2 L2v + . . . evolves objects along 2!
the curve by parameter t. For example, the exponential of a Lie derivative applied to a vector field is
tLv t2 2
e y=y+tLvy+2Lvy+···
t2
= y + t[v,y] + 2 [v,[v,y]] + ···. (7.32)
   
#page(92)
 Consider a simple case. We advance the coordinate-basis vector field ∂/∂y by an angle a around the circle. Let Jz = x ∂/∂y − y ∂/∂x, the circular vector field. We recall
(define Jz (- (* x d/dy) (* y d/dx)))
We can apply the exponential of the Lie derivative with respect to Jz to ∂/∂y. We examine how the result affects a general function on the manifold:
(series:for-each print-expression
((((exp (* ’a (Lie-derivative Jz))) d/dy)
(literal-manifold-function ’f-rect R3-rect)) ((point R3-rect) (up 1 0 0)))
5)
(((partial 0) f-rect) (up 1 0))
(* -1 a (((partial 1) f-rect) (up 1 0)))
(* -1/2 (expt a 2) (((partial 0) f-rect) (up 1 0)))
(* 1/6 (expt a 3) (((partial 1) f-rect) (up 1 0)))
(* 1/24 (expt a 4) (((partial 0) f-rect) (up 1 0)))
;Value: ...
There is a simple but useful operation available between vector fields and form fields called interior product. This is the substitution of a vector field v into the first argument of a p-form field ω to produce a p − 1-form field:
(ivω)(v1, . . . vp−1) = ω(v, v1, . . . vp−1). (7.34) There is a mundane identity corresponding to the product rule
for the Lie derivative of an interior product:
Lv (iyω) = iLvyω + iy (Lvω). (7.35)
And there is a rather nice identity for the Lie derivative in terms of the interior product and the exterior derivative, called Cartan’s formula:
Lvω = iv(dω) + d(ivω). (7.36)
Apparently the result is
exp aL ∂ = −sin(a) ∂ + cos(a) ∂ .
(7.33)
(x ∂/∂y−y ∂/∂x) ∂y ∂x ∂y Interior Product
   
#page(93)
 We can verify Cartan’s formula in a simple case with a program:
(define X (literal-vector-field ’X-rect R3-rect)) (define Y (literal-vector-field ’Y-rect R3-rect)) (define Z (literal-vector-field ’Z-rect R3-rect))
(define a (literal-manifold-function ’alpha R3-rect)) (define b (literal-manifold-function ’beta R3-rect)) (define c (literal-manifold-function ’gamma R3-rect))
(define omega
(+ (* a (wedge dx dy))
(* b (wedge dy dz)) (* c (wedge dz dx))))
(define ((L1 X) omega)
(+ ((interior-product X) (d omega))
(d ((interior-product X) omega))))
((- (((Lie-derivative X) omega) Y Z) (((L1 X) omega) Y Z))
((point R3-rect) (up ’x0 ’y0 ’z0)))
0
Note that iv ◦iu +iu ◦iv = 0. One consequence of this is that iv ◦ iv = 0.
7.2 Covariant Derivative
The covariant derivative is another kind of directional derivative operator. We write the covariant derivative operator with respect to a vector field v as ∇v. This is pronounced “covariant derivative with respect to v” or “nabla v.”
Covariant Derivative of Vector Fields
We may also choose our Fδvu to define what we mean by “parallel” transport of the vector field u along an integral curve of the vector field v. This may correspond to our usual understanding of parallel in situations where we have intuitive insight.
The notion of parallel transport is path dependent. Remember our example from the Introduction, page 1: Start at the North Pole carrying a stick along a line of longitude to the Equator, always pointing it south, parallel to the surface of the Earth. Then

#page(94)
 proceed eastward for some distance, still pointing the stick south. Finally, return to the North Pole along this new line of longitude, keeping the stick pointing south all the time. At the pole the stick will not point in the same direction as it did at the beginning of the trip, and the discrepancy will depend on the amount of eastward motion.5
So if we try to carry a stick parallel to itself and tangent to the sphere, around a closed path, the stick generally does not end up pointing in the same direction as it started. The result of carrying the stick from one point on the sphere to another depends on the path taken. However, the direction of the stick at the endpoint of a path does not depend on the rate of transport, just on the particular path on which it is carried. Parallel transport over a zero-length path is the identity.
A vector may be resolved as a linear combination of other vectors. If we parallel-transport each component, and form the same linear combination, we get the transported original vector. Thus parallel transport on a particular path for a particular distance is a linear operation.
So the transport function Fδv is a linear operator on the components of its argument, and thus
Fδvu(f)(m) =
for some functions Aij that depend on the particular path (hence its tangent vector v) and the initial point. We reach back along the integral curve to pick up the components of u and then paralleltransport them forward by the matrix Aij(δ) to form the components of the parallel-transported vector at the advanced point.
As before, we compute
∇vu(f)(m) = Dg(0), (7.38) where
g(δ) = u(f)(m) − (Fδvu) (f)(m). (7.39)
5In the introduction the stick was always kept east-west rather than pointing south, but the phenomenon is the same!
i,j
(Aij(δ)(uj ◦ φv−δ)ei(f))(m) (7.37)
 
7.2 Covariant Derivative
95
 Expanding with respect to a basis {ei} we get 
g(δ) = uiei(f) − Aij(δ)(uj ◦ φv−δ)ei(f) ij
By the product rule for derivatives, Dg(δ) =
(m).
(7.40)
(7.41) Aij(δ)((v(uj))◦φv−δ)ei(f)−DAij(δ)(uj ◦φv−δ)ei(f) (m).
ij
S o , s i n c e A ij ( 0 ) ( m ) i s t h e i d e n t i t y m u l t i p l i e r , a n d φ v0 i s t h e i d e n t i t y
function,
Dg(0) = v(ui)(m)ei(f) − DAij(0)uj(m)ei(f) (m). (7.42)
ij
We need DAij(0). Parallel transport depends on the path, but not on the parameterization of the path. From this we can deduce that DAij(0) can be written as one-form fields applied to the vector field v, as follows.
Introduce B to make the dependence of As on v explicit:
Aij (δ) = Bji (v)(δ). (7.43)
Parallel transport depends on the path but not on the rate along the path. Incrementally, if we scale the vector field v by ξ,

d (B(v)(δ)) = d (B(ξv)(δ/ξ)). dδ dδ
Using the chain rule D(B(v))(δ) = 1D(B(ξv))(δ),
so, for δ = 0,
ξD(B(v))(0) = D(B(ξv))(0).
(7.44)
(7.45)
(7.46)
    ξξ
The scale factor ξ can vary from place to place. So DAij(0) is
homogeneous in v over manifold functions. This is stronger than the homogeneity required by equation (7.7).

#page(96)
 The superposition property (equation (7.6)) is true of the ordinary directional derivative of manifold functions. By analogy we require it to be true of directional derivatives of vector fields.
These two properties imply that DAij(0) is a one-form field: DAij(0) = −πij(v), (7.47)
where the minus sign is a matter of convention.
As before, we can take a stab at computing the covariant deriva-
tive of a vector field by supplying an appropriate transport operator for F in F->directional-derivative. Again, this is expanded to a given order with a given coordinate system. These will be unnecessary in the final version.
(define (covariant-derivative-vector omega coordsys order) (let ((Phi (phi coordsys order)))
(F->directional-derivative (F-parallel omega Phi coordsys))))
(define ((((((F-parallel omega phi coordsys) v) delta) u) f) m) (let ((basis (coordinate-system->basis coordsys)))
So Dg(0) =
(* ((e f) m) (* Aij ui)))))))

v(ui)(m) + πij(v)(m)uj(m) ij
ei(f)(m).
(7.48)
(7.49)
(let ((etilde (basis->1form-basis basis)) (e (basis->vector-basis basis)))
(let ((m0 (((phi v) (- delta)) m)))
(let ((Aij (+ (identity-like ((omega v) m0))
(* delta (- ((omega v) m0))))) (ui ((etilde u) m0)))
Thus the covariant derivative is

∇vu(f) = v(ui) + πij(v)uj ei(f). ij
The one-form fields πij are called the Cartan one-forms, or the connection one-forms. They are defined with respect to the basis e.

#page(97)
 As a program, the covariant derivative is:6
(define ((((covariant-derivative-vector Cartan) V) U) f) (let ((basis (Cartan->basis Cartan))
(Cartan-forms (Cartan->forms Cartan)))
(let ((vector-basis (basis->vector-basis basis))
(1form-basis (basis->1form-basis basis))) (let ((u-components (1form-basis U)))
(* (vector-basis f)
(+ (V u-components)
(* (Cartan-forms V) u-components)))))))
An important property of ∇vu is that it is linear over manifold functions g in the first argument
∇gvu(f) = g∇vu(f), (7.50)
consistent with the fact that the Cartan forms πij share the same property.
Additionally, we can extend the product rule, for any manifold function g and any vector field u:

∇v(gu)(f) = v(gui) + πij(v)guj ij
ei(f)
v(g)uiei(f) + g∇v(u)(f) = (∇vg)u(f) + g∇v(u)(f).
An Alternate View
=
i
As we did with the Lie derivative (equations 7.18–7.21), we can write the vector field
(7.52)
(7.53)
ui(m)ei(f)(m).
u(f)(m) =
By the extended product rule, equation (7.51), we get:
∇vu(f) =
i
i
(v(ui)ei(f) + ui∇vei(f)).
(7.51)
 6This program is incomplete. It must construct a vector field; it must make a differential operator; and it does not apply to functions or forms.

#page(98)
 Because the covariant derivative of a vector field is a vector field we can extract the components of ∇vei using the dual basis:
πij(v) =  ̃ei (∇vej). (7.54) This gives an alternate expression for the Cartan one forms. So

∇vu(f) = v(ui) + πij(v)uj ei(f). (7.55) ij
This analysis is parallel to the analysis of the Lie derivative, except that here we have the Cartan form fields πij and there we had Δij, which are not form fields.
Notice that the Cartan forms appear here (equation 7.53) in terms of the covariant derivatives of the basis vectors. By contrast, in the first derivation (see equation 7.42) the Cartan forms appear as the derivatives of the linear forms that accomplish the parallel transport of the coefficients.
The Cartan forms can be constructed from the dual basis oneforms:
πij(v)(m) =
The connection coefficient functions Γijk are called the Christoffel coefficients (traditionally called Christoffel symbols).7 Making use of the structures,8 the Cartan forms are
π(v) = Γ  ̃e(v). (7.57) Conversely, the Christoffel coefficients may be obtained from the
Cartan forms
Γijk = πij(ek). (7.58)
7This terminology may be restricted to the case in which the basis is a coordinate basis.
8The structure of the Cartan forms π together with this equation forces the shape of the Christoffel coefficient structure.
k
Γijk(m) ̃ek(v)(m). (7.56)
 
#page(99)
 Covariant Derivative of One-Form Fields
The covariant derivative of a vector field induces a compatible covariant derivative for a one-form field. Because the application of a one-form field to a vector field yields a manifold function, we can evaluate the covariant derivative of such an application. Let τ be a one-form field and w be a vector field. Then
∇v(τ(w)) = =
= = =
v

τjwj
j
v(τj)wj + τjv(wj)
j   v(τj)wj + τj  ̃ej(∇vw) − πjk(v)wk
jk

v(τj)wj − τj πjk(v)wk + τ(∇vw) jk
v(τj) ̃ej −τj πjk(v) ̃ek (w)+τ(∇vw). jk
So if we define the covariant derivative of a one-form field to be

∇ v ( τ ) = v ( τ k ) − τ j π jk ( v )  ̃e k , ( 7 . 5 9 ) kj
then the generalized product rule holds:
∇v (τ(u)) = (∇vτ)(u)+τ (∇vu). (7.60)
Alternatively, assuming the generalized product rule forces the definition of covariant derivative of a one-form field.
As a program this is
(define ((((covariant-derivative-1form Cartan) V) tau) U) (let ((nabla V ((covariant-derivative-vector Cartan) V)))
(- (V (tau U)) (tau (nabla V U)))))
  
#page(100)
 This program extends naturally to higher-rank form fields:
(define ((((covariant-derivative-form Cartan) V) tau) vs) (let ((k (get-rank tau))
(nabla V ((covariant-derivative-vector Cartan) V))) (- (V (apply tau vs))
(sigma (lambda (i) (apply tau
(list-with-substituted-coord vs i (nabla V (list-ref vs i)))))
0 (- k 1)))))
Change of Basis
The basis-independence of the covariant derivative implies a relationship between the Cartan forms in one basis and the equivalent Cartan forms in another basis. Recall (equation 4.13) that the basis vector fields of two bases are always related by a linear transformation. Let J be the matrix of coefficient functions and let e and e′ be down tuples of basis vector fields. Then
e(f) = e′(f)J. (7.61)
We want the covariant derivative to be independent of basis. This will determine how the connection transforms with a change of basis:

∇vu(f) = ei(f) v(ui) + πij(v)uj ij
= e′i(f)Jij v (J−1)jk(u′)k + πjk(v)(J−1)kl(u′)l ijk l
= e′i(f) v((u′)i) + Jijv (J−1)jk (u′)k
  i jk
+ J i j π jk ( v ) ( J − 1 ) kl ( u ′ ) l
 jkl 
= e′i(f) v((u′)i) + (π′)ij(v)(u′)j . (7.62) ij

#page(101)
 The last line of equation (7.62) gives the formula for the covariant derivative we would have written down naturally in the primed coordinates; comparing with the next-to-last line, we see that
π′(v) = Jv J−1 + Jπ(v)J−1. (7.63)
This transformation rule is weird. It is not a linear transformation of π because the first term is an offset that depends on v. So it isnotrequiredthatπ′ =0whenπ=0. Thusπisnotatensor field. See Appendix C.
We can write equation (7.61) in terms of components
j
e′j(f)Jj . i
ei(f) =
LetK=J−1,soKi(m)Jj (m)=δi.Then
(7.64)
(7.65)
jjkk
π′i(v)= Ji v(Kj )+ Ji πj(v)Kk. ljljkl
j jk
The transformation rule for π is implemented in the following program:
(define (Cartan-transform Cartan basis-prime) (let ((basis (Cartan->basis Cartan))
(forms (Cartan->forms Cartan))
(prime-dual-basis (basis->1form-basis basis-prime))
(prime-vector-basis (basis->vector-basis basis-prime))) (let ((vector-basis (basis->vector-basis basis))
(1form-basis (basis->1form-basis basis)))
(let ((J-inv (s:map/r 1form-basis prime-vector-basis))
(J (s:map/r prime-dual-basis vector-basis))) (let ((omega-prime-forms
(procedure->1form-field (lambda (v)
(+ (* J (v J-inv))
(* J (* (forms v) J-inv)))))))
(make-Cartan omega-prime-forms basis-prime))))))
The s:map/r procedure constructs a tuple of the same shape as its second argument whose elements are the result of applying the first argument to the corresponding elements of the second argument.

#page(102)
 We can illustrate that the covariant derivative is independent of the coordinate system in a simple case, using rectangular and polar coordinates in the plane.9 We can choose Christoffel coefficients for rectangular coordinates that are all zero:10
(define R2-rect-Christoffel (make-Christoffel
(let ((zero (lambda (m) 0))) (down (down (up zero zero)
(up zero zero)) (down (up zero zero)
                 (up zero zero))))
   R2-rect-basis))
With these Christoffel coefficients, parallel transport preserves the components relative to the rectangular basis. This corresponds to our usual notion of parallel in the plane. We will see later in Chapter 9 that these Christoffel coefficients are a natural choice for the plane. From these we obtain the Cartan form:11
(define R2-rect-Cartan
(Christoffel->Cartan R2-rect-Christoffel))
And from equation (7.63) we can get the corresponding Cartan form for polar coordinates:
(define R2-polar-Cartan
(Cartan-transform R2-rect-Cartan R2-polar-basis))
9We will need a few definitions:
(define R2-rect-basis (coordinate-system->basis R2-rect)) (define R2-polar-basis (coordinate-system->basis R2-polar)) (define-coordinates (up x y) R2-rect)
(define-coordinates (up r theta) R2-polar)
10Since the Christoffel coefficients are basis-dependent they are packaged with the basis.
11The code for making the Cartan forms is as follows:
(define (Christoffel->Cartan Christoffel)
(let ((basis (Christoffel->basis Christoffel))
(Christoffel-symbols (Christoffel->symbols Christoffel))) (make-Cartan
(* Christoffel-symbols (basis->1form-basis basis)) basis)))
 
#page(103)
 The vector field ∂/∂θ generates a rotation in the plane (the same as circular). The covariant derivative with respect to ∂/∂x of ∂/∂θ applied to an arbitrary manifold function is:
(define circular (- (* x d/dy) (* y d/dx)))
(define f (literal-manifold-function ’f-rect R2-rect)) (define R2-rect-point ((point R2-rect) (up ’x0 ’y0)))
(((((covariant-derivative R2-rect-Cartan) d/dx) circular)
  f)
 R2-rect-point)
(((partial 1) f-rect) (up x0 y0))
Note that this is the same thing as ∂/∂y applied to the function: ((d/dy f) R2-rect-point)
(((partial 1) f-rect) (up x0 y0))
In rectangular coordinates, where the Christoffel coefficients are zero, the covariant derivative ∇uv is the vector whose coefficents are obtained by applying u to the coefficients of v. Here, only one coefficient of ∂/∂θ depends on x, the coefficient of ∂/∂y, and it depends linearly on x. So ∇∂/∂x∂/∂θ = ∂/∂y. (See figure 7.1.)
Note that we get the same answer if we use polar coordinates to compute the covariant derivative:
(((((covariant-derivative R2-polar-Cartan) d/dx) J) f) R2-rect-point)
(((partial 1) f-rect) (up x0 y0))
In rectangular coordinates the Christoffel coefficients are all zero; in polar coordinates there are nonzero coefficients, but the value of the covariant derivative is the same. In polar coordinates the basis elements vary with position, and the Christoffel coefficients compensate for this.
Of course, this is a pretty special situation. Let’s try something more general:

104
Chapter 7
Directional Derivatives
   Δν
ν’ ν
d/dy
d/dx
If v and v′ are “arrow” representations of vectors in the circular field and we parallel-transport v in the ∂/∂x direction, then the difference between v′ and the parallel transport of v is in the ∂/∂y direction.
(define V (literal-vector-field ’V-rect R2-rect)) (define W (literal-vector-field ’W-rect R2-rect))
(((((- (covariant-derivative R2-rect-Cartan) (covariant-derivative R2-polar-Cartan))
V) W)
  f)
 R2-rect-point)
0
7.3 Parallel Transport
We have defined parallel transport of a vector field along integral curves of another vector field. But not all paths are integral curves of a vector field. For example, paths that cross themselves are not integral curves of any vector field.
Here we extend the idea of parallel transport of a stick to make sense for arbitrary paths on the manifold. Any path can be written as a map γ from the real-line manifold to the manifold M. We
      Figure 7.1

#page(105)
 construct a vector field over the map uγ by parallel-transporting the stick to all points on the path γ.
For any path γ there are locally directional derivatives of functions on M defined by tangent vectors to the curve. The vector over the map wγ = dγ(∂/∂t) is a directional derivative of functions on the manifold M along the path γ.
Our goal is to determine the equations satisfied by the vector field over the map uγ . Consider the parallel-transport F wγ uγ .12
δ
So a vector field uγ is parallel-transported to itself if and only
if uγ = F wγ uγ . Restricted to a path, the equation analogous to δ
equation (7.40) is

g(δ) = ui(t) − Aij(δ)uj(t − δ) eγi (f)(t), (7.66) ij
where the coefficient function ui is now a function on the real-line parameter manifold and where we have rewritten the basis as a basis over the map γ.13 Here g(δ) = 0 if uγ is parallel-transported into itself.
Taking the derivative and setting δ = 0 we find 
0 = Dui(t) + γπij(wγ)(t)uj(t) ij
But this implies that
eγi (f)(t).
(7.67)
(7.68)
γ πij (wγ )(t)uj (t),
0 = Dui(t) +
an ordinary differential equation in the coefficients of uγ.
j
 12The argument wγ makes sense because our parallel-transport operator never depended on the vector field tangent to the integral curve existing off of the curve. Because the connection is a form field (see equation 7.47), it does not depend on the value of its vector argument anywhere except at the point where it is being evaluated.
The argument uγ is more difficult. We must modify equation (7.37): F wγ uγ (f)(t) =  Ai (δ)uj (t − δ)eγ (f)(t).
δji i,j
13You may have noticed that t and t appear here. The real-line manifold point t has coordinate t.

#page(106)
 We can abstract these equations of parallel transport by inventing a covariant derivative over a map. We also generalize the time line to a source manifold N.
∇ γv u γ ( f ) ( n )  
= v(ui)(n) + γπij(dγ(v))(n)uj(n) eγi (f)(n), (7.69)
ij
wherethemapγ:N→M,visavectoronN,uγ isavectorover themapγ,fisafunctiononM,andnisapointinN. Indeed, if w is a vector field on M, f is a manifold function on M, and if dγ(v) = wγ then
∇γvuγ(f)(n) = ∇wu(f)(γ(n)). (7.70)
This is why we are justified in calling ∇γv a covariant derivative. Respecializing the source manifold to the real line, we can write
the equations governing the parallel transport of uγ as
∇γ∂/∂tuγ = 0. (7.71)
We obtain the set of differential equations (7.68) for the coordinates of uγ, the vector over the map γ, that is parallel-transported along the curve γ:
Dui(t) +
Expressing the Cartan forms in terms of the Christoffel coefficients
we obtain
Dui(t) + Γijk(γ(t))Dσk(t)uj(t) = 0 (7.73)
j
γ πij (dγ(∂/∂t))(t)uj (t) = 0. (7.72)
j,k
where σ = χ ◦γ ◦χ−1 are the coordinates of the path (χ MRM
χR are the coordinate functions for M and the real line).
On a Sphere
and
Let’s figure out what the equations of parallel transport of uγ, an arbitrary vector over the map γ, along an arbitrary path γ on a sphere are. We start by constructing the necessary manifold.

#page(107)
 (define sphere (make-manifold S^2 2 3)) (define S2-spherical
(coordinate-system-at ’spherical ’north-pole sphere)) (define S2-basis
(coordinate-system->basis S2-spherical))
We need the path γ, which we represent as a map from the real line to M, and w, the parallel-transported vector over the map:
(define gamma
(compose (point S2-spherical)
(up (literal-function ’alpha) (literal-function ’beta))
           (chart R1-rect)))
where alpha is the colatitude and beta is the longitude.
We also need an arbitrary vector field u gamma over the map gamma. To make this we multiply the structure of literal compo-
nent functions by the vector basis structure.
(define basis-over-gamma (basis->basis-over-map gamma S2-basis))
(define u gamma
(* (up (compose (literal-function ’u^0)
(chart R1-rect)) (compose (literal-function ’u^1)
(chart R1-rect))) (basis->vector-basis basis-over-gamma)))
We specify a connection by giving the Christoffel coefficients.14
(define S2-Christoffel
  (make-Christoffel
(let ((zero (lambda (point) 0))) (down (down (up zero zero)
(up zero (/ 1 (tan theta)))) (down (up zero (/ 1 (tan theta)))
  (up (- (* (sin theta) (cos theta))) zero)))) (define sphere-Cartan (Christoffel->Cartan S2-Christoffel))
14We will show later that these Christoffel coefficients are a natural choice for the sphere.
S2-basis))
 
#page(108)
 Finally, we compute the residual of the equation (7.71) that governs parallel transport for this situation:15
(define-coordinates t R1-rect)
(s:map/r
 (lambda (omega)
((omega
(((covariant-derivative sphere-Cartan gamma)
d/dt)
u gamma))
((point R1-rect) ’tau))) (basis->1form-basis basis-over-gamma))
(up (+ (* -1
          (sin (alpha tau))
(cos (alpha tau)) ((D beta) tau) (uˆ1 tau))
((D uˆ0) tau))
(/ (+ (* (uˆ0 tau) (cos (alpha tau)) ((D beta) tau))
(* ((D alpha) tau) (cos (alpha tau)) (uˆ1 tau))
(* ((D uˆ1) tau) (sin (alpha tau)))) (sin (alpha tau))))
Thus the equations governing the evolution of the components of the transported vector are:
 Du0(τ) = sin(α(τ)) cos(α(τ))Dβ(τ)u1(τ),
Du1(τ) = −cos(α(τ)) Dβ(τ)u0(τ) + Dα(τ)u1(τ) . (7.74)
 sin(α(τ ))
These equations describe the transport on a sphere, but more
generally they look like
Du(τ) = f(σ(τ),Dσ(τ))u(τ), (7.75)
where σ is the tuple of the coordinates of the path on the manifold and u is the tuple of the components of the vector. The equation is linear in u and is driven by the path σ, as in a variational equation.
15If we give covariant-derivative an extra argument, in addition to the Cartan form, the covariant derivative treats the extra argument as a map and transforms the Cartan form to work over the map.
 
#page(109)
 We now set this up for numerical integration. Let s(t) = (t,u(t)) be a state tuple, combining the time and the coordinates of uγ at that time. Then we define g:
g(s(t)) = Ds(t) = (1, Du(t)), (7.76) where Du(t) is the tuple of right-hand sides of equation (7.72).
On a Great Circle
We illustrate parallel transport in a case where we should know the answer: we carry a vector along a great circle of a sphere. Given a path and Cartan forms for the manifold we can produce a state derivative suitable for numerical integration. Such a state derivative takes a state and produces the derivative of the state.
(define (g gamma Cartan) (let ((omega
((Cartan->forms
(Cartan->Cartan-over-map Cartan gamma))
((differential gamma) d/dt)))) (define ((the-state-derivative) state)
(let ((t ((point R1-rect) (ref state 0))) (u (ref state 1)))
(up 1 (* -1 (omega t) u)))) the-state-derivative))
The path on the sphere will be the target of a map from the real line. We choose one that starts at the origin of longitudes on the equator and follows the great circle that makes a given tilt angle with the equator.
(define ((transform tilt) coords) (let ((colat (ref coords 0)) (long (ref coords 1)))
(let ((x (* (sin colat) (cos (y (* (sin colat) (sin
(z (cos colat)))
(let ((vp ((rotate-x tilt) (let ((colatp (acos (ref
long)))
long)))
(up x y z))))
vp 2)))
(longp (atan (ref vp 1) (ref vp 0))))
(up colatp longp))))))

#page(110)
Directional Derivatives
 (define (tilted-path tilt) (define (coords t)
((transform tilt) (up :pi/2 t))) (compose (point S2-spherical)
           coords
           (chart R1-rect)))
A southward pointing vector, with components (up 1 0), is transformed to an initial vector for the tilted path by multiplying by the derivative of the tilt transform at the initial point. We then parallel transport this vector by numerically integrating the differential equations. In this example we tilt by 1 radian, and we advance for π/2 radians. In this case we know the answer: by advancing by π/2 we walk around the circle a quarter of the way and at that point the transported vector points south:
((state-advancer (g (tilted-path 1) sphere-Cartan)) (up 0 (* ((D (transform 1)) (up :pi/2 0)) (up 1 0))) pi/2)
(up 1.5707963267948957
    (up .9999999999997626 7.376378522558262e-13))
However, if we transport by 1 radian rather than π/2, the numbers are not so pleasant, and the transported vector no longer points south:
((state-advancer (g (tilted-path 1) sphere-Cartan)) (up 0 (* ((D (transform 1)) (up :pi/2 0)) (up 1 0))) 1)
(up 1. (up .7651502649360408 .9117920272006472))
But the transported vector can be obtained by tilting the original southward-pointing vector after parallel-transporting along the equator:16
(* ((D (transform 1)) (up :pi/2 1)) (up 1 0))
(up .7651502649370375 .9117920272004736)
16A southward-pointing vector remains southward-pointing when it is paralleltransported along the equator. To do this we do not have to integrate the differential equations, because we know the answer.
 
#page(111)
 7.4 Geodesic Motion
In geodesic motion the velocity vector is parallel-transported by itself. Recall (equation 6.9) that the velocity is the differential of the vector ∂/∂t over the map γ. The equation of geodesic motion is17
∇γ∂/∂tdγ(∂/∂t) = 0. (7.78) In coordinates, this is
D2σi(t) +
where σ(t) is the coordinate path corresponding to the manifold path γ.
For example, let’s consider geodesic motion on the surface of a unit sphere. We let gamma be a map from the real line to the sphere, with colatitude alpha and longitude beta, as before. The geodesic equation is:
jk
Γijk(γ(t))Dσj(t)Dσk(t) = 0, (7.79)
(show-expression
(((((covariant-derivative sphere-Cartan gamma)
d/dt)
((differential gamma) d/dt))
(chart S2-spherical)) ((point R1-rect) ’t0)))
17The equation of a geodesic path is often said to be
∇vv = 0,
(7.77)
⎛ − cos (α (t0)) sin (α (t0)) (Dβ (t0))2 + D2α (t0) ⎞
⎜⎝ 2Dβ (t0) cos (α (t0)) Dα (t0) ⎟⎠ sin (α (t)) + D2β (t0)
      but this is nonsense. The geodesic equation is a constraint on the path, but
the path does not appear in this equation. Further, the velocity along a path is not a vector field, so it cannot appear in either argument to the covariant derivative.
What is true is that a vector field v all of whose integral curves are geodesics satisfies equation (7.77).

#page(112)
 The geodesic equation is the same as the Lagrange equation for free motion constrained to the surface of the unit sphere. The Lagrangian for motion on the sphere is the composition of the free-particle Lagrangian and the state transformation induced by the coordinate constraint:18
(define (Lfree s)
(* 1/2 (square (velocity s))))
(define (sphere->R3 s)
(let ((q (coordinate s)))
(let ((theta (ref q 0)) (phi (ref q 1))) (up (* (sin theta) (cos phi))
(* (sin theta) (sin phi)) (cos theta)))))
(define Lsphere
(compose Lfree (F->C sphere->R3)))
Then the Lagrange equations are:
(show-expression (((Lagrange-equations Lsphere)
(up (literal-function ’alpha) (literal-function ’beta)))
’t))
The Lagrange equations are true of the same paths as the geodesic equations. The second Lagrange equation is the second geodesic equation multiplied by (sin(α(t)))2, and the Lagrange equations are arranged in a down tuple, whereas the geodesic equations are arranged in an up tuple.19 The two systems are equivalent unless α(t) = 0, where the coordinate system is singular.
18The method of formulating a system with constraints by composing a free system with the state-space coordinate transformation that represents the constraints can be found in [19], Section 1.6.3. The procedure F->C takes a coordinate transformation and produces a corresponding transformation of Lagrangian state.
19The geodesic equations and the Lagrange equations are related by a contraction with the metric.
 −(Dβ(t))2 sin(α(t))cos(α(t))+D2α(t)  2Dα (t) Dβ (t) sin (α (t)) cos (α (t)) + D2β (t) (sin (α (t)))2
     
#page(113)
 Exercise 7.1: Hamiltonian Evolution
We have just seen that the Lagrange equations for the motion of a free particle constrained to the surface of a sphere determine the geodesics on the sphere. We can investigate this phenomenon in the Hamiltonian formulation. The Hamiltonian is obtained from the Lagrangian by a Legendre transformation:
(define Hsphere (Lagrangian->Hamiltonian Lsphere))
We can get the coordinate representation of the Hamiltonian vector field as follows:
((phase-space-derivative Hsphere)
(up ’t (up ’theta ’phi) (down ’p theta ’p phi)))
(up 1
(up p theta
(/ p phi (expt (sin theta) 2))) (down (/ (* (expt p phi 2) (cos theta)) (expt (sin theta) 3))
0))
The state space for Hamiltonian evolution has five dimensions: time, two dimensions of position on the sphere, and two dimensions of momentum:
(define state-space (make-manifold R^n 5))
(define states
(coordinate-system-at ’rectangular ’origin state-space))
(define-coordinates
(up t (up theta phi) (down p theta p phi)) states)
So now we have coordinate functions and the coordinate-basis vector fields and coordinate-basis one-form fields.
a. Define the Hamiltonian vector field as a linear combination of these fields.
b. Obtain the first few terms of the Taylor series for the evolution of the coordinates (θ, φ) by exponentiating the Lie derivative of the Hamiltonian vector field.
Exercise 7.2: Lie Derivative and Covariant Derivative
How are the Lie derivative and the covariant derivative related?
a. Prove that for every vector field there exists a connection such that the covariant derivative for that connection and the given vector field is equivalent to the Lie derivative with respect to that vector field.
b. Show that there is no connection that for every vector field makes the Lie derivative the same as the covariant derivative with the chosen connection.
 