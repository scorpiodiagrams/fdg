 

!!Polyglot
## Over a Map
To deal with motion on manifolds we need to think about paths on manifolds and vectors along these paths. Tangent vectors along paths are not vector fields on the manifold because they are defined only on the path. And the path may even cross itself, which would give more than one vector at a point. Here we introduce the concept of a vector field over a map.1 A vector field over a map assigns a vector to each image point of the map. In general the map may be a function from one manifold to another. If the domain of the map is the manifold of the real line, the range of the map is a 1-dimensional path on the target manifold. One possible way to define a vector field over a map is to assign a tangent vector to each image point of a path, allowing us to work with tangent vectors to paths. A one-form field over the map allows us to extract the components of a vector field over the map.

!!Polyglot
### 6.1 Vector Fields Over a Map
Let μ be a map from points n in the manifold N to points m in the manifold M. A vector over the map μ takes directional derivatives of functions on M at points m = μ(n). The vector over the map applied to the function on M is a function on N.
Restricted Vector Fields
One way to make a vector field over a map is to restrict a vector field on M to the image of N over μ, as illustrated in figure 6.1.
Let v be a vector field on M, and f a function on M. Then
vμ(f) = v(f) ◦ μ, (6.1)
is a vector over the map μ. Note that vμ(f) is a function on N, not M:
vμ(f)(n) = v(f)(μ(n)). (6.2) 1See Bishop and Goldberg, Tensor Analysis on Manifolds [3].
 
72
Chapter 6 Over a Map
        μ
          N
μ(N)
M
  The vector field v on M is indicated by arrows. The solid arrows are vμ, the restricted vector field over the map μ. The vector field over the map is restricted to the image of N in M.
We can implement this definition as:
(define ((vector-field->vector-field-over-map mu:N->M) v-on-M) (procedure->vector-field
(lambda (f-on-M)
(compose (v-on-M f-on-M) mu:N->M))))
Differential of a Map
Another way to construct a vector field over a map μ is to transport a vector field from the source manifold N to the target manifold M with the differential of the map
dμ(v)(f)(n) = v(f ◦ μ)(n), (6.3)
which takes its argument in the source manifold N. The differential of a map μ applied to a vector field v on N is a vector field over the map. A procedure to compute the differential is:
(define (((differential mu) v) f) (v (compose f mu)))
Figure 6.1

#page(73)
 The nomenclature of this subject is confused. The “differential of a map between manifolds,” dμ, takes one more argument than the “differential of a real-valued function on a manifold,” df, but when the target manifold of μ is the reals and I is the identity function on the reals,
dμ(v)(I)(n) = (v(I ◦ μ))(n) = (v(μ))(n) = dμ(v)(n). (6.4) We avoid this problem in our notation by distinguishing d and d.
In our programs we encode d as differential and d as d.
Velocity at a Time
Let μ be the map from the time line to the manifold M, and ∂/∂t be a basis vector on the time line. Then dμ(∂/∂t) is the vector over the map μ that computes the rate of change of functions on M along the path that is the image of μ. This is the velocity vector. We can use the differential to assign a velocity vector to each moment, solving the problem of multiple vectors at a point if the path crosses itself.
6.2 One-Form Fields Over a Map
Given a one-form ω on the manifold M, the one-form over the map μ : N → M is constructed as follows:
ωμ(vμ)(n) = ω(u)(μ(n)), where u(f)(m) = vμ(f)(n). (6.5)
The object u is not really a vector field on M even though we have given it that shape so that the dual vector can apply to it; u(f) is evaluated only at images m = μ(n) of points n in N. If we were defining u as a vector field we would need the inverse of μ to find the point n = μ−1(m), but this is not required to define the object u in a context where there is already an m associated with the n of interest. To extend this idea to k-forms, we carry each vector argument over the map.
The procedure that constructs a k-form over the map from a k-form is:

