!!Polyglot
## Vector Fields and One-Form Fields
We want a way to think about how a function varies on a manifold. Suppose we have some complex linkage, such as a multiple pendulum. The potential energy is an important function on the multi-dimensional configuration manifold of the linkage. To understand the dynamics of the linkage we need to know how the potential energy changes as the configuration changes. The change in potential energy for a step of a certain size in a particular direction in the configuration space is a real physical quantity; it does not depend on how we measure the direction or the step size. What exactly this means is to be determined: What is a step size? What is a direction? We cannot subtract two configurations to determine the distance between them. It is our job here to make sense of this idea.
So we would like something like a derivative, but there are problems. Since we cannot subtract two manifold points, we cannot take the derivative of a manifold function in the way described in elementary calculus. But we can take the derivative of a coordinate representation of a manifold function, because it takes real-number coordinates as its arguments. This is a start, but it is not independent of coordinate system. Let’s see what we can build out of this.
3.1 Vector Fields
In multiple dimensions the derivative of a function is the multiplier for the best linear approximation of the function at each argument point:1
f(x + Δx) ≈ f(x) + (Df(x))Δx (3.1) The derivative Df(x) is independent of Δx. Although the deriva-
tive depends on the coordinates, the product (Df(x))Δx is in-
1In multiple dimensions the derivative Df(x) is a down tuple structure of the partial derivatives and the increment Δx is an up tuple structure, so the indicated product is to be interpreted as a contraction. (See equation B.8.)
 
#page(22)
 variant under change of coordinates in the following sense. Let φ = χ ◦ χ′−1 be a coordinate transformation, and x = φ(y). Then Δx = Dφ(y)Δy is the linear approximation to the change in x when y changes by Δy. If f and g are the representations of a manifold function in the two coordinate systems, g(y) = f(φ(y)) = f(x), then the linear approximations to the increments in f and g are equal:
Dg(y)Δy = Df (φ(y)) (Dφ(y)Δy) = Df (x)Δx.
The invariant product (Df(x))Δx is the directional derivative of f at x with respect to the vector specified by the tuple of components Δx in the coordinate system. We can generalize this idea to allow the vector at each point to depend on the point, making a vector field. Let b be a function of coordinates. We then have a directional derivative of f at each point x, determined by b
Db(f)(x) = (Df(x))b(x). (3.2)
Now we bring this back to the manifold and develop a useful generalization of the idea of directional derivative for functions on a manifold, rather than functions on Rn. A vector field on a manifold is an assignment of a vector to each point on the manifold. In elementary geometry, a vector is an arrow anchored at a point on the manifold with a magnitude and a direction. In differential geometry, a vector is an operator that takes directional derivatives of manifold functions at its anchor point. The direction and magnitude of the vector are the direction and scale factor of the directional derivative.
Let m be a point on a manifold, v be a vector field on the manifold, and f be a real-valued function on the manifold. Then v(f) is the directional derivative of the function f and v(f)(m) is the directional derivative of the function f at the point m. The vector field is an operator that takes a real-valued manifold function and a manifold point and produces a number. The order of arguments is chosen to make v(f) be a new manifold function that can be manipulated further. Directional derivative operators, unlike ordinary derivative operators, produce a result of the same type as their argument. Note that there is no mention here of any coordinate system. The vector field specifies a direction and magnitude at each manifold point that is independent of how it is described using any coordinate system.

#page(23)
 A useful way to characterize a vector field in a particular coordinate system is by applying it to the coordinate functions. The resulting functions biχ,v are called the coordinate component functions or coefficient functions of the vector field; they measure how quickly the coordinate functions change in the direction of the vector field, scaled by the magnitude of the vector field:
b iχ , v = v χ i ◦ χ − 1 . ( 3 . 3 )
Note that we have chosen the coordinate components to be functions of the coordinate tuple, not of a manifold point.
A vector with coordinate components bχ,v applies to a manifold function f via
v(f)(m) = ((D(f ◦ χ−1) bχ,v) ◦ χ)(m)
= D(f ◦ χ−1)(χ(m)) bχ,v(χ(m))
(3.4) (3.5)
(3.6)
=
∂i(f ◦ χ−1)(χ(m)) biχ,v(χ(m)).
i
In equation (3.4), the quantity f◦χ−1 is the coordinate representation of the manifold function f. We take its derivative, and weight the components of the derivative with the coordinate components bχ,v of the vector field that specify its direction and magnitude. Since this product is a function of coordinates we use χ to extract the coordinates from the manifold point m. In equation (3.5), the composition of the product with the coordinate chart χ is replaced by function evaluation. In equation (3.6) the tuple multiplication is expressed explicitly as a sum of products of corresponding components. So the application of the vector is a linear combination of the partial derivatives of f in the coordinate directions weighted by the vector components. This computes the rate of change of f in the direction specified by the vector.
Equations (3.3) and (3.5) are consistent:
v(χ)(χ−1(x)) = D(χ ◦ χ−1)(x) bχ,v(x) = D(I)(x) bχ,v(x)
= bχ,v(x). (3.7)
The coefficient tuple bχ,v(x) is an up structure compatible for addition to the coordinates. Note that for any vector field v the coefficients bχ,v(x) are different for different coordinate functions χ.

#page(24)
 In the text that follows we will usually drop the subscripts on b, understanding that it is dependent on the coordinate system and the vector field.
We implement the definition of a vector field (3.4) as:
(define (components->vector-field components coordsys) (define (v f)
(compose (* (D (compose f (point coordsys))) components)
(chart coordsys))) (procedure->vector-field v))
The vector field is an operator, like derivative.2
Given a coordinate system and coefficient functions that map
coordinates to real values, we can make a vector field. For example, a general vector field can be defined by giving components relative to the coordinate system R2-rect by
(define v (components->vector-field
(up (literal-function ’b^0 R2->R) (literal-function ’b^1 R2->R))
R2-rect))
To make it convenient to define literal vector fields we provide a shorthand: (define v (literal-vector-field ’b R2-rect)) This makes a vector field with component functions named b^0 and b^1 and names the result v. When this vector field is applied to an arbitrary manifold function it gives the directional derivative of that manifold function in the direction specified by the components bˆ0 and bˆ1:
((v (literal-manifold-function ’f-rect R2-rect)) R2-rect-point)
(+ (* (((partial 0) f-rect) (up x0 y0)) (bˆ0 (up x0 y0))) (* (((partial 1) f-rect) (up x0 y0)) (bˆ1 (up x0 y0))))
This result is what we expect from equation (3.6).
We can recover the coordinate components of the vector field
by applying the vector field to the coordinate chart:
2An operator is just like a procedure except that multiplication is interpreted as composition. For example, the derivative procedure is made into an operator D so that we can say (expt D 2) and expect it to compute the second derivative. The procedure procedure->vector-field makes a vector-field operator.
 
#page(25)
 ((v (chart R2-rect)) R2-rect-point)
(up (bˆ0 (up x y)) (bˆ1 (up x y)))
Coordinate Representation
The vector field v has a coordinate representation v:
v(f)(m) = D(f ◦ χ−1)(χ(m)) b(χ(m)) = Df(x) b(x)
= v(f)(x), (3.8)
with the definitions f = f ◦ χ−1 and x = χ(m). The function b is the coefficient function for the vector field v. It provides a scale factor for the component in each coordinate direction. However, v is the coordinate representation of the vector field v in that it takes directional derivatives of coordinate representations of manifold functions.
Given a vector field v and a coordinate system coordsys we can construct the coordinate representation of the vector field.3
(define (coordinatize v coordsys) (define ((coordinatized-v f) x)
(let ((b (compose (v (chart coordsys)) (point coordsys))))
(* ((D f) x) (b x))))) (make-operator coordinatized-v))
We can apply a coordinatized vector field to a function of coordinates to get the same answer as before.
(((coordinatize v R2-rect) (literal-function ’f-rect R2->R)) (up ’x0 ’y0))
(+ (* (((partial 0) f-rect) (up x0 y0)) (bˆ0 (up x0 y0))) (* (((partial 1) f-rect) (up x0 y0)) (bˆ1 (up x0 y0))))
Vector Field Properties
The vector fields on a manifold form a vector space over the field of real numbers and a module over the ring of real-valued manifold functions. A module is like a vector space except that there is no multiplicative inverse operation on the scalars of a module. Manifold functions that are not the zero function do not necessarily
3The make-operator procedure takes a procedure and returns an operator.
 
