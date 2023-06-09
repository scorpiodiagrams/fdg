!!Polyglot
#page(0)
# Appendix C: Tensors

There are a variety of objects that have meaning independent of any particular basis. Examples are form fields, vector fields, covariant derivative, and so on.
We call objects that are independent of basis /geometric objects/. Some of these are functions that take other geometric objects, such as vector fields and form fields, as arguments and produce further geometric objects. We refer to such functions as /geometric functions/. We want the laws of physics to be independent of the coordinate systems. How we describe an experiment should not affect the result. If we use only geometric objects in our descriptions then this is automatic.

A geometric function of vector fields and form fields that is linear in each argument with functions as multipliers is called a /tensor/. For example, let
$\mathsf{T}$ be a geometric function of a vector field and form field that gives a real-number result at the manifold point $\mathsf{m}$. Then

$$\begin{equation}
\mathsf{T}(\mathsf{f} \mathsf{u} + \mathsf{g} \mathsf{v}, \boldsymbol{\omega}) \
= \mathsf{f}\,\mathsf{T}(\mathsf{u}, \boldsymbol{\omega}) + \mathsf{g} \mathsf{T}(\mathsf{u}, \boldsymbol{\omega})
\end{equation}$$

$$\begin{equation}
\mathsf{T}(\mathsf{u}, \mathsf{f} \boldsymbol{\omega} + \mathsf{g} \boldsymbol{\theta}) \
= \mathsf{f}\,\mathsf{T}(\mathsf{u}, \boldsymbol{\omega}) + \mathsf{g} \mathsf{T}(\mathsf{u}, \boldsymbol{\theta}),
\end{equation}$$

where $\mathsf{u}$ and $\mathsf{v}$ are vector fields, $\boldsymbol{\omega}$ and
$\boldsymbol{\theta}$ are form fields, and $\mathsf{f}$ and $\mathsf{g}$ are manifold functions. That a tensor is linear over functions and not just constants is important.

The multilinearity over functions implies that the components of the tensor transform in a particularly simple way as the basis is changed. The components of a real-valued geometric function of vector fields and form fields are obtained by evaluating the function on a set of basis vectors and their dual form basis. In our example,

$$\begin{equation}
\mathsf{T}_j^i = \mathsf{T}(e_j, \tilde{e}^i),
\end{equation}$$

for basis vector fields $\mathsf{e}_j$ and dual form fields $\tilde{e}^i$. On the left, $\mathsf{T}_j^i$ is a function of place (manifold point); on the right, $\mathsf{T}$ is a function of a vector field and a form field that returns a function of place.

Now we consider a change of basis, $\mathsf{e}(\mathsf{f}) =
\mathsf{e}^\prime(\mathsf{f})\mathsf{J}$ or

$$\begin{equation}
\mathsf{e}_i(\mathsf{f}) = \sum_j{\mathsf{e}^\prime(\mathsf{f})\mathsf{J}_i^j},
\end{equation}$$

Where $\mathsf{J}$ typically depends on place. The corresponding dual basis transforms as

$$\begin{equation}
\tilde{\mathsf{e}}^i(\mathsf{v}) = \sum_j{\mathsf{K}_j^i \tilde{\mathsf{e}}^{\prime j}(\mathsf{v})},
\end{equation}$$

where $\mathsf{K} = \mathsf{J}^{-1}$ or $\sum_j{\mathsf{K}_j^i(\mathsf{m}) \mathsf{J}_k^j(\mathsf{m})} = \delta_k^i$.

Because the tensor is multilinear over functions, we can deduce that the tensor components in the two bases are related by, in our example,

$$\begin{equation}
\mathsf{T}_j^i = \sum_{kl}{\mathsf{K}_k^i \mathsf{T}_l^{\prime k} \mathsf{J}_j^l},
\end{equation}$$

or

$$\begin{equation}
\mathsf{T}_j^{\prime i} = \sum_{kl}{\mathsf{J}_k^i \mathsf{T}_l^k \mathsf{K}_j^l}.
\end{equation}$$

Tensors are a restricted set of mathematical objects that are geometric, so if we restrict our descriptions to tensor expressions they are /prima facie/ independent of the coordinates used to represent them. So if we can represent the physical laws in terms of tensors we have built in the coordinate-system independence.

Let's test whether the geometric function $\mathsf{R}$, which we have called the Riemann tensor (see equation 8.2), is indeed a tensor field. A real-valued geometric function is a tensor if it is linear (over the functions) in each of its arguments. We can try it for 3-dimensional rectangular coordinates:

```Scheme
(let ((cs R3-rect))
  (let ((u (literal-vector-field 'u-coord cs))
        (v (literal-vector-field 'v-coord cs))
        (w (literal-vector-field 'w-coord cs))
        (x (literal-vector-field 'x-coord cs))
        (omega (literal-1form-field 'omega-coord cs))
        (nu (literal-1form-field 'nu-coord cs))
        (f (literal-manifold-function 'f-coord cs))
        (g (literal-manifold-function 'g-coord cs))
        (nabla (covariant-derivative (literal-Cartan 'G cs)))
        (m (typical-point cs)))
    (let ((F (Riemann nabla)))
      ((up (- (F (+ (* f omega) (* g nu)) u v w)
              (+ (* f (F omega u v w)) (* g (F nu u v w))))
           (- (F omega (+ (* f u) (* g x)) v w)
              (+ (* f (F omega u v w)) (* g (F omega x v w))))
           (- (F omega v (+ (* f u) (* g x)) w)
              (+ (* f (F omega v u w)) (* g (F omega v x w))))
           (- (F omega v w (+ (* f u) (* g x)))
              (+ (* f (F omega v w u)) (* g (F omega v w x)))))
       m))))
;; (up 0 0 0 0)
```

Now that we are convinced that the Riemann tensor is indeed a tensor, we know how its components change under a change of basis. Let

$$\begin{equation}
\mathsf{R}_{jkl}^i = \mathsf{R}(\tilde{\mathsf{e}}^i, \mathsf{e}_j, \mathsf{e}_k, \mathsf{e}_l),
\end{equation}$$

then

$$\begin{equation}
\mathsf{R}_{jkl}^i = \sum_{mnpq}{\mathsf{K}_m^i \mathsf{R}_{npq}^{\prime m} \mathsf{J}_j^n \mathsf{J}_k^p \mathsf{J}_l^q},
\end{equation}$$

or

$$\begin{equation}
\mathsf{R}_{jkl}^i = \sum_{mnpq}{\mathsf{J}_m^i \mathsf{R}_{npq}^m \mathsf{K}_j^n \mathsf{K}_k^p \mathsf{K}_l^q}.
\end{equation}$$

Whew!

It is easy to generalize these formulas to tensors with general arguments. We have formulated the general tensor test as a program #Code(tensor-test) that takes the procedure #Code(T) to be tested, a list of argument types, and a coordinate system to be used. It tests each argument for linearity (over functions). If the function passed as T is a tensor, the result will be a list of zeros.

```Scheme
(tensor-test
 (Riemann (covariant-derivative (literal-Cartan 'G R3-rect)))
 '(1form vector vector vector)
 R3-rect)
;; (0 0 0 0)
```

and so does the torsion (see equation 8.21):

```Scheme
(tensor-test
 (torsion (covariant-derivative (literal-Cartan 'G R3-rect)))
 '(1form vector vector)
 R3-rect)
;; (up 0 0 0)
```

But not all geometric functions are tensors. The covariant derivative is an interesting and important case. The function $\mathsf{F}$, defined by

$$\begin{equation}
\mathsf{F}(\boldsymbol{\omega}, \mathsf{u}, \mathsf{v}) = \boldsymbol{\omega}(\nabla_\mathsf{u} \mathsf{v}),
\end{equation}$$

is a geometric object, since the result is independent of the coordinate system used to represent the $\nabla$. For example:

```Scheme
(define ((F nabla) omega u v)
  (omega ((nabla u) v)))
(((- (F (covariant-derivative
         (Christoffel->Cartan
          (metric->Christoffel-2
           (coordinate-system->metric S2-spherical)
           (coordinate-system->basis S2-spherical)))))
     (F (covariant-derivative
         (Christoffel->Cartan
          (metric->Christoffel-2
           (coordinate-system->metric S2-stereographic)
           (coordinate-system->basis S2-stereographic))))))
  (literal-1form-field 'omega S2-spherical)
  (literal-vector-field 'u S2-spherical)
  (literal-vector-field 'v S2-spherical))
 ((point S2-spherical) (up 'theta 'phi)))
;; 0
```

But it is not a tensor field:

```Scheme
(tensor-test
 (F (covariant-derivative (literal-Cartan 'G R3-rect)))
 '(1form vector vector)
 R3-rect)
;; (0 0 MESS)
```

This result tells us that the function $\mathsf{F}$ is linear in its first two arguments but not in its third argument.

That the covariant derivative is not linear over functions in the second vector argument is easy to understand. The first vector argument takes derivatives of the coefficients of the second vector argument, so multiplying these coefficients by a manifold function changes the derivative.