#page(74)
 (define ((form-field->form-field-over-map mu:N->M) w-on-M) (define (make-fake-vector-field V-over-mu n)
(define ((u f) m) ((V-over-mu f) n))
(procedure->vector-field u)) (procedure->nform-field
   (lambda vectors-over-map
     (lambda (n)
((apply w-on-M
(map (lambda (V-over-mu)
(make-fake-vector-field V-over-mu n)) vectors-over-map))
(mu:N->M n)))) (get-rank w-on-M)))
The internal procedure make-fake-vector-field counterfeits a vector field u on M from the vector field over the map μ : N → M. This works here because the only value that is ever passed as m is (mu:N->M n).
6.3 Basis Fields Over a Map
Let e be a tuple of basis vector fields, and  ̃e be the tuple of basis one-forms that is dual to e:
 ̃e i ( e j ) ( m ) = δ ji . ( 6 . 6 ) The basis vectors over the map, eμ, are particular cases of vectors
over a map:
eμ(f) = e(f) ◦ μ. (6.7)
And the elements of the dual basis over the map,  ̃eμ, are particular cases of one-forms over the map. The basis and dual basis over the map satisfy
 ̃e iμ ( e μj ) ( n ) = δ ji . ( 6 . 8 )

#page(75)
 Walking on a Sphere
For example, let μ map the time line to the unit sphere.2 We use colatitude θ and longitude φ as coordinates on the sphere:
(define S2 (make-manifold S^2 2 3)) (define S2-spherical
(coordinate-system-at ’spherical ’north-pole S2)) (define-coordinates (up theta phi) S2-spherical)
(define S2-basis (coordinate-system->basis S2-spherical))
A general path on the sphere is:3
(define mu
(compose (point S2-spherical)
(up (literal-function ’theta) (literal-function ’phi))
           (chart R1-rect)))
The basis over the map is constructed from the basis on the sphere:
(define S2-basis-over-mu (basis->basis-over-map mu S2-basis))
(define h
(literal-manifold-function ’h-spherical S2-spherical))
(((basis->vector-basis S2-basis-over-mu) h) ((point R1-rect) ’t0))
(down
 (((partial 0) h-spherical) (up (theta t0) (phi t0)))
 (((partial 1) h-spherical) (up (theta t0) (phi t0))))
The basis vectors over the map compute derivatives of the function h evaluated on the path at the given time.
2We execute (define-coordinates t R1-rect) to make t the coordinate function of the real line.
3We provide a shortcut to make literal manifold maps:
(define mu (literal-manifold-map ’mu R1-rect S2-spherical))
But if we used this shortcut, the component functions would be named mu^0 and mu^1. Here we wanted to use more mnemonic names for the component functions.
 
#page(76)
 We can check that the dual basis over the map does the correct thing:
(((basis->1form-basis S2-basis-over-mu) (basis->vector-basis S2-basis-over-mu))
((point R1-rect) ’t0))
(up (down 1 0) (down 0 1))
Components of the Velocity
Let χ be a tuple of coordinates on M, with associated basis vectors Xi, and dual basis elements dxi. The vector basis and dual basis over the map μ are Xμi and dxiμ. The components of the velocity (rates of change of coordinates along the path μ) are obtained by applying the dual basis over the map to the velocity
vi(t) = dxiμ(dμ(∂/∂t))(t),
where t is the coordinate for the point t.
For example, the coordinate velocities on a sphere are
(((basis->1form-basis S2-basis-over-mu) ((differential mu) d/dt))
((point R1-rect) ’t0))
(up ((D theta) t0) ((D phi) t0)))
as expected.
6.4 Pullbacks and Pushforwards
(6.9)
Maps from one manifold to another can also be used to relate the vector fields and one-form fields on one manifold to those on the other. We have introduced two such relations: restricted vector fields and the differential of a function. However, there are other ways to relate the vector fields and form fields on different manifolds that are connected by a map.
Pullback and Pushforward of a Function
The pullback of a function f on M over the map μ is defined as μ∗f = f ◦ μ. (6.10)

