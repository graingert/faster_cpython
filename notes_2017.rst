+++++++++++++++++++++++++++++++++++++++++++++
Notes on Python and CPython performance, 2017
+++++++++++++++++++++++++++++++++++++++++++++

* Python is slow compared to C (also compared to Javascript and/or PHP?)
* Solutions:

  * Ignore Python issue and solve the problem externally: buy faster hardware,
    buy new servers. At the scale of a computer: spawn more Python processes
    to feed all CPUs.
  * Use a different programming language: rewrite the whole application,
    or at least the functions where the program spend most of its time.
    Dropbox rewrote performance critical code in Go, then Dropbox stopped to
    sponsor Pyston.
  * Optimize CPython: solution discussed here. The two other options are not
    always feasible. Rewriting OpenStack in a different language would be
    too expensive for "little gain". Buying more hardware can become too
    expensive at very large scale.

* Python optimizations are limited by:

  * Weak typing: function prototypes don't have to define types of parameters
    and the return value. Annotations are fully optional and there is no plan
    to make type checks mandatory.
  * Python semantics: Python has powerful features which prevents optimizations.
    Examples: introspection and monkey-patching. A simple instruction like
    ``obj.attr`` can call ``type(obj).__getattr__(attr)`` or
    ``type(obj).__getattribute__(attr)``, but it also requires to handle
    descriptors: call ``descr.__get__(obj)``... It's not always a simple
    dictionary lookup. It's not allowed to replace ``len("abc")`` with ``3``
    without a guard on the ``len`` global variable and the ``len()`` builtin
    function.

* CPython optimizations are limited by:

  * Age of its implementation: 20 years ago, phones didn't have 4 CPUs.
  * CPython implementation was designed to be simple to maintain, performance
    was not a design goal.
  * CPython exposes basically all its internal in a "C API" which is *widely*
    used by Python extensions modules (written in C) like numpy.
  * The C API exposes major implementation design choices:

    * Reference counting
    * A specific implementation of garbage collector
    * Global Interpreter Lock (GIL)
    * C structures: C extensions *can* access structure members, not everything
      is hidden in macros or functions.


JIT compiler
============

"Rebase" Pyston on Python 3.7 and continue the project?

Efficient optimizations require type information and assumptions. For example,
function inlining requires that the inlined function is not modified. Guards
must be added to deoptimize if something changed, and these guards must be
checked at runtime.

Collecting information on types can be done at runtime. Type annotation might
help, but Numba and PyPy need more precise types.

PyPy is a very efficient JIT compiler for Python, it is fully compatible with
the Python language, but its support of the CPython C API is still incomplete
and slower (the API is "emulated").

Failure of previous JIT compilers for CPython:

* Pyjion (not completely dead yet), written with Microsoft CLR
* Pyston (Dropbox doesn't sponsor it anymore), only support Python 2.7
* Unladen Swallow (dead)

Explanation of these failures:

* Unladen Swallow: LLVM wasn't as good as expected for dynamic languages like
  Python. Unladen Swallow contributed a lot to LLVM.
* LLVM API evolving quickly.
* Lack of sponsoring: it's just to justify working on Python performances.
  (see: "Spawn more processes! Buy new hardware!")
* Optimizing Python is harder than expected?

Notes on a JIT compiler for CPython:

* compatibility with CPython must be the most important point, PyPy took years
  to be fully compatible with CPython, compatibility was one reason of Pyston
  project failure
* must run Django faster than CPython: Django, not only microbenchmarks
* must keep compatibility with the C API
* be careful of memory usage: major issue in Unladen Swallow, and then Pyston


Optimization ahead of time (AoT compiler)
=========================================

See FAT Python project which adds guards checked at runtime.


Break the C API?
================

The stable ABI created a subset of the CPython C API and hides most
implementation details, but not all of them. Sadly, it's not popular... not
sure if it really works in practice. Not sure that it would be feasible to use
the stable ABI in numpy for example?

The Gilectomy project (CPython without GIL but locks per object) proposes to
add a new compilation mode for extensions compatible with Gilectomy, but keep
backward compatibility.


New language similar to Python
==============================

PHP has the `Hack language <http://hacklang.org/>`_ which is similar but more
strict and so easier to optimize in `HHVM <http://hhvm.com/>`_ (JIT compiler
for PHP and Hack).

Monkey-patching is very popular for unit tests, but do we need it on production
applications?

Some parts of the Python language are very complex like getting an attribute
(``obj.attr``). Would it be possible to restrict such feature? Would it
allow to optimize a Python implementation?

