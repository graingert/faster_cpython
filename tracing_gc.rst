++++++++++++++++++++++++++++++++
CPython GC Meeting, Oct 24, 2020
++++++++++++++++++++++++++++++++

Attending
=========

* Neil Schemenauer
* Joannah Nanjekye
* Pablo Galindo Salgado

Agenda
======

* How to do the roots for tracing GC? Explain to Neil how could work.
* Define why a tracing a GC for CPython would be worthwhile (for CPython
  community, for research institution, for corporpate sponser)
* What are the constraints for changing CPython (backwards compatibility,
  support from core Python devs)

CPython tracing GC design, Constraints
======================================

* Must allow 3 rd party C extensions to compile and work without major source
  code changes
* Must allow most pure Python code to run, without changes. Some breakage
  expected (e.g. it assumes immediate deletion as given by RC).

  * Code that relies on ‘gc’ module is okay to break.
  * Would be nice to provide gc.get_referents(), gc.get_referents().
  * Support __del__ behavior, perhaps limit ability to resurrect objects? Must
    still call __del__ if there is a reference cycle.
  * Must support weakrefs and weakref callbacks.

* It is okay to change internal CPython C code and require extensive source
  code changes
* Ideally, it would only require C99 (or maybe newer ANSI C) and not C++
  runtime

  * bringing C++ runtime into CPython could be problem (cannot intermix C++ runtimes?)
  * C++ doesn’t have stable ABI
  * Some platforms don’t even support C++ runtime

* It will be at least as fast as existing CPython and not use much more memory
* Wishlist item: friendly copy-on-write behavior

  * Some people think fork() is evil and copy-on-write doesn’t matter (not
    useful on platforms other than Linux, BSDs)
  * Others (Instagram) rely heavily on this behavior, dirty pages from RC
    causes issue them

GC Properties
=============

* GC pause time (current Python GC is stop-the-world but collection of younger
  generations is very fast, collection of oldest generation is much slower and
  can have pretty long pause times)
* Memory fragmentation (can be bad with non-moving GC)
* Portability (e.g. write barries that require OS support, userfaultfd(), stack crawling to find roots)
* Support for weakrefs
* Support for powerful finalizers

Which kind of GC can meet contraints above
==========================================

* It could be moving GC but needs to provide stable object pointers for
  external C API
* Incremental GC would be good to reduce pauses•
* Generational GC is good so that GC time is not too long for large heaps
* Concurrent GC seems highly desirable, to make use of multiple-cores for
  speedup

Define why a tracing a GC for CPython would be worthwhile
=========================================================

* Allow finer grain locking (remove big GIL lock)
* RC on multi-core machines is not efficient, even if GIL remains
* A non-RC C API will allow internal optimizations for CPython and also allow
  other Python implementions to provide a common C API.
* Might be faster but a speedup vs RC does not seem a given (given all
  constraints)
* Can be good for attracting research interest in improving CPython, raise
  CPython as a platform for GC research.
* Python is now one of top used languages, even small improvement would be
  noteworthy research result
* Lessons learned can be re-used for future work

  * e.g. how can a compatible C API be provided if a tracing GC is used
  * Some of the solutions for that can be re-used even if initial GC is not
    ready to merge into offical CPython distribution (e.g. breaks C API too
    badly)
  * If prototype GC shows some good properties (runs faster if tested with pure
    Python benchmarks), gives us information on future path.