#page(26)
 have multiplicative inverses, because they can have isolated zeros. So the manifold functions form a ring, not a field, and vector fields must be a module over the ring of manifold functions rather than a vector space.
Vector fields have the following properties. Let u and v be vector fields and let α be a real-valued manifold function. Then
(u + v)(f) = u(f) + v(f) (3.9) (αu)(f) = α(u(f)). (3.10)
Vector fields are linear operators. Assume f and g are functions on the manifold, a and b are real constants.4 The constants a and b are not manifold functions, because vector fields take derivatives. See equation (3.13).
v(af + bg)(m) = av(f)(m) + bv(g)(m)
v(af)(m) = av(f)(m)
Vector fields satisfy the product rule (Leibniz rule).
v(fg)(m) = v(f)(m) g(m) + f(m) v(g)(m)
(3.11) (3.12)
(3.13)
Vector fields satisfy the chain rule. Let F be a function on the
range of f.
v(F ◦ f)(m) = DF (f(m)) v(f)(m) (3.14)
3.2 Coordinate-Basis Vector Fields
For an n-dimensional manifold any set of n linearly independent vector fields5 form a basis in that any vector field can be expressed as a linear combination of the basis fields with manifold-function
4If f has structured output then v(f) is the structure resulting from v being applied to each component of f.
5 A set of vector fields, {vi}, is linearly independent with respect to manifold functions if we cannot find nonzero manifold functions, {ai}, such that

aivi(f) = 0(f), i
where 0 is the vector field such that 0(f)(m) = 0 for all f and m.
 
#page(27)
 coefficients. Given a coordinate system we can construct a basis as follows: we choose the component tuple bi(x) (see equation 3.5) to be the ith unit tuple ui(x)—an up tuple with one in the ith position and zeros in all other positions—selecting the partial derivative in that direction. Here ui is a constant function. Like b, it formally takes coordinates of a point as an argument, but it ignores them. We then define the basis vector field Xi by
Xi(f)(m) = D(f ◦ χ−1)(χ(m)) ui(χ(m)) = ∂i(f ◦ χ−1)(χ(m)).
In terms of Xi the vector field of equation (3.6) is
(3.15)
(3.16)
(3.17)
(3.18)
Xi(f)(m) bi(χ(m)).
We can also write
v(f)(m) = X(f)(m) b(χ(m)),
letting the tuple algebra do its job.
The basis vector field is often written
∂ =Xi, ∂xi
v(f)(m) =
i
 to call to mind that it is an operator that computes the directional derivative in the ith coordinate direction.
In addition to making the coordinate functions, the procedure define-coordinates also makes the traditional named basis vectors. Using these we can examine the application of a rectangular basis vector to a polar coordinate function:
(define-coordinates (up x y) R2-rect) (define-coordinates (up r theta) R2-polar)
((d/dx (square r)) R2-rect-point)
(* 2 x0)
More general functions and vectors can be made as combinations of these simple pieces:
(((+ d/dx (* 2 d/dy)) (+ (square r) (* 3 x))) R2-rect-point)
(+ 3 (* 2 x0) (* 4 y0))

#page(28)
 Coordinate Transformations