#page(77)
 This allows us to take a function defined on M and use it to define a new function on N.
For example, the integral curve of v evolved for time t as a function of the initial manifold point m generates a map φvt of the manifold onto itself. This is a simple currying4 of the integral curve of v from m as a function of time: φvt (m) = γmv (t). The evolution of the function f along an integral curve, equation (3.33), can be written in terms of the pullback over φvt :
(Et,vf)(m) = f(φvt (m)) = ((φvt )∗f)(m). (6.11) This is implemented as:
(define ((pullback-function mu:N->M) f-on-M) (compose f-on-M mu:N->M))
A vector field over the map that was constructed by restriction (equation 6.1) can be seen as the pullback of the function constructed by application of the vector field to a function:
vμ(f) = v(f) ◦ μ = μ∗(v(f)). (6.12)
A vector field over the map that was constructed by a differential (equation 6.3) can be seen as the vector field applied to the pullback of the function:
dμ(v)(f)(n) = v(f ◦ μ)(n) = v(μ∗f)(n). (6.13)
If we have an inverse for the map μ we can also define a pushforward of the function g, defined on the source manifold of the map:5
μ∗g = g ◦ μ−1. (6.14)
4A function of two arguments may be seen as a function of one argument whose value is a function of the other argument. This can be done in two different ways, depending on which argument is supplied first. The general process of specifying a subset of the arguments to produce a new function of the others is called currying the function, in honor of the logician Haskell Curry (1900– 1982) who, with Moses Sch ̈onfinkel (1889–1942), developed combinatory logic.
5Notation note: superscript asterisk indicates pullback, subscript asterisk indicates pushforward. Pullbacks and pushforwards are tightly binding operators, so, for example μ∗f(n) = (μ∗f)(n).
 
#page(78)
 Pushforward of a Vector Field
We can also define the pushforward of a vector field over the map μ. The pushforward takes a vector field v defined on N. The result takes directional derivatives of functions on M at a place determined by a point in M:
μ∗v(f)(m) = v(μ∗f)(μ−1(m)) = v(f ◦ μ)(μ−1(m)), (6.15) or
μ∗v(f) = μ∗(v(μ∗f)). (6.16)
Here we expressed the pushforward of the vector field in terms of pullbacks and pushforwards of functions. Note that the pushforward requires the inverse of the map.
If the map is from time to some configuration manifold and represents the time evolution of a process, we can think of the pushforward of a vector field as a velocity measured at a point on the trajectory in the configuration manifold. By contrast, the differential of the map applied to the vector field gives us the velocity vector at each moment in time. Because a trajectory may cross itself, the pushforward is not defined at any point where the crossing occurs, but the differential is always defined.
Pushforward Along Integral Curves
We can push a vector field forward over the map generated by an integral curve of a vector field w, because the inverse is always available.6
((φwt )∗v)(f)(m) = v((φwt )∗f)(φw−t(m)) = v(f ◦ φwt )(φw−t(m)). (6.17) This is implemented as:
(define ((pushforward-vector mu:N->M mu^-1:M->N) v-on-N) (procedure->vector-field
(lambda (f)
(compose (v-on-N (compose f mu:N->M)) mu^-1:M->N))))
6The map φwt is always invertible: (φwt )−1 = φw−t because of the uniqueness of the solutions of the initial-value problem for ordinary differential equations.
 
#page(79)
 Pullback of a Vector Field
