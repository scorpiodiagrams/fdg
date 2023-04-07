!!Polyglot
## Basis Fields
A vector field may be written as a linear combination of basis vector fields. If n is the dimension, then any set of n linearly independent vector fields may be used as a basis. The coordinate basis X is an example of a basis.1 We will see later that not every basis is a coordinate basis: in order to be a coordinate basis, there must be a coordinate system such that each basis element is the directional derivative operator in a corresponding coordinate direction.
Let e be a tuple of basis vector fields, such as the coordinate basis X. The general vector field v applied to an arbitrary manifold function f can be expressed as a linear combination
ei(f)(m) bi(m), (4.1)
where b is a tuple-valued coefficient function on the manifold. When expressed in a coordinate basis, the coefficients that specify the direction of the vector are naturally expressed as functions bi of the coordinates of the manifold point. Here, the coefficient function b is more naturally expressed as a tuple-valued function on the manifold. If b is the coefficient function expressed as a function of coordinates, then b = b ◦ χ is the coefficient function as a function on the manifold.
The coordinate-basis forms have a simple definition in terms of the coordinate-basis vectors and the coordinates (equation 3.40). With this choice, the dual property, equation (3.41), holds without further fuss. More generally, we can define a basis of one-forms  ̃e that is dual to e in that the property
 ̃ei(ej)(m) = δji (4.2) is satisfied, analogous to property (3.41). Figure 4.1 illustrates
the duality of basis fields.
1We cannot say if the basis vectors are orthogonal or normalized until we introduce a metric.
v(f)(m) = e(f)(m) b(m) =
i
 
#page(42)
            e1
      e0
Figure 4.1 Let arrows e0 and e1 depict the vectors of a basis vector field at a particular point. Then the foliations shown by the parallel lines depict the dual basis one-form fields at that point. The dotted lines represent the field  ̃e0 and the dashed lines represent the field  ̃e1. The spacings of the lines are 1/3 unit. That the vectors pierce three of the lines representing their duals and do not pierce any of the lines representing the other basis elements is one way to see the relationship  ̃e i ( e j ) ( m ) = δ ji .
To solve for the dual basis  ̃e given the basis e, we express the basis vectors e in terms of a coordinate basis2
k
X k ( f ) c kj , ( 4 . 3 ) dil Xl(v), (4.4)
e j ( f ) =
and the dual one-forms  ̃e in terms of the dual coordinate one-forms
 ̃ei(v) =
2We write the vector components on the right and the tuple of basis vectors on the left because if we think of the basis vectors as organized as a row and the components as organized as a column then the formula is just a matrix multiplication.
l
 
Chapter 4
Basis Fields
#page(43)
 then
 ̃e i ( e j ) =
= = = =
d il X l ( e j ) d il e j ( χ l )
 ̃ei(ej)(m) = δji =
dik(m)ckj (m).
l
l
d il lk
d il δ kl c kj d ik c kj .
X k ( χ l ) c kj
kl
k
( 4 . 5 )
(4.6)
Applying this at m we get
k
So the d coefficients can be determined from the c coefficents (essentially by matrix inversion).
A set of vector fields {ei} may be linearly independent in the sense that a weighted sum of them may not be identically zero over a region, yet it may not be a basis in that region. The problem is that there may be some places in the region where the vectors are not independent. For example, two of the vectors may be parallel at a point but not parallel elsewhere in the region. At such a point m the determinant of the matrix c(m) is zero. So at these points we cannot define the dual basis forms.3
The dual form fields can be used to determine the coefficients b of a vector field v relative to a basis e, by applying the dual basis form fields  ̃e to the vector field. Let
ei(f) bi. (4.7)
 ̃ej(v) = bj. (4.8)
3This is why the set of vector fields and the set of one-form fields are modules rather than vector spaces.
v(f) = Then
i
 
#page(44)
Chapter 4 Basis Fields
 Define two general vector fields:
```Scheme
(define e0
(+ (* (literal-manifold-function ’e0x R2-rect) d/dx)
(* (literal-manifold-function ’e0y R2-rect) d/dy)))
(define e1
(+ (* (literal-manifold-function ’e1x R2-rect) d/dx)
(* (literal-manifold-function ’e1y R2-rect) d/dy)))
```
We use these as a vector basis and compute the dual:
```Scheme
(define e-vector-basis (down e0 e1)) (define e-dual-basis
(vector-basis->dual e-vector-basis R2-polar))
```
The procedure vector-basis->dual requires an auxiliary coordinate system (here R2-polar) to get the ckj coefficient functions from which we compute the dik coefficient functions. However, the final result is independent of this coordinate system. Then we can verify that the bases e and  ̃e satisfy the dual relationship (equation 3.41) by applying the dual basis to the vector basis:
```Scheme
((e-dual-basis e-vector-basis) R2-rect-point)
(up (down 1 0) (down 0 1))
```
Note that the dual basis was computed relative to the polar coordinate system: the resulting objects are independent of the coordinates in which they were expressed!
Or we can make a general vector field with this basis and then pick out the coefficients by applying the dual basis:
```Scheme
(define v
(* (up (literal-manifold-function ’b^0 R2-rect)
(literal-manifold-function ’b^1 R2-rect)) e-vector-basis))
((e-dual-basis v) R2-rect-point)
(up (bˆ0 (up x0 y0)) (bˆ1 (up x0 y0)))
```
4.1 Change of Basis
Suppose that we have a vector field v expressed in terms of one basis e and we want to reexpress it in terms of another basis e′. We have

#page(45)
 v(f) = ei(f)bi = e′j(f)b′j. (4.9) ij
The coefficients b′ can be obtained from v by applying the dual basis
b′j =  ̃e′j(v) =
Let
Jji = ̃e′j(ei), then
b′j =
 ̃e′j(ei)bi.
(4.10)
(4.11)
(4.12)
(4.13)
and ei(f) =
i
j
Jjibi, e′j(f)Jji.
i
The Jacobian J is a structure of manifold functions. Using tuple arithmetic, we can write
b′ = Jb
and
e(f) = e′(f)J.
We can write
(define (Jacobian to-basis from-basis) (s:map/r (basis->1form-basis to-basis)
(basis->vector-basis from-basis)))
These are the rectangular components of a vector field:
```Scheme
(define b-rect ((coordinate-system->1form-basis R2-rect)
(literal-vector-field ’b R2-rect)))
The polar components are:
```Scheme
(4.14)
(4.15)

#page(46)
Basis Fields
```Scheme
(define b-polar
(* (Jacobian (coordinate-system->basis R2-polar)
(coordinate-system->basis R2-rect))
b-rect))
(b-polar ((point R2-rect) (up ’x0 ’y0)))
(up
(/ (+ (* x0 (bˆ0 (up x0 y0))) (* y0 (bˆ1 (up x0 y0))))
    (sqrt (+ (expt x0 2) (expt y0 2))))
(/ (+ (* x0 (bˆ1 (up x0 y0))) (* -1 y0 (bˆ0 (up x0 y0))))
    (+ (expt x0 2) (expt y0 2))))
```    
We can also get the polar components directly:
```Scheme
(((coordinate-system->1form-basis R2-polar) (literal-vector-field ’b R2-rect))
((point R2-rect) (up ’x0 ’y0)))
(up
(/ (+ (* x0 (bˆ0 (up x0 y0))) (* y0 (bˆ1 (up x0 y0))))
    (sqrt (+ (expt x0 2) (expt y0 2))))
(/ (+ (* x0 (bˆ1 (up x0 y0))) (* -1 y0 (bˆ0 (up x0 y0))))
    (+ (expt x0 2) (expt y0 2))))
```    
We see that they are the same.
If K is the Jacobian that relates the basis vectors in the other
direction
e′(f) = e(f)K (4.16) then
KJ = I = JK (4.17)
where I is a manifold function that returns the multiplicative identity.
The dual basis transforms oppositely. Let
ω = ai ̃ei = a′i ̃e′i. (4.18) ii

4.2 Rotation Basis
47
 The coefficients are4
ai = ω(ei) = a′j ̃e′j(ei) =
jj
a′jJji
(4.19)
(4.20)
(4.21)
or, in tuple arithmetic,
a = a′J.
Because of equation (4.18) we can deduce  ̃e = K ̃e′.
4.2 Rotation Basis
One interesting basis for rotations in 3-dimensional space is not a coordinate basis.
Rotations are the actions of the special orthogonal group SO(3), which is a 3-dimensional manifold. The elements of this group may be represented by the set of 3 × 3 orthogonal matrices with determinant +1.
We can use a coordinate patch on this manifold with Euler angle coordinates: each element has three coordinates, θ, φ, ψ. A manifold point may be represented by a rotation matrix. The rotation matrix for Euler angles is a product of three simple rotations: M(θ,φ,ψ) = Rz(φ)Rx(θ)Rz(ψ), where Rx and Rz are functions that take an angle and produce the matrices representing rotations about the x and z axes, respectively. We can visualize θ as the colatitude of the pole from the zˆ-axis, φ as the longitude, and ψ as the rotation around the pole.
Given a rotation specified by Euler angles, how do we change the Euler angle to correspond to an incremental rotation of size ε about the xˆ-axis? The direction (a, b, c) is constrained by the equation
Rx(ε)M(θ,φ,ψ) = M(θ + aε,φ + bε,ψ + cε). (4.22)
4We see from equations (4.15) and (4.16) that J and K are inverses. We can obtain their coefficients by: Jji =  ̃e′j(ei) and Kji =  ̃ej(e′i).
 