Consider a coordinate change from the chart χ to the chart χ′.
X(f)(m) = D(f ◦ χ−1)(χ(m))
= D(f ◦ (χ′)−1 ◦ χ′ ◦ χ−1)(χ(m))
= D(f ◦ (χ′)−1)(χ′(m))(D(χ′ ◦ χ−1))(χ(m))
= X′(f)(m)(D(χ′ ◦ χ−1))(χ(m)). (3.19)
This is the rule for the transformation of basis vector fields. The second factor can be recognized as “∂x′/∂x,” the Jacobian.6
The vector field does not depend on coordinates. So, from equation (3.17), we have
v(f)(m) = X(f)(m) b(χ(m)) = X′(f)(m) b′(χ′(m)). (3.20)
Using equation (3.19) with x = χ(m) and x′ = χ′(m), we deduce
D(χ′ ◦ χ−1)(x) b(x) = b′(x′). (3.21)
Because χ′ ◦ χ−1 is the inverse function of χ ◦ (χ′)−1, their derivatives are multiplicative inverses,
D(χ′ ◦ χ−1)(x) = (D(χ ◦ (χ′)−1)(x′))−1, (3.22) and so
b(x) = D(χ ◦ (χ′)−1)(x′) b′(x′), (3.23)
as expected.7
It is traditional to express this rule by saying that the basis
elements transform covariantly and the coefficients of a vector in 6This notation helps one remember the transformation rule:
 ∂f  ∂f ∂x′j
∂x′j ∂xi ,
which is the relation in the usual Leibniz notation. As Spivak pointed out in Calculus on Manifolds, p.45, f means something different on each side of the equation.
7For coordinate paths q and q′ related by q(t) = (χ◦(χ′)−1)(q′(t)) the velocities are related by Dq(t) = D(χ ◦ (χ′)−1)(q′(t))Dq′(t). Abstracting off paths, we get v = D(χ ◦ (χ′)−1)(x′)v′.
∂xi =
j
   
3.3 Integral Curves 29 terms of a basis transform contravariantly; their product is invari-
ant under the transformation.
3.3 Integral Curves
A vector field gives a direction and rate for every point on a manifold. We can start at any point and go in the direction specified by the vector field, tracing out a parametric curve on the manifold. This curve is an integral curve of the vector field.
More formally, let v be a vector field on the manifold M. An integral curve γmv : R → M of v is a parametric path on M satisfying
D(f ◦ γmv )(t) = v(f)(γmv (t)) = (v(f) ◦ γmv )(t) (3.24) γmv (0) = m, (3.25)
for arbitrary functions f on the manifold, with real values or structured real values. The rate of change of a function along an integral curve is the vector field applied to the function evaluated at the appropriate place along the curve. Often we will simply write γ, rather than γmv . Another useful variation is φvt (m) = γmv (t).
We can recover the differential equations satisfied by a coordinate representation of the integral curve by letting f = χ, the coordinate function, and letting σ = χ ◦ γ be the coordinate path corresponding to the curve γ. Then the derivative of the coordinate path σ is
Dσ(t) = D(χ ◦ γ)(t) = (v(χ) ◦ γ)(t)
= (v(χ) ◦ χ−1 ◦ χ ◦ γ)(t)
= (b ◦ σ)(t), (3.26)
where b = v(χ) ◦ χ−1 is the coefficient function for the vector field v for coordinates χ (see equation 3.7). So the coordinate path σ satisfies the differential equations
Dσ = b ◦ σ. (3.27)
Differential equations for the integral curve can be expressed only in a coordinate representation, because we cannot go from one point on the manifold to another by addition of an increment.
 
#page(30)
 However, we can do this by adding the coordinates to an increment of coordinates and then finding the corresponding point on the manifold.
Iterating the process described by equation (3.24) we can compute higher-order derivatives of functions along the integral curve:
D(f ◦ γ) = v(f) ◦ γ
D2(f ◦ γ) = D(v(f) ◦ γ) = v(v(f)) ◦ γ
...
Dn(f ◦ γ) = vn(f) ◦ γ (3.28)
Thus, the evolution of f ◦ γ can be written formally as a Taylor series in the parameter:
(f ◦ γ)(t)
=(f◦γ)(0)+tD(f◦γ)(0)+ 1t2D2(f◦γ)(0)+···
 = (etD(f ◦ γ))(0) = (etvf)(γ(0)).
(3.29)
(3.30)
(3.31)
(3.32)
Using φ rather than γ
(f ◦ γmv )(t) = (f ◦ φvt )(m),
so, when the series converges, (etvf)(m) = (f ◦ φvt )(m).
In particular, let f = χ, then
σ(t) = (χ ◦ γ)(t) = (etD(χ ◦ γ))(0) = (etvχ)(γ(0)),
a Taylor series representation of the solution to the differential equation (3.27).
For example, a vector field circular that generates a rotation about the origin is:8
8In this expression d/dx and d/dy are vector fields that take directional derivatives of manifold functions and evaluate them at manifold points; x and y are manifold functions. define-coordinates was used to create these operators and functions, see page 27.
Note that circular is an operator—a property inherited from d/dx and d/dy.
2
 
