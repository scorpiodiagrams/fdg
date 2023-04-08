!!Polyglot

## SCM Utils

The library behind the book is Gerald Sussman’s **scmutils**, short for “Scheme Utilities”. It’s a ~65,000-lines-of-code treasure trove of ideas and experiments in physics, electrical engineering and software design accumulated over the course of Sussman’s career. He started the project during a sabbatical to Caltech in the late 80s to work with Feynman and learn astronomy, and has spent the subsequent decades enriching it with the artefacts produced by his intense curiosity.

### Symbolic Manipulation

SCM Utils has numerical code *and* code for manipulating equations symbolically. It can differentiate symbolic expressions. It is happy applying operations like compose and square to abstract functions like 'f', that don't have any specific implementation (such functions like 'f' that only have a name are called 'function literals').

The LISP family of languages is well suited to this kind of flexibility between numerical and symbolic. The library avoids an artificial distinction between symbolic and numerical code. Code is data.  Any formula can be inspected by LISP code. Whether that is evaluation or some more abstract operation on the formula is up to the code.

## Overview of the Library
!!Scorpio
# ScmUtils
!!Polyglot

The diagram above is a sketchy top level look at the contents of the mathematics library. Hover over a label for more information.

### Clojure and Emmy
More details of Colin Smith's port of scmutils to modern clojure can be found at:
* [Clojure Version documentation](https://cljdoc.org/d/sicmutils/sicmutils/0.22.0/doc/introduction)
Colin switched to and tested faster datastructures, made possible by use of the Clojure LISP variant.

In Sam Ritchie's algebra platform 'Emmy', Colin's port of the core mathematics is combined with libraries for the internet, for 2D and 3D plotting, for dynamic modification of values and an editor for LaTeX formulae.
* [Emmy Algebra System](https://nextjournal.com/samritchie/sicmutils)

### SCM Utils
The original refman.txt documentation of SCM Utils is at:
* [Original Scheme documentation](https://groups.csail.mit.edu/mac/users/gjs/6946/refman.txt)
