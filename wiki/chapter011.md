!!Polyglot
#page(0)

# Chapter 11: Special Relativity

Although the usual treatments of special relativity begin with the Michelson-Morley experiment, this is not how Einstein began. In fact, Einstein was impressed with Maxwell's work and he was emulating Maxwell's breakthrough.

Maxwell was preceded by Faraday, Ampere, Oersted, Coulomb, Gauss, and Franklin. These giants discovered electromagnetism and worked out empirical equations that described the phenomena. They understood the existence of conserved charges and fields. Faraday invented the idea of lines of force by which fields can be visualized.

Maxwell's great insight was noticing and resolving the contradiction between the empirically-derived laws of electromagnetism and conservation of charge. He did this by introducing the then experimentally undetectable displacement-current term into one of the empirical equations. The modified equations implied a wave equation and the propagation speed of the wave predicted by the new equation turned out to be the speed of light, as measured by the eclipses of the Galilean satellites of Jupiter. The experimental confirmation by Hertz of the existence of electromagnetic radiation that obeyed Maxwell's equations capped the discovery.

By analogy, Einsten noticed that Maxwell's equations were inconsistent with Galilean relativity. In free space, where electromagnetic waves propagate, Maxwell's equations say that the vector source of electric fields is the time rate of change of the magnetic field and the vector source of magnetic field is the time rate of change of the electric field. The combination of these ideas yields the wave equation. The wave equation itself is not invariant under the Galilean transformation: As Einstein noted, if you run with the propagation speed of the wave there is no time variation in the field you observe, so there is no space variation either, contradicting the wave equation. But the Maxwell theory is beautiful, and it can be verified to a high degree of accuracy, so there must be something wrong with Galilean relativity. Einstein resolved the contradiction by generalizing the meaning of the Lorentz transformation, which was invented to explain the failure of the Michelson-Morley experiment. Lorentz and his colleagues decided that the problem with the Michelson-Morley experiment was that matter interacting with the luminiferous ether contracts in the direction of motion. To make this consistent he had to invent a "local time" which had no clear interpretation. Einstein took the Lorentz transformation to be a fundamental replacement for the Galilean transformation in all of mechanics.

Now to the details. Before Maxwell the empirical laws of electromagnetism were as follows. Electric fields arise from charges, with the inverse square law of Coulomb. This is Carl Friedrich Gauss's law for electrostatics:

$$\begin{equation}
\nabla\cdot \overrightarrow{E}= 4\pi\rho.
\tag{11.1}
\end{equation}$$

Magnetic fields do not have a scalar source. This is Gauss's law for magnetostatics:

$$\begin{equation}
\nabla\cdot \overrightarrow{B} = 0. 
\tag{11.2}
\end{equation}$$

Magnetic fields are produced by electric currents, as discovered by Hans Christian Oersted and quantified by André-Marie Ampère:

$$\begin{equation}
\nabla\times B = 4 {\pi \over c} \overrightarrow{I} \tag{11.3}
\end{equation}$$

Michael Faraday (and Joseph Henry) discovered that electric fields are produced by moving magnetic fields:

$$\begin{equation}
 \nabla\times \overrightarrow{E}=  {−1 \over c} {\partial\overrightarrow{B} \over \partial t}. \tag{11.4}
\end{equation}$$

Benjamin Franklin was the first to understand that electrical charges are conserved:

$$\begin{equation}
\nabla\cdot \overrightarrow{I} + {\partial ρ \over \partial t} = 0 \tag{11.5} 
\end{equation}$$

Although these equations are written in terms of the speed of light $c$, these laws were originally written in terms of electrical permittivity and magnetic permeability of free space, which could be determined by measurement of the forces for given currents and charges.

#page(169)
It is easy to see that these equations are mutually contradictory. Indeed, if we take the divergence of equation (11.3) we get

$$\begin{equation}
\nabla\cdot \nabla\times \overrightarrow{B} = 0 = {4 \pi \over c} \nabla\cdot \overrightarrow{I} \tag{11.6}
\end{equation}$$

which directly contradicts conservation of charge (11.5).

Maxwell patched this bug by adding in the displacement current, changing equation (11.3) to read

$$\begin{equation}
\nabla\times \overrightarrow{B} = {-1 \over c} {\partial \over \partial t} + {4 \pi \over c} \overrightarrow{I}. \tag{11.7} 
\end{equation}$$

Maxwell proceeded by taking the curl of equation (11.4) to get