#page(31)
 (define circular (- (* x d/dy) (* y d/dx)))
We can exponentiate the circular vector field, to generate an evolution in a circle around the origin starting at (1, 0):
(series:for-each print-expression
(((exp (* ’t circular)) (chart R2-rect))
((point R2-rect) (up 1 0))) 6)
(up 1 0)
(up 0 t)
(up (* -1/2 (expt t 2)) 0)
(up 0 (* -1/6 (expt t 3)))
(up (* 1/24 (expt t 4)) 0)
(up 0 (* 1/120 (expt t 5)))
These are the first six terms of the series expansion of the coordinates of the position for parameter t.
We can define an evolution operator EΔt,v using equation (3.31)
(EΔt,vf)(m) = (eΔtvf)(m) = (f ◦ φvΔt)(m). (3.33)
We can approximate the evolution operator by summing the series up to a given order:
(define ((((evolution order) delta-t v) f) m) (series:sum
(((exp (* delta-t v)) f) m) order))
We can evolve circular from the initial point up to the parameter t, and accumulate the first six terms as follows:
((((evolution 6) ’delta-t circular) (chart R2-rect)) ((point R2-rect) (up 1 0)))
(up (+ (* -1/720 (expt delta-t 6))
       (* 1/24 (expt delta-t 4))
       (* -1/2 (expt delta-t 2))
       1)
    (+ (* 1/120 (expt delta-t 5))
       (* -1/6 (expt delta-t 3))
       delta-t))
Note that these are just the series for cos Δt and sin Δt, so the coordinate tuple of the evolved point is (cos Δt, sin Δt).

#page(32)
 For functions whose series expansions have finite radius of convergence, evolution can progress beyond the point at which the Taylor series converges because evolution is well defined whenever the integral curve is defined.
Exercise 3.1: State Derivatives
Newton’s equations for the motion of a particle in a plane, subject to a force that depends only on the position in the plane, are a system of second-order differential equations for the rectangular coordinates (X, Y ) of the particle:
D2X(t)=Ax(X(t),Y(t)) and D2Y(t)=Ay(X(t),Y(t)),
where A is the acceleration of the particle.
These are equivalent to a system of first-order equations for the coor-
dinate path σ = χ ◦ γ, where χ = (t,x,y,vx,vy) is a coordinate system on the manifold R5. Then our equations are:
D(t ◦ γ) = 1 D(x ◦ γ) = vx ◦ γ D(y ◦ γ) = vy ◦ γ
D(vx ◦γ)=Ax(x◦γ,y◦γ) D(vy ◦γ)=Ay(x◦γ,y◦γ)
Construct a vector field on R5 corresponding to this system of differential equations. Derive the first few terms in the series solution of this problem by exponentiation.
3.4 One-Form Fields
A vector field that gives a velocity for each point on a topographic map of the surface of the Earth can be applied to a function, such as one that gives the height for each point on the topographic map, or a map that gives the temperature for each point. The vector field then provides the rate of change of the height or temperature as one moves in the way described by the vector field. Alternatively, we can think of a topographic map, which gives the height at each point, as measuring a velocity field at each point. For example, we may be interested in the velocity of the wind or the trajectories of migrating birds. The topographic map gives the rate of change of height at each point for each velocity vector field. The rate of change of height can be thought of as the

#page(33)
 number of equally-spaced (in height) contours that are pierced by each velocity vector in the vector field.