Given a vector field v on manifold M we can pull the vector field back through the map μ : N → M as follows:
μ∗v(f)(n) = (v(f ◦ μ−1))(μ(n)) (6.18) or
μ∗v(f) = μ∗(v(μ∗f)). (6.19)
This may be useful when the map is invertible, as in the flow generated by a vector field.
This is implemented as:
(define (pullback-vector-field mu:N->M mu^-1:M->N) (pushforward-vector mu^-1:M->N mu:N->M))
Pullback of a Form Field
We can also pull back a one-form field ω defined on M, but an honest definition is rarely written. The pullback of a one-form field applied to a vector field is intended to be the same as the one-form field applied to the pushforward of the vector field.
The pullback of a one-form field is often described by the relation
μ∗ω(v) = ω(μ∗v), (6.20)
but this is wrong, because the two sides are not functions of points in the same manifold. The one-form field ω applies to a vector field on the manifold M, which takes a directional derivative of a function defined on M and is evaluated at a point on M, but the left-hand side is evaluated at a point on the manifold N.
A more precise description would be
μ∗ω(v)(n) = ω(μ∗v)(μ(n)) (6.21) or
μ∗ω(v) = μ∗(ω(μ∗v)). (6.22)

#page(80)
 Although this is accurate, it may not be effective, because computing the pushforward requires the inverse of the map μ. But the inverse is available when the map is the flow generated by a vector field.
In fact it is possible to compute the pullback of a one-form field without having the inverse of the map. Instead we can use form-field->form-field-over-map to avoid needing the inverse:
μ∗ω(v)(n) = ωμ(dμ(v))(n).
The pullback of a k-form generalizes equation 6.21:
μ∗ω(u, v, . . .)(n) = ω(μ∗u, μ∗v, . . .)(μ(n)).
This is implemented as follows:7
(define ((pullback-form mu:N->M) omega-on-M) (let ((k (get-rank omega-on-M)))
(if (= k 0)
((pullback-function mu:N->M) omega-on-M) (procedure->nform-field
(6.23)
(6.24)
(lambda vectors-on-N
(apply ((form-field->form-field-over-map mu:N->M)
omega-on-M)
(map (differential mu:N->M) vectors-on-N)))
k))))
Properties of Pullback
The pullback through a map has many nice properties: it distributes through addition and through wedge product:
μ∗(θ + φ) = μ∗θ + μ∗φ, (6.25) μ∗(θ ∧ φ) = μ∗θ ∧ μ∗φ. (6.26)
The pullback also commutes with the exterior derivative: d(μ∗θ) = μ∗(dθ), (6.27) for θ a function or k-form field.
7There is a generic pullback procedure that operates on any kind of manifold object. However, to pull a vector field back requires providing the inverse map.
 
#page(81)
 We can verify this by computing an example. Let μ map the rectangular plane to rectangular 3-space:
(define mu (literal-manifold-map ’MU R2-rect R3-rect))
First, let’s compare the pullback of the exterior derivative of a function with the exterior derivative of the pullback of the function:
(define f (literal-manifold-function ’f-rect R3-rect)) (define X (literal-vector-field ’X-rect R2-rect))
(((- ((pullback mu) (d f)) (d ((pullback mu) f))) X) ((point R2-rect) (up ’x0 ’y0)))
0
More generally, we can consider what happens to a form field. For a one-form field the result is as expected:
(define theta (literal-1form-field ’THETA R3-rect)) (define Y (literal-vector-field ’Y-rect R2-rect))
(((- ((pullback mu) (d theta)) (d ((pullback mu) theta))) X Y) ((point R2-rect) (up ’x0 ’y0)))
0
Pushforward of a Form Field
By symmetry, it is possible to define the pushforward of a oneform field as
μ∗ω(v) = μ∗(ω(μ∗v)), (6.28)
but this is rarely useful.
Exercise 6.1: Velocities on a Globe
We can use manifold functions, vector fields, and one-forms over a map to understand how paths behave.
a. Suppose that a vehicle is traveling east on the Earth at a given rate of change of longitude. What is the actual ground speed of the vehicle?
b. Stereographic projection is useful for navigation because it is conformal (it preserves angles). For the situation of part a, what is the speed measured on a stereographic map? Remember that the stereographic projection is implemented with S2-Riemann.