$$\begin{equation}
\nabla\times ( \nabla\times \overrightarrow{E}) = {−1 \over c} {\partial \over \partial t} \nabla\times \overrightarrow{B}. \tag{11.8}
\end{equation}$$

Expanding the left-hand side

$$\begin{equation}
\nabla(\nabla\cdot \overrightarrow{E}) − \nabla^2 \overrightarrow{E} = {−1 \over c} { \partial \over \partial t} \nabla\times \overrightarrow{B},\tag{11.9}
\end{equation}$$

substituting from equations (11.7) and (11.1), and rearranging the terms we get the inhomogeneous wave equation:

$$\begin{equation}
\nabla^2 \overrightarrow{E} - {1 \over c^2} {\partial^2 \overrightarrow{E} \over \partial t^2} =4π \left( \nabla ρ + {1 \over c^2} \overrightarrow{I} \right). \tag{11.10}
\end{equation}$$

We see that in free space (in the absence of any charges or currents) we have the familiar homogeneous linear wave equation. A similar equation can be derived for the magnetic field.

Lorentz, whom Einstein also greatly respected, developed a general formula to describe the force on a particle with charge $q$ moving with velocity $\vec{v}$ in an electromagnetic field:

$$\begin{equation}
\overrightarrow{F} = q\overrightarrow{E} + {q \over c} \overrightarrow{v} \times \overrightarrow{B} . \tag{11.11}
\end{equation}$$

A crucial point in Einstein's inspiration for relativity is, quoting Einstein (in English translation), "During that year [1895–1896] in Aarau the question came to me: If one runs after a light wave with light velocity, then one would encounter a time-independent 

#page(170)
wavefield. However, something like that does not seem to exist!"#Footnote(1) This was the observation of the inconsistency.

Let's be more precise about this. Consider a plane sinusoidal wave moving in the $\hat{x}$ direction with velocity $c$ in free space ($\rho = 0$ and $\vec{I} = 0$). This is a perfectly good solution of the wave equation. Now suppose that an observer is moving with the wave in the $\hat{x}$ direction with velocity $c$.
Such an observer will see no time variation of the field. So the wave equation reduces to Laplace's equation. But a sinusoidal variation in space is not a solution of Laplace's equation.

Einstein believed that the Maxwell-Lorentz electromagnetic theory was fundamentally correct, though he was unhappy with an apparent asymmetry in the formulation. Consider a system consisting of a conductor and a magnet. If the conductor is moved and the magnet is held stationary (a stationary magnetic field) then the charge carriers in the conductor are subject to the Lorentz force (11.11), causing them to move. However, if the magnet is moved past a stationary conductor then the changing magnetic field induces an electric field in the conductor by equation (11.4), which causes the charge carriers in the conductor to move. The actual current which results is identical for both explanations if the relative velocity of the magnet and the conductor are the same. To Einstein, there should not have been two explanations for the same phenomenon.

### Invariance of the Wave Equation

Let $u = (t, x, y, z)$ be a tuple of time and space coordinates that specify a point in spacetime.#Footnote(2) If $\phi(t, x, y, z)$ is a scalar field over time and space, the homogeneous linear wave equation is

$$\begin{equation}
{\partial ^2 \phi(u) \over \partial x^2 } + {\partial ^2 \phi(u) \over \partial y^2 } + {\partial ^2 \phi(u) \over \partial z^2 } − {1 \over c^2} { \partial ^2 \phi(u) \over \partial t^2 }= 0 \tag{11.12}
\end{equation}$$

The characteristics for this equation are the "light cones." If we define a function of spacetime points and increments, length, 

#page(171)
such that for an incremental tuple in position and time $\xi = (\Delta t, \Delta x, \Delta y, \Delta z)$ we have#Footnote(3)

$$\begin{equation}
length_u(ξ) = \sqrt{(Δx)^2 + (Δy)^2 + (Δz)^2 − (c Δt)^2}, \tag{11.13}
\end{equation}$$

then the light cones are the hypersurfaces, for which

$$\begin{equation}
length_u(Δt, Δx, Δy, Δz) = 0. \tag{11.14}
\end{equation}$$

This "length" is called the /interval/.

What is the class of transformations of time and space coordinates that leave the Maxwell-Lorentz theory invariant? The transformations that preserve the wave equation are exactly those that leave its characteristics invariant. We consider a transformation $u = A(u^\prime)$ of time and space coordinates:

$$\begin{equation}
t = A^0(t^\prime, x^\prime, y^\prime, z^\prime),
\tag{11.15}
\end{equation}$$