Differential of a Function
For example, consider the differential 9 df of a manifold function f, defined as follows. If df is applied to a vector field v we obtain
df(v) = v(f), (3.34)
which is a function of a manifold point.
The differential of the height function on the topographic map is
a function that gives the rate of change of height at each point for a velocity vector field. This gives the same answer as the velocity vector field applied to the height function.
The differential of a function is linear in the vector fields. The differential is also a linear operator on functions: if f1 and f2 are manifold functions, and if c is a real constant, then
d(f1 +f2) = df1 +df2
and
d(cf) = cdf.
Note that c is not a manifold function.
One-Form Fields
A one-form field is a generalization of this idea; it is something that measures a vector field at each point.
One-form fields are linear functions of vector fields that produce real-valued functions on the manifold. A one-form field is linear in vector fields: if ω is a one-form field, v and w are vector fields, and c is a manifold function, then
ω(v + w) = ω(v) + ω(w) (3.35) and
ω(cv) = cω(v). (3.36)
9The differential of a manifold function will turn out to be a special case of the exterior derivative, which will be introduced later.
 
#page(34)
 Sums and scalar products of one-form fields on a manifold have the following properties. If ω and θ are one-form fields, and if f is a real-valued manifold function, then:
(ω + θ)(v) = ω(v) + θ(v), (3.37) (f ω)(v) = f ω(v). (3.38)
3.5 Coordinate-Basis One-Form Fields
Given a coordinate function χ, we define the coordinate-basis oneform fields Xi by
Xi(v)(m) = v(χi)(m) (3.39) or collectively
X(v)(m) = v(χ)(m). (3.40)
With this definition the coordinate-basis one-form fields are dual to the coordinate-basis vector fields in the following sense (see equation 3.15):10
Xi(Xj)(m) = Xj(χi)(m) = ∂j(χi ◦ χ−1)(χ(m)) = δji. (3.41)
The tuple of basis one-form fields X(v)(m) is an up structure like that of χ.
The general one-form field ω is a linear combination of coordinatebasis one-form fields:
this more simply as
ω(v) = (a ◦ χ) X(v), (3.43) because everything is evaluated at m.
10The Kronecker delta δji is one if i = j and zero otherwise.
ω(v)(m) = a(χ(m)) X(v)(m) =
with coefficient-function tuple a(x), for x = χ(m). We can write
i
ai(χ(m)) Xi(v)(m), (3.42)
 
#page(35)
 The coefficient tuple can be recovered from the one-form field:11
ai(x) = ω(Xi)(χ−1(x)). (3.44)
This follows from the dual relationship (3.41). We can see this as a program:12
(define omega (components->1form-field
(down (literal-function ’a 0 R2->R) (literal-function ’a 1 R2->R))
R2-rect))
((omega (down d/dx d/dy)) R2-rect-point)
(down (a 0 (up x0 y0)) (a 1 (up x0 y0)))
We provide a shortcut for this construction:
(define omega (literal-1form-field ’a R2-rect))
A differential can be expanded in a coordinate basis:
    df(v) =
ciX ̃i(v).
(3.45)
i
The coefficients ci = df(Xi) = Xi(f) = ∂i(f◦χ−1)◦χ are the partial derivatives of the coordinate representation of f in the coordinate system of the basis:
(((d (literal-manifold-function ’f-rect R2-rect)) (coordinate-system->vector-basis R2-rect))
 R2-rect-point)
(down (((partial 0) f-rect) (up x0 y0))
      (((partial 1) f-rect) (up x0 y0)))
However, if the coordinate system of the basis differs from the coordinates of the representation of the function, the result is complicated by the chain rule:
11The analogous recovery of coefficient tuples from vector fields is equation (3.3): biχ,v = v (χi) ◦ χ−1.
12The procedure components->1form-field is analogous to the procedure components->vector-field introduced earlier.
 
#page(36)
 (((d (literal-manifold-function ’f-polar R2-polar)) (coordinate-system->vector-basis R2-rect))
((point R2-polar) (up ’r ’theta)))
(down (- (* (((partial 0) f-polar) (up r theta)) (cos theta))
         (/ (* (((partial 1) f-polar) (up r theta))
               (sin theta))
            r))
      (+ (* (((partial 0) f-polar) (up r theta)) (sin theta))
         (/ (* (((partial 1) f-polar) (up r theta))
               (cos theta))
            r)))
