!!Polyglot

FUNCTIONAL DIFFERENTIAL GEOMETRY
Gerald Jay Sussman and Jack Wisdom with Will Farr

Functional Differential Geometry

Functional Differential Geometry
Gerald Jay Sussman and Jack Wisdom with Will Farr
The MIT Press Cambridge, Massachusetts London, England

�c 2013 Massachusetts Institute of Technology
This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 3.0 Unported License. To view a copy of this license, visit creativecommons.org.
Other than as provided by this license, no part of this book may be reproduced, transmitted, or displayed by any electronic or mechanical means without permission from the MIT Press or as permitted by law.
MIT Press books may be purchased at special quantity discounts for business or sales promotional use. For information, please email
special sales@mitpress.mit.edu or write to Special Sales Department, The MIT Press, 55 Hayward Street, Cambridge, MA 02142.
This book was set in Computer Modern by the authors with the LATEX typesetting system and was printed and bound in the United States of America.
 Library of Congress Cataloging-in-Publication Data
Sussman, Gerald Jay.
Functional Differential Geometry / Gerald Jay Sussman and Jack Wisdom; with Will Farr.
p. cm.
Includes bibliographical references and index.
ISBN 978-0-262-01934-7 (hardcover : alk. paper)
1. Geometry, Differential. 2. Functional Differential Equations. 3. Mathematical Physics.
I. Wisdom, Jack. II. Farr, Will. III. Title.
QC20.7.D52S87 2013 516.3'6—dc23
10 9 8 7 6 5 4 3 2 1
2012042107

#Quote(The author has spared himself no pains in his endeavour to present the main ideas in the simplest and most intelligible form, and on the whole, in the sequence and connection in which they actually originated. In the interest of clearness, it appeared to me inevitable that I should repeat myself frequently, without paying the slightest attention to the elegance of the presentation. I adhered scrupulously to the precept of that brilliant theoretical physicist L. Boltzmann, according to whom matters of elegance ought be left to the tailor and to the cobbler.)
#Caption Albert Einstein, in Relativity, the Special and General Theory, (1961#), p. v

Contents
Preface xi Prologue xv
1 Introduction 1
2 Manifolds 11
2.1 Coordinate Functions 12
2.2 Manifold Functions 14
3 Vector Fields and One-Form Fields 21
3.1 Vector Fields 21
3.2 Coordinate-Basis Vector Fields 26
3.3 Integral Curves 29
3.4 One-Form Fields 32
3.5 Coordinate-Basis One-Form Fields 34
4 Basis Fields 41
4.1 Change of Basis 44
4.2 Rotation Basis 47
4.3 Commutators 48
5 Integration 55
5.1 Higher Dimensions 57
5.2 Exterior Derivative 62
5.3 Stokes’s Theorem 65

viii Contents
 5.4 Vector Integral Theorems 67
6 OveraMap 71
6.1 Vector Fields Over a Map 71
6.2 One-Form Fields Over a Map 73
6.3 Basis Fields Over a Map 74
6.4 Pullbacks and Pushforwards 76
7 Directional Derivatives 83
7.1 Lie Derivative 85
7.2 Covariant Derivative 93
7.3 Parallel Transport 104
7.4 Geodesic Motion 111
8 Curvature 115
8.1 Explicit Transport 116
8.2 Torsion 124
8.3 Geodesic Deviation 125
8.4 Bianchi Identities 129
9 Metrics 133
9.1 Metric Compatibility 135
9.2 Metrics and Lagrange Equations 137
9.3 General Relativity 144
10 Hodge Star and Electrodynamics 153
10.1 The Wave Equation 159 10.2 Electrodynamics 160
11 Special Relativity 167
11.1 Lorentz Transformations 172
11.2 Special Relativity Frames 179

Contents ix
 11.3 Twin Paradox 181
A Scheme 185
B Our Notation 195
C Tensors 211
References 217 Index 219