$$\begin{equation}
x = A^1(t^\prime, x^\prime, y^\prime, z^\prime),
\tag{11.16}
\end{equation}$$

$$\begin{equation}
y = A^2(t^\prime, x^\prime, y^\prime, z^\prime),
\tag{11.17}
\end{equation}$$

$$\begin{equation}
z = A^3(t^\prime, x^\prime, y^\prime, z^\prime).
\tag{11.18}
\end{equation}$$

If we define a new field $\psi(t, x, y, z)$ such that $\psi = \phi \circ A$, or

$$\begin{equation}
\psi(t^\prime, x^\prime, y^\prime, z^\prime) = \phi(A(t^\prime, x^\prime, y^\prime, z^\prime)),
\tag{11.19}
\end{equation}$$

Then $\psi$ will satisfy the wave equation

$$\begin{equation}
{\partial^2ψ(u^\prime ) \over \partial x^{\prime 2}}  + {\partial^2ψ(u^\prime ) \over \partial y^{\prime 2}}  + {\partial^2ψ(u^\prime ) \over \partial z^{\prime 2}}  − {1 \over c^2} {\partial^2ψ(u^\prime ) \over \partial t^{\prime 2}}  = 0
\tag{11.20}
\end{equation}$$

if and only if

$$\begin{equation}
length_u^\prime  (ξ^\prime ) = length_{A(u^\prime )}(DA ξ^\prime ) = length_u(ξ).
\tag{11.21}
\end{equation}$$

But this is just a statement that the velocity of light is invariant under change of the coordinate system. The class of transformations that satisfy equation (11.21) are the Poincaré transformations.

#page(172)
### Lorentz Transformations

Special relativity is usually presented in terms of global Lorentz frames, with rectangular spatial coordinates. In this context the Lorentz transformations (and, more generally, the Poincaré transformations) can be characterized as the set of affine transformations (linear transformations plus shift) of the coordinate tuple (time and spatial rectangular coordinates) that preserve the length of incremental spacetime intervals as measured by

$$\begin{equation}
f(\xi) = -(\xi^0)^2 + (\xi^2)^2 + (\xi^2)^2 + (\xi^3)^2,
\tag{11.22}
\end{equation}$$

where $\xi$ is an incremental 4-tuple that could be added to the coordinate 4-tuple $(ct, x, y, z)$.#Footnote(4) The Poincaré-Lorentz transformations are of the form

$$\begin{equation}
x = \Lambda x^\prime + a,
\tag{11.23}
\end{equation}$$

Where $\Lambda$ is the tuple representation of a linear transformation and $a$ is a 4-tuple shift. Because the 4-tuple includes the time, these transformations include transformations to a uniformly moving frame. A transformation that does not rotate or shift, but just introduces relative velocity, is sometimes called a /boost/.

In general relativity, global Lorentz frames do not exist, and so global affine transformations are irrelevant. In general relativity Lorentz invariance is a local property of incremental 4-tuples at a point.

Incremental 4-tuples transform as

$$\begin{equation}
\xi = \Lambda \xi^\prime.
\tag{11.24}
\end{equation}$$

This places a constraint on the allowed $\Lambda$

$$\begin{equation}
f(\xi^\prime) = f(\Lambda \xi^\prime),
\tag{11.25}
\end{equation}$$

for arbitrary $\xi^\prime$.

The possible $\Lambda$ that are consistent with the preservation of the interval can be completely specified and conveniently parameterized.

#page(173)
### Simple Lorentz Transformations

Consider the linear transformation, in the first two coordinates,

$$\begin{equation}
\begin{aligned}
\xi^0 &= p(\xi^\prime)^0 + q(\xi^\prime)^1 \\
\xi^1 &= r(\xi^\prime)^0 + s(\xi^\prime)^1.
\end{aligned}
\tag{11.26}
\end{equation}$$

The requirement to preserve the interval gives the constraints

$$\begin{equation}
\begin{aligned}
p^2 - r^2 &= 1, \\
pq - rs &= 0, \\
q^2 - s^2 &= -1.
\end{aligned}
\tag{11.27}
\end{equation}$$

There are four parameters to determine, and only three equations, so the solutions have a free parameter. It turns out that a good choice is $\beta = q/p$. Solve to find

$$\begin{equation}
p = \frac{1}{\sqrt{1 - \beta^2}} = \gamma(\beta),
\tag{11.28}
\end{equation}$$

and also $p = s$ and $q = r = \beta p$. This defines $\gamma$. Written out, the transformation is