#page(48)
 Linear equations for (a, b, c) can be found by taking the derivative of this equation with respect to ε. We find
0 = c cos θ + b,
0 = asinφ−ccosφsinθ, 1 = c sin φ sin θ + a cos φ,
with the solution
a = cosφ,
b= −sinφcosθ,
ex = a ∂ + b ∂ + c ∂ ∂θ ∂φ ∂ψ
= cosφ∂ −sinφcosθ ∂ +sinφ ∂ . (4.29) ∂θ sin θ ∂φ sin θ ∂ψ
Similarly, vector fields for the incremental y and z rotations are
 sin θ c= sinφ.
(4.23) (4.24) (4.25)
(4.26) (4.27)
(4.28) Therefore, we can write the basis vector field that takes directional
 sin θ
derivatives in the direction of incremental x rotations as
        ey = cos φ cos θ ∂ + sin φ ∂ − cos φ ∂ , sinθ ∂φ ∂θ sinθ ∂ψ
ez = ∂ . ∂φ
4.3 Commutators
The commutator of two vector fields is defined as [v, w](f) = v(w(f)) − w(v(f)).
(4.30) (4.31)
(4.32)
      In the special case that the two vector fields are coordinate basis fields, the commutator is zero:

#page(49)
 [Xi,Xj](f) = Xi(Xj(f)) − Xj(Xi(f))
= ∂i∂j(f ◦ χ−1) ◦ χ − ∂j∂i(f ◦ χ−1) ◦ χ
= 0, (4.33)
because the individual partial derivatives commute. The vanishing commutator is telling us that we get to the same manifold point by integrating from a point along first one basis vector field and then another as from integrating in the other order. If the commutator is zero we can use the integral curves of the basis vector fields to form a coordinate mesh.
More generally, the commutator of two vector fields is a vector field. Let v be a vector field with coefficient function c = c ◦ χ, and u be a vector field with coefficient function b = b ◦ χ, both with respect to the coordinate basis X. Then
u(v(f)) − v(u(f))
u( Xi(f)ci) − v( Xj(f)bj)
ij
Xj( Xi(f)ci)bj − Xi( Xj(f)bj)ci ji ij
[Xj,Xi](f)cibj
Xi(f) (Xj(ci)bj − Xj(bi)cj)
ai =
= u(ci) − v(bi). (4.35)
We used the fact, shown above, that the commutator of two coordinate basis fields is zero.
[u, v](f) = =
= =
= where the
ij
+
(4.34) coefficient function a of the commutator vector field is
j
i
ij Xi (f )ai ,
Xj(ci)bj − Xj(bi)cj

#page(50)
 We can check this formula for the commutator for the general vector fields e0 and e1 in polar coordinates:
```Scheme
(let* ((polar-basis (coordinate-system->basis R2-polar)) (polar-vector-basis (basis->vector-basis polar-basis)) (polar-dual-basis (basis->1form-basis polar-basis))
(f (literal-manifold-function ’f-rect R2-rect)))
((- ((commutator e0 e1) f)
(* (- (e0 (polar-dual-basis e1))
(e1 (polar-dual-basis e0))) (polar-vector-basis f)))
   R2-rect-point))
0
```
Let e be a tuple of basis vector fields. The commutator of two basis fields can be expressed in terms of the basis vector fields:
basis vector fields. The coefficients are
dkij =  ̃ek([ei,ej]). (4.37)
The commutator [u, v] with respect to a non-coordinate basis ei is

