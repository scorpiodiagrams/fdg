!!Polyglot
## Tensors
There are a variety of objects that have meaning independent of any particular basis. Examples are form fields, vector fields, covariant derivative, and so on. We call objects that are independent of basis geometric objects. Some of these are functions that take other geometric objects, such as vector fields and form fields, as arguments and produce further geometric objects. We refer to such functions as geometric functions. We want the laws of physics to be independent of the coordinate systems. How we describe an experiment should not affect the result. If we use only geometric objects in our descriptions then this is automatic.
A geometric function of vector fields and form fields that is linear in each argument with functions as multipliers is called a tensor. For example, let T be a geometric function of a vector field and form field that gives a real-number result at the manifold point m. Then
T(fu + gv,ω) = f T(u,ω) + gT(v,ω) (C.1) T(u,fω + gθ) = f T(u,ω) + gT(u,θ), (C.2)
where u and v are vector fields, ω and θ are form fields, and f and g are manifold functions. That a tensor is linear over functions and not just constants is important.
The multilinearity over functions implies that the components of the tensor transform in a particularly simple way as the basis is changed. The components of a real-valued geometric function of vector fields and form fields are obtained by evaluating the function on a set of basis vectors and their dual form basis. In our example,
T ij = T ( e j ,  ̃e i ) , ( C . 3 )
for basis vector fields ej and dual form fields  ̃ei. On the left, Tij is a function of place (manifold point); on the right, T is a function of a vector field and a form field that returns a function of place.

#page(212)
 Now we consider a change of basis, e(f) = e′(f) J or
e ′j ( f ) J ji , ( C . 4 )
where J typically depends on place. The corresponding dual basis transforms as
Kij  ̃e′j(v), (C.5) whereK=J−1 or Ki(m)Jj(m)=δi.
e i ( f ) =
 ̃ei(v) =
or T′i =
j
j
Ji TkKl . j klj
kl
jjkk
Because the tensor is multilinear over functions, we can deduce that the tensor components in the two bases are related by, in our example,
Ti = KiT′kJl, j klj
kl
(C.6)
(C.7)
Tensors are a restricted set of mathematical objects that are geometric, so if we restict our descriptions to tensor expressions they are prima facie independent of the coordinates used to represent them. So if we can represent the physical laws in terms of tensors we have built in the coordinate-system independence.
Let’s test whether the geometric function R, which we have called the Riemann tensor (see equation 8.2), is indeed a tensor field. A real-valued geometric function is a tensor if it is linear (over the functions) in each of its arguments. We can try it for 3-dimensional rectangular coordinates:

#page(213)
 (let ((cs R3-rect))
(let ((u
      (v
      (w
      (x
(literal-vector-field ’u-coord cs)) (literal-vector-field ’v-coord cs)) (literal-vector-field ’w-coord cs)) (literal-vector-field ’x-coord cs))
Ri = jkl
or
R′i =
mnpq
mnpq
Ki R′m JnJpJq, m npq j k l
Ji Rm KnKpKq. m npq j k l
jkl
(omega (literal-1form-field ’omega-coord cs))
(nu (literal-1form-field ’nu-coord cs))
(f (literal-manifold-function ’f-coord cs))
(g (literal-manifold-function ’g-coord cs))
(nabla (covariant-derivative (literal-Cartan ’G cs))) (m (typical-point cs)))
(let ((F (Riemann nabla)))
((up (-
(F (+ (* f omega) (* g nu)) u v w)
(+ (* f (F omega u v w)) (* g (F nu u v w))))
(- (F omega (+ (* f u) (* g x)) v w)
(+ (* f (F omega u v w)) (* g (F omega x v w))))
(- (F omega v (+ (* f u) (* g x)) w)
(+ (* f (F omega v u w)) (* g (F omega v x w))))
(- (F omega v w (+ (* f u) (* g x)))
(+ (* f (F omega v w u)) (* g (F omega v w x))))) m))))
(up 0 0 0 0)
Now that we are convinced that the Riemann tensor is indeed a tensor, we know how its components change under a change of basis. Let
R ij k l = R (  ̃e i , e j , e k , e l ) , then
( C . 8 )
(C.9)
(C.10)
Whew!
It is easy to generalize these formulas to tensors with general
arguments. We have formulated the general tensor test as a program tensor-test that takes the procedure T to be tested, a list of argument types, and a coordinate system to be used. It tests each argument for linearity (over functions). If the function passed as T is a tensor, the result will be a list of zeros.

#page(214)
 So, for example, Riemann proves to be a tensor
(tensor-test
(Riemann (covariant-derivative (literal-Cartan ’G R3-rect))) ’(1form vector vector vector)
R3-rect)
(0 0 0 0)
and so does the torsion (see equation 8.21):
(tensor-test
(torsion (covariant-derivative (literal-Cartan ’G R3-rect))) ’(1form vector vector)
R3-rect)
(up 0 0 0)
But not all geometric functions are tensors. The covariant derivative is an interesting and important case. The function F, defined by
F(ω, u, v) = ω(∇uv), (C.11) is a geometric object, since the result is independent of the coor-
dinate system used to represent the ∇. For example: (define ((F nabla) omega u v)
(omega ((nabla u) v)))
(((- (F (covariant-derivative (Christoffel->Cartan
(metric->Christoffel-2 (coordinate-system->metric S2-spherical) (coordinate-system->basis S2-spherical)))))
     (F (covariant-derivative
         (Christoffel->Cartan
(metric->Christoffel-2
(coordinate-system->metric S2-stereographic) (coordinate-system->basis S2-stereographic))))))
(literal-1form-field ’omega S2-spherical) (literal-vector-field ’u S2-spherical) (literal-vector-field ’v S2-spherical))
((point S2-spherical) (up ’theta ’phi)))
0
But it is not a tensor field:

#page(215)
 (tensor-test
(F (covariant-derivative (literal-Cartan ’G R3-rect))) ’(1form vector vector)
R3-rect)
(0 0 MESS)
This result tells us that the function F is linear in its first two arguments but not in its third argument.
That the covariant derivative is not linear over functions in the second vector argument is easy to understand. The first vector argument takes derivatives of the coefficients of the second vector argument, so multiplying these coefficients by a manifold function changes the derivative.