The coordinate-basis one-form fields can be used to find the coefficients of vector fields in the corresponding coordinate vectorfield basis:
Xi(v) = v(χi) = bi ◦ χ (3.46) or collectively,
X(v) = v(χ) = b ◦ χ. (3.47)
A coordinate-basis one-form field is often written dxi. This traditional notation for the coordinate-basis one-form fields is justified by the relation:
dxi = Xi = d(χi). (3.48)
The define-coordinates procedure also makes the basis oneform fields with these traditional names inherited from the coordinates.
We can illlustrate the duality of the coordinate-basis vector fields and the coordinate-basis one-form fields:
(define-coordinates (up x y) R2-rect) ((dx d/dy) R2-rect-point)
0
((dx d/dx) R2-rect-point)
1
We can use the coordinate-basis one-form fields to extract the coefficients of circular on the rectangular vector basis:

#page(37)
 ((dx circular) R2-rect-point)
(* -1 y0)
((dy circular) R2-rect-point)
x0
But we can also find the coefficients on the polar vector basis:
((dr circular) R2-rect-point)
0
((dtheta circular) R2-rect-point)
1
So circular is the same as d/dtheta, as we can see by applying them both to the general function f:
(define f (literal-manifold-function ’f-rect R2-rect)) (((- circular d/dtheta) f) R2-rect-point)
0
Not All One-Form Fields Are Differentials
Although all one-form fields can be constructed as linear combinations of basis one-form fields, not all one-form fields are differentials of functions.
The coefficients of a differential are (see equation 3.45):
ci = Xi(f) = df(Xi) (3.49)
and partial derivatives of functions commute
Xi(Xj(f)) = Xj(Xi(f)). (3.50)
As a consequence, the coefficients of a differential are constrained
Xi(cj) = Xj(ci), (3.51)
but a one-form field can be constructed with arbitrary coefficient functions. For example:
xdx + xdy (3.52)
is not a differential of any function. This is why we started with the basis one-form fields and built the general one-form fields in terms of them.

#page(38)
 Coordinate Transformations
Consider a coordinate change from the chart χ to the chart χ′.
X(v) = v(χ)
= v(χ ◦ (χ′)−1 ◦ χ′)
= (D(χ ◦ (χ′)−1) ◦ χ′) v(χ′)
= (D(χ ◦ (χ′)−1) ◦ χ′) X′(v), (3.53)
where the third line follows from the chain rule for vector fields. One-form fields are independent of coordinates. So,
ω(v) = (a ◦ χ) X(v) = (a′ ◦ χ′) X′(v). (3.54) Eqs. (3.54) and (3.53) require that the coefficients transform under
coordinate transformations as follows:
a(χ(m)) D(χ ◦ (χ′)−1)(χ′(m)) = a′(χ′(m)), (3.55) or
a(χ(m)) = a′(χ′(m)) (D(χ ◦ (χ′)−1)(χ′(m)))−1. (3.56)
The coefficient tuple a(x) is a down structure compatible for contraction with b(x). Let v be the vector with coefficient tuple b(x), and ω be the one-form with coefficient tuple a(x). Then, by equation (3.43),
ω(v) = (a◦χ) (b◦χ).
As a program:
(define omega (literal-1form-field ’a R2-rect))
(define v (literal-vector-field ’b R2-rect))
((omega v) R2-rect-point)
(+ (* (bˆ0 (up x y)) (a0 (up x0 y0))) (* (bˆ1 (up x y)) (a 1 (up x0 y0))))
(3.57)
  Comparing equation (3.56) with equation (3.23) we see that one-form components and vector components transform oppositely, so that
a(x) b(x) = a′(x′) b′(x′), (3.58) as expected because ω(v)(m) is independent of coordinates.

#page(39)
 Exercise 3.2: Verification
Verify that the coefficients of a one-form field transform as described in equation (3.56). You should use equation (3.44) in your derivation.
Exercise 3.3: Hill Climbing
The topography of a region on the Earth can be specified by a manifold function h that gives the altitude at each point on the manifold. Let v be a vector field on the manifold, perhaps specifying a direction and rate of walking at every point on the manifold.
a. Form an expression that gives the power that must be expended to follow the vector field at each point.
b. Write this as a computational expression.