[u,v](f) = ek(f) u(ck) − v(bk) + cibjdkji . (4.38) k ij
Define the vector fields Jx, Jy, and Jz that generate rotations about the three rectangular axes in three dimensions:5
```Scheme
(define Jz (- (* x d/dy) (* y d/dx))) (define Jx (- (* y d/dz) (* z d/dy))) (define Jy (- (* z d/dx) (* x d/dz)))
```
5 Using
```Scheme
(define R3-rect (coordinate-system-at ’rectangular ’origin R3)) (define-coordinates (up x y z) R3-rect)
(define R3-rect-point ((point R3-rect) (up ’x0 ’y0 ’z0))) (define g (literal-manifold-function ’g-rect R3-rect))
k
```
dkijek(f), (4.36)
[ei,ej](f) =
where dkij are functions of m, called the structure constants for the
 
#page(51)
```Scheme
(((+ (commutator Jx Jy) Jz) g) R3-rect-point)
0
(((+ (commutator Jy Jz) Jx) g) R3-rect-point)
0
(((+ (commutator Jz Jx) Jy) g) R3-rect-point)
0
```
We see that
[Jx,Jy] = −Jz
[Jy,Jz] = −Jx
[Jz,Jx] = −Jy. (4.39)
We can also compute the commutators for the basis vector fields ex, ey, and ez in the SO(3) manifold (see equations 4.29–4.31) that correspond to rotations about the x, y, and z axes, respectively:6
```Scheme
(((+ (commutator e x e y) e z) f) SO3-point)
0
(((+ (commutator e y e z) e x) f) SO3-point)
0
(((+ (commutator e z e x) e y) f) SO3-point)
0
```
You can tell if a set of basis vector fields is a coordinate basis by calculating the commutators. If they are nonzero, then the basis is not a coordinate basis. If they are zero then the basis vector fields can be integrated to give the coordinate system.
Recall equation (3.31) (etvf)(m) = (f ◦ φvt )(m). Iterating this equation, we find (eswetvf)(m) = (f ◦ φvt ◦ φws )(m).
6 Using
(4.40)
(4.41)
 (define Euler-angles (coordinate-system-at ’Euler ’Euler-patch SO3)) (define Euler-angles-chi-inverse (point Euler-angles)) (define-coordinates (up theta phi psi) Euler-angles)
(define SO3-point ((point Euler-angles) (up ’theta ’phi ’psi))) (define f (literal-manifold-function ’f-Euler Euler-angles))

#page(52)
 Notice that the evolution under w occurs before the evolution under v.
To illustrate the meaning of the commutator, consider the evolution around a small loop with sides made from the integral curves of two vector fields v and w. We will first follow v, then w, then −v, and then −w:
(eεv eεw e−εv e−εw f )(m). (4.42) To second order in ε the result is7
(eε2[v,w]f)(m). (4.43)
This result is illustrated in figure 4.2.
Take a point 0 in M as the origin. Then, presuming [ei,ej] = 0,
the coordinates x of the point m in the coordinate system corresponding to the e basis satisfy8
m = φxe(0) = χ−1(x), (4.44) 1
where χ is the coordinate function being defined. Because the elements of e commute, we can translate separately along the integral curves in any order and reach the same point; the terms in the exponential can be factored into separate exponentials if needed.
Exercise 4.1: Alternate Angles
Note that the Euler angles are singular at θ = 0 (where φ and ψ become degenerate), so the representations of ex, ey, and ez (defined in equa-
7 For non-commuting operators A and B, eA eB e−A e−B
 A2  B2  = 1+A+ 2 +··· 1+B+ 2 +···
 A2  B2  × 1−A+ 2 +··· 1−B+ 2 +···
= 1 + [A, B] + · · · ,
to second order in A and B. All higher-order terms can be written in terms of higher-order commutators of A and B. An example of a higher-order commutator is [A, [A, B]].
8Here x is an up-tuple structure of components, and e is down-tuple structure of basis vectors. The product of the two contracts to make a scaled vector, along which we translate by one unit.
     
4.3 Commutators
53
 m ε2[v,w] εv
    εw
−εw
−εv
 The commutator of two vector fields computes the residual of a small loop following their integral curves.
tions 4.29–4.31) have problems there. An alternate coordinate system avoids this problem, while introducing a similar problem elsewhere in the manifold.
Consider the “alternate angles” (θa, φa, ψa) which define a rotation matrix via M (θa, φa, ψa) = Rz (φa) Rx (θa) Ry (ψa).
a. Where does the singularity appear in these alternate coordinates? Do you think you could define a coordinate system for rotations that has no singularities?
b. What do the ex, ey, and ez basis vector fields look like in this coordinate system?
Exercise 4.2: General Commutators
Verify equation (4.38).
Exercise 4.3: SO(3) Basis and Angular Momentum Basis
How are Jx, Jy, and Jz related to ex, ey, and ez in equations (4.29–4.31)?