$$\begin{equation}
\begin{aligned}
\xi^0 &= \gamma(\beta)\left((\xi^\prime)^0 + \beta(\xi^\prime)^1 \right) \\
\xi^1 &= \gamma(\beta)\left(\beta(\xi^\prime)^0 + (\xi^\prime)^1 \right).
\end{aligned}
\tag{11.29}
\end{equation}$$

Simple physical arguments#Footnote(5) show that this mathematical result relates the time and space coordinates for two systems in uniform relative motion. The parameter $\beta$ is related to the relative velocity.

Consider incremental vectors as spacetime vectors relative to an origin in a global inertial frame. So, for example, $\xi = (ct, x)$, ignoring $y$ and $z$ for a moment. The unprimed coordinate origin $x = 0$ corresponds, in primed coordinates, to (using equations 11.29)

$$\begin{equation}
x = 0 = \gamma(\beta)(x^\prime + \beta c t'),
\tag{11.30}
\end{equation}$$

so

$$\begin{equation}
\beta = -\frac{x^\prime}{ct^\prime} = -\frac{v^\prime}{c},
\tag{11.31}
\end{equation}$$

#page(174)
with the definition $v^\prime = x^\prime / t^\prime$. We see that $\beta$ is minus $1/c$ times the velocity ($v^\prime$) of the unprimed system (which moves with its origin) as "seen" in the primed coordinates

$$\begin{equation}
\beta = \frac{x}{ct} = \frac{v}{c}.
\tag{11.32}
\end{equation}$$

So $v^\prime = -v$.

A consistent interpretation is that the origin of the primed system moves with velocity v = βc along the ˆx-axis of the unprimed system. And the unprimed system moves with the same velocity in the other direction, when viewed in terms of the primed system. What happened to the other coordinates: y and z? We did not need them to find this one-parameter family of Lorentz transformations. They are left alone. This mathematical result has a physical interpretation: Lengths are not affected by perpendicular boosts. Think about two observers on a collision course, each carrying a meter stick perpendicular to their relative velocity. At the moment of impact, the meter sticks must coincide. The symmetry of the situation does not permit one observer to conclude that one meter stick is shorter than the other, because the other observer must come to the same conclusion. Both observers can put their conclusions to the test upon impact.

We can fill in the components of this simple boost:

$$\begin{equation}
\begin{aligned}
\xi^0 &= \gamma(\beta)\left((\xi^\prime)^0 + \beta(\xi^\prime)^1 \right) \\
\xi^1 &= \gamma(\beta)\left(\beta(\xi^\prime)^0 + (\xi^\prime)^1 \right) \\
\xi^2 &= (\xi^\prime)^2 \\
\xi^3 &= (\xi^\prime)^3.
\end{aligned}
\tag{11.33}
\end{equation}$$

### More General Lorentz Transformations

One direction was special in our consideration of simple boosts. We can make use of this fact to find boosts in any direction.

Let $c \boldsymbol{\beta} = (v^0, v^1, v^2)$ be the tuple of components of the relative velocity of the origin of the primed system in the unprimed system. The components are with respect to the same rectangular basis used to define the spatial components of any incremental vector.

#page(175)
An incremental vector can be decomposed into vectors parallel and perpendicular to the velocity. Let $\boldsymbol{\xi}$ be the tuple of spatial components of $\xi$, and $\xi^0$ be the time component. Then,

$$\begin{equation}
\boldsymbol{\xi} = \boldsymbol{\xi}^\perp + \boldsymbol{\xi}^\parallel,
\tag{11.34}
\end{equation}$$

where $\boldsymbol{\beta} \cdot \boldsymbol{\xi} = 0$. (This is the ordinary dot product in three dimensions.) Explicitly,

$$\begin{equation}
\boldsymbol{\xi}^\parallel \
= \frac{\boldsymbol{\beta}}{\beta}\left(\frac{\boldsymbol{\beta}}{\beta} \cdot \boldsymbol{\xi} \right),
\tag{11.35}
\end{equation}$$

where $\beta = \left\Vert \boldsymbol{\beta} \right\Vert$, the magnitude of
$\boldsymbol{\beta}$, and

$$\begin{equation}
\boldsymbol{\xi}^\perp = \boldsymbol{\xi} - \boldsymbol{\xi}^\parallel.
\tag{11.36}
\end{equation}$$

In the simple boost of equation (11.33) we can identify $\xi^1$ with the magnitude $|\xi^{\Vert}|$ of the parallel component. The perpendicular component is unchanged:

$$\begin{equation}
\begin{aligned}
\xi^0 &= \gamma(\beta)\left( (\xi^\prime)^0 + \beta |(\boldsymbol{\xi})^\parallel| \right), \\
|\boldsymbol{\xi}^\parallel| &= \gamma(\beta)\left( \beta(\xi^\prime)^0 + |(\boldsymbol{\xi})^\parallel| \right), \\
\boldsymbol{\xi}^\perp &= \left(\boldsymbol{\xi}^\prime\right)^\perp.
\end{aligned}
\tag{11.37}
\end{equation}$$

Putting the components back together, this leads to

$$\begin{equation}
\begin{aligned}
\xi^0 &= \gamma(\beta)\left( (\xi^\prime)^0 + \boldsymbol{\beta} \cdot \boldsymbol{\xi} \right) \\
\boldsymbol{\xi} &= \gamma(\beta)\left(\boldsymbol{\beta}(\xi^\prime)^0 + \boldsymbol{\xi}^\prime \
+ \frac{\gamma(\beta) - 1}{\beta^2} \boldsymbol{\beta} \left( \boldsymbol{\beta} \cdot \boldsymbol{\xi} \right) \right),
\end{aligned}
\tag{11.38}
\end{equation}$$

which gives the components of the general boost $B$ along velocity $c \boldsymbol{\beta}$:

$$\begin{equation}
\xi = B(\boldsymbol{\beta})(\xi^\prime).
\tag{11.39}
\end{equation}$$

### Implementation

We represent a 4-tuple as a flat up-tuple of components.

#page(176)
```Scheme
(define (make-4tuple ct space)
(up ct (ref space 0) (ref space 1) (ref space 2)))
(define (4tuple->ct v) (ref v 0))
(define (4tuple->space v)
(up (ref v 1) (ref v 2) (ref v 3)))
```

The invariant interval is then

```Scheme
(define (proper-space-interval 4tuple)
(sqrt (- (square (4tuple->space 4tuple))
(square (4tuple->ct 4tuple)))))
```

This is a real number for space-like intervals. A space-like interval is one where spatial distance is larger than can be traversed by light in the time interval.

It is often convenient for the interval to be real for time-like intervals, where light can traverse the spatial distance in less than the time interval.

```Scheme
(define (proper-time-interval 4tuple)
(sqrt (- (square (4tuple->ct 4tuple))
(square (4tuple->space 4tuple)))))
```

The general boost $B$ is

```Scheme
(define ((general-boost beta) xi-p)
(let ((gamma (expt (- 1 (square beta)) -1/2)))
(let ((factor (/ (- gamma 1) (square beta))))
(let ((xi-p-time (4tuple->ct xi-p))
(xi-p-space (4tuple->space xi-p)))
(let ((beta-dot-xi-p (dot-product beta xi-p-space)))
(make-4-tuple
(* gamma (+ xi-p-time beta-dot-xi-p))
(+ (* gamma beta xi-p-time)
  xi-p-space
  (* factor beta beta-dot-xi-p))))))))
```

We can check that the interval is invariant:

```Scheme
(- (proper-space-interval
((general-boost (up 'vx 'vy 'vz))
(make-4tuple 'ct (up 'x 'y 'z))))
(proper-space-interval
(make-4tuple 'ct (up 'x 'y 'z)))))
;; 0
```

It is inconvenient that the general boost as just defined does not work if $\boldsymbol{\beta}$ is zero. An alternate way to specify a boost is through the magnitude of $v/c$ and a direction:

#page(177)
```Scheme
(define ((general-boost2 direction v/c) 4tuple-prime)
(let ((delta-ct-prime (4tuple->ct 4tuple-prime))
(delta-x-prime (4tuple->space 4tuple-prime)))
(let ((betasq (square v/c)))
(let ((bx (dot-product direction delta-x-prime))
(gamma (/ 1 (sqrt (- 1 betasq)))))
(let ((alpha (- gamma 1)))
(let ((delta-ct
     (* gamma (+ delta-ct-prime (* bx v/c))))
    (delta-x
     (+ (* gamma v/c direction delta-ct-prime)
        delta-x-prime
        (* alpha direction bx))))
(make-4tuple delta-ct delta-x)))))))
```

This is well behaved as $v/c$ goes to zero.

### Rotations

A linear transformation that does not change the magnitude of the spatial and time components, individually, leaves the interval invariant. So a transformation that rotates the spatial coordinates and leaves the time component unchanged is also a Lorentz transformation. Let $R$ be a 3-dimensional rotation. Then the extension to a Lorentz transformation
$\mathcal{R}$ is defined by

$$\begin{equation}
(\xi^0, \boldsymbol{\xi}) \
= \mathcal{R}(R)\left((\xi^\prime)^0, \boldsymbol{\xi}^\prime \right) \
= \left((\xi^\prime)^0, R(\boldsymbol{\xi}^\prime) \right).
\tag{11.40}
\end{equation}$$

Examining the expression for the general boost, equation (11.38), we see that the boost transforms simply as the arguments are rotated. Indeed,

$$\begin{equation}
B(\boldsymbol{\beta})=\left(\mathcal{R}(R) \right)^{-1} \circ B\left( R(\boldsymbol{\beta}) \right) \circ \mathcal{R}(R).
\tag{11.41}
\end{equation}$$

Note that $\left(\mathcal{R}(R) \right)^{-1} = \mathcal{R}(R^{=1})$. The functional inverse of the extended rotation is the extension of the inverse rotation. We could use this property of boosts to think of the general boost as a combination of a rotation and a simple boost along some special direction.

The extended rotation can be implemented:

```Scheme
(define ((extended-rotation R) xi)
(make-4tuple
(4tuple->ct xi)
(R (4tuple->space xi))))
```

In terms of this we can check the relation between boosts and rotations:

```Scheme
(let ((beta (up 'bx 'by 'bz))
(xi (make-4tuple 'ct (up 'x 'y 'z)))
(R (compose
(rotate-x 'theta)
(rotate-y 'phi)
(rotate-z 'psi)))
(R-inverse (compose
      (rotate-z (- 'psi))
      (rotate-y (- 'phi))
      (rotate-x (- 'theta)))))
(- ((general-boost beta) xi)
((compose (extended-rotation R-inverse)
   (general-boost (R beta))
   (extended-rotation R))
xi)))
;; (up 0 0 0 0)
```

### General Lorentz Transformations

A Lorentz transformation carries an incremental 4-tuple to another 4-tuple. A general linear transformation on 4-tuples has sixteen free parameters. The interval is a symmetric quadratic form, so the requirement that the interval be preserved places only ten constraints on these parameters. Evidently there are six free parameters to the general Lorentz transformation. We already have three parameters that specify boosts (the three components of the boost velocity). And we have three more parameters in the extended rotations. The general Lorentz transformation can be constructed by combining generalized rotations and boosts.

Any Lorentz transformation has a unique decomposition as a generalized rotation followed by a general boost. Any $\Lambda$ that preserves the interval can be written uniquely:

$$\begin{equation}
\Lambda = B(\boldsymbol{\beta})\mathcal{R}.
\tag{11.42}
\end{equation}$$

We can use property (11.41) to see this. Suppose we follow a general boost by a rotation. A new boost can be defined to absorb this rotation, but only if the boost is preceded by a suitable rotation:

$$\begin{equation}
\mathcal{R}(R) \circ B(\boldsymbol{\beta}) = B\left(R(\boldsymbol{\beta})\right) \circ \mathcal{R}(R).
\tag{11.43}
\end{equation}$$

#page(179)
### Exercise 11.1: Lorentz Decomposition

The counting of free parameters supports the conclusion that the general Lorentz transformation can be constructed by combining generalized rotations and boosts. Then the decomposition (11.42) follows from property (11.41).
Find a more convincing proof.

### Special Relativity Frames

A new frame is defined by a Poincaré transformation from a given frame (see equation 11.23). The transformation is specified by a boost magnitude and a unit-vector boost direction, relative to the given frame, and the position of the origin of the frame being defined in the given frame.

Points in spacetime are called events. It must be possible to compare two events to determine if they are the same. This is accomplished in any particular experiment by building all frames involved in that experiment from a base frame, and representing the events as coordinates in that base frame.

When one frame is built upon another, to determine the event from frame-specific coordinates or to determine the frame-specific coordinates for an event requires composition of the boosts that relate the frames to each other. The two procedures that are required to implement this strategy are#Footnote(6)

```Scheme
(define ((coordinates->event ancestor-frame this-frame
                 boost-direction v/c origin) coords)
        ((point ancestor-frame)
        (make-SR-coordinates ancestor-frame
            (+ ((general-boost2 boost-direction v/c) coords)
               origin))))
(define ((event->coordinates ancestor-frame this-frame
                 boost-direction v/c origin) event)
        (make-SR-coordinates this-frame
           ((general-boost2 (- boost-direction) v/c)
            (- ((chart ancestor-frame) event) origin))))
```
#page(180)
With these two procedures, the procedure #Code(make-SR-frame) constructs a new relativistic frame by a Poincaré transformation from a given frame.

```Scheme
(define make-SR-frame
(frame-maker coordinates->event event->coordinates))
```

#### Velocity Addition Formula

For example, we can derive the traditional velocity addition formula. Assume that we have a base frame called #Code(home). We can make a frame #Code(A) by a boost from home in the $\hat{x}$ direction, with components $(1, 0, 0)$, and with a dimensionless measure of the speed $v_a / c$. We also specify that the 4-tuple origin of this new frame coincides with the origin of #Code(home).

```Scheme
(define A
   (make-SR-frame 'A home
     (up 1 0 0)
     'va/c
     (make-SR-coordinates home (up 0 0 0 0))))
```

Frame #Code(B) is built on frame #Code(A) similarly, boosted by $v_b/c$.

```Scheme
(define B
   (make-SR-frame 'B A
     (up 1 0 0)
     'vb/c
     (make-SR-coordinates A (up 0 0 0 0))))
```

So any point at rest in frame #Code(B) will have a speed relative to home. For the spatial origin of frame #Code(B), with #Code(B) coordinates =(up 'ct 0 0 0)=, we have

```Scheme
(let ((B-origin-home-coords
((chart home)
((point B)
(make-SR-coordinates B (up 'ct 0 0 0))))))
(/ (ref B-origin-home-coords 1)
(ref B-origin-home-coords 0)))
;; (/ (+ va/c vb/c) (+ 1 (* va/c vb/c)))
```

obtaining the traditional velocity-addition formula. (Note that the resulting velocity is represented as a fraction of the speed of light.) This is a useful result, so:

#page(181)
```Scheme
(define (add-v/cs va/c vb/c)
(/ (+ va/c vb/c)
(+ 1 (* va/c vb/c))))
```

### Twin Paradox

Special relativity engenders a traditional conundrum: consider two twins, one of whom travels and the other stays at home. When the traveller returns it is discovered that the traveller has aged less than the twin who stayed at home.
How is this possible?

The experiment begins at the start event, which we arbitrarily place at the origin of the home frame.

```Scheme
(define start-event
((point home)
(make-SR-coordinates home (up 0 0 0 0))))
```

There is a homebody and a traveller. The traveller leaves home at the start event and proceeds at $24/25$ of the speed of light in the $\hat{x}$ direction.
We define a frame for the traveller, by boosting from the home frame.

```Scheme
(define outgoing
(make-SR-frame 'outgoing       ; for debugging
     home            ; base frame
     (up 1 0 0)      ; x direction
     24/25           ; velocity as fraction of c
     ((chart home)
      start-event)))
```

After 25 years of home time the traveller is 24 light-years out. We define that event using the coordinates in the home frame. Here we scale the time coordinate by the speed of light so that the units of $ct$ slot in the 4-vector are the same as the units in the spatial slots. Since $v/c = 24/25$ we must multiply that by the speed of light to get the velocity. This is multiplied by 25 years to get the $\hat{x}$ coordinate of the traveller in the home frame at the turning point.

```Scheme
(define traveller-at-turning-point-event
((point home)
(make-SR-coordinates home
            (up (* :c 25) (* 25 24/25 :c) 0 0))))
```
#page(182)
Note that the first component of the coordinates of an event is the speed of light multiplied by time. The other components are distances. For example, the second component (the $\hat{x}$ component) is the distance travelled in 25 years at $24/25$ the speed of light. This is 24 light-years.

If we examine the displacement of the traveller in his own frame we see that the traveller has aged 7 years and he has not moved from his spatial origin.

```Scheme
(- ((chart outgoing) traveller-at-turning-point-event)
((chart outgoing) start-event))
;; (up (* 7 :c) 0 0 0)
```

But in the frame of the homebody we see that the time has advanced by 25 years.

```Scheme
(- ((chart home) traveller-at-turning-point-event)
((chart home) start-event))
;; (up (* 25 :c) (* 24 :c) 0 0)
```

The proper time interval is 7 years, as seen in any frame, because it measures the aging of the traveller:

```Scheme
(proper-time-interval
(- ((chart outgoing) traveller-at-turning-point-event)
((chart outgoing) start-event)))
;; (* 7 :c)
```

```Scheme
(proper-time-interval
(- ((chart home) traveller-at-turning-point-event)
((chart home) start-event)))
;; (* 7 :c)
```

When the traveller is at the turning point, the event of the homebody is:

```Scheme
(define halfway-at-home-event
((point home)
(make-SR-coordinates home (up (* :c 25) 0 0 0))))
```

and the homebody has aged

```Scheme
(proper-time-interval
(- ((chart home) halfway-at-home-event)
((chart home) start-event)))
;; (* 25 :c)
```

```Scheme
(proper-time-interval
(- ((chart outgoing) halfway-at-home-event)
((chart outgoing) start-event)))
;; (* 25 :c)
```
#page(183)
as seen from either frame.

As seen by the traveller, home is moving in the $−\hat{x}$ direction at $24/25$ of the velocity of light. At the turning point (7 years by his time) home is at:

```Scheme
(define home-at-outgoing-turning-point-event
((point outgoing)
(make-SR-coordinates outgoing
            (up (* 7 :c) (* 7 -24/25 :c) 0 0))))
```

Since home is speeding away from the traveller, the twin at home has aged less than the traveller. This may seem weird, but it is OK because this event is different from the halfway event in the home frame.

```Scheme
(proper-time-interval
(- ((chart home) home-at-outgoing-turning-point-event)
((chart home) start-event)))
;; (* 49/25 :c)
```

The traveller turns around abruptly at this point (painful!) and begins the return trip. The incoming trip is the reverse of the outgoing trip, with origin at the turning-point event:

```Scheme
(define incoming
(make-SR-frame 'incoming home
     (up -1 0 0) 24/25
     ((chart home)
      traveller-at-turning-point-event)))
```

After 50 years of home time the traveller reunites with the homebody:

```Scheme
(define end-event
((point home)
(make-SR-coordinates home (up (* :c 50) 0 0 0))))
```

Indeed, the traveller comes home after 7 more years in the incoming frame:

```Scheme
(- ((chart incoming) end-event)
(make-SR-coordinates incoming
            (up (* :c 7) 0 0 0)))
;; (up 0 0 0 0)
```

```Scheme
(- ((chart home) end-event)
((chart home)
((point incoming)
(make-SR-coordinates incoming
              (up (* :c 7) 0 0 0)))))
;; (up 0 0 0 0)
```

The traveller ages only 7 years on the return segment, so his total aging is
14 years:

```Scheme
(+ (proper-time-interval
(- ((chart outgoing) traveller-at-turning-point-event)
((chart outgoing) start-event)))
(proper-time-interval
(- ((chart incoming) end-event)
((chart incoming) traveller-at-turning-point-event))))
;; (* 14 :c)
```

But the homebody ages 50 years:

```Scheme
(proper-time-interval
(- ((chart home) end-event)
((chart home) start-event)))
;; (* 50 :c)
```

At the turning point of the traveller the homebody is at

```Scheme
(define home-at-incoming-turning-point-event
((point incoming)
(make-SR-coordinates incoming
            (up 0 (* 7 -24/25 :c) 0 0))))
```

The time elapsed for the homebody between the reunion and the turning point of the homebody, as viewed by the incoming traveller, is about 2 years.

```Scheme
(proper-time-interval
(- ((chart home) end-event)
((chart home) home-at-incoming-turning-point-event)))
;; (* 49/25 :c)
```

Thus the aging of the homebody occurs at the turnaround, from the point of view of the traveller.

----
### Footnotes


#FootnoteRef(1) The quote is from Pais [12](references!bib_12), p. 131.

#FootnoteRef(2) Points in spacetime are often called /events/.

#FootnoteRef(3) Here the length is independent of the spacetime point specified by $u$. In General Relativity we find that the metric, and thus the length function needs to vary with the point in spacetime.

#FootnoteRef(4) Incrementally, $\xi = \xi^0 \partial / {\partial ct} + \xi^1 \partial /{\partial x} + \xi^2 \partial / {\partial y} + \xi^3 \partial / {\partial z}$.
The length of this vector, using the Minkowski metric (see equation 10.11), is the Lorentz interval, the right-hand side of equation (11.22).

#FootnoteRef(5) See, for instance, Mermin, "Space and Time in Special Relativity."

#FootnoteRef(6) The procedure #Code(make-SR-coordinates) labels the given coordinates with the given frame. The procedures that manipulate coordinates, such as #Code((point ancestor-frame#)), check that the coordinates they are given are in the appropriate frame. This error checking makes it easier to debug relativity procedures.

#FootnoteEnd