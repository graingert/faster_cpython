********************************
Projects to optimize CPython 3.6
********************************

Complete or almost complete projects
====================================

* **MERGED**: `Wordcode <http://bugs.python.org/issue26647>`_

  * New format of bytecode which will allow to fetch opcode+oparg in a single
    16-bit operation.

* :ref:`FAT Python <fat-python>`: PEP 509, PEP 510, PEP 511, fat and
  fatoptimizer.

  * Owner: Victor Stinner.
  * Speed-up: unknown :-(

* `CPython build options for out-of-the box performance
  <https://bugs.python.org/issue26359>`_

  * Owner: Alecsandru Patrascu
  * Speed-up: unknown.

* **MERGED**: `Change PyMem_Malloc to use PyObject_Malloc allocator?
  <https://bugs.python.org/issue26249>`_

  * Owner: Victor Stinner
  * Speed-up: up to 6% faster in fastpickle of perf.py (up to 22% faster on
    unpickle_list of perf.py, according to Intel run of perf.py).


Micro optimizations
===================

Open
----

* `Speedup method calls 1.2x
  <https://bugs.python.org/issue26110>`_

  * Owner: Yury Selivanov
  * python-dev: `Opcode cache in ceval loop
    <https://mail.python.org/pipermail/python-dev/2016-February/143025.html>`_
  * python-dev: `Speeding up CPython 5-10%
    <https://mail.python.org/pipermail/python-dev/2016-January/142945.html>`_
  * Speedup: up to 21% faster on specific perf.py macro (micro?) benchmarks
    (call_method, call_method_slots, call_method_unknown).
  * Related to `implement per-opcode cache in ceval
    <https://bugs.python.org/issue26219>`_

* `Globals / builtins cache <https://bugs.python.org/issue10401>`_

  * Owner: Antoine Pitrou
  * Speedup: 35% faster on a microbenchmark (LOAD_GLOBAL)

* `ceval: Optimize list[int] (subscript) operation similarly to CPython 2.7
  <https://bugs.python.org/issue26280>`_

  * Owners: Yury Selivanov, Zach Byrne
  * Speed-up: up to 30% faster on microbenchmark.

* `Free list for single-digits ints <https://bugs.python.org/issue24165>`_

  * Owners: Serhiy Storchaka, Yury Selivanov
  * Speedup: up to 18% faster on microbenchmark.

* `Faster bit ops for single-digit positive longs
  <https://bugs.python.org/issue26342>`_

  * Owner: Yury Selivanov
  * Speedup: between 30% and 55% faster on a microbenchmark

:
Closed
------

* [CLOSED, REJECTED] `ceval.c: implement fast path for integers with a single digit
  <https://bugs.python.org/issue21955>`_

  * Owners: many authors :-)
  * Speedup: up to 26% on microbenchmark, unclear status on macrobenchmark.
    Unclear status for types other than int and float (slow-down or not?).



Experimental projects
=====================

* `co_stacksize is calculated from unoptimized code
  <https://bugs.python.org/issue26549>`_

* FASTCALL: avoid creation of temporary tuple/dict when calling C and Python
  functions

  * `Add a new _PyObject_FastCall() function which avoids the creation of a
    tuple or dict for arguments
    <http://bugs.python.org/issue26814>`_
  * property_descr_get:

    * `segfault due to null pointer in tuple
      <http://bugs.python.org/issue26811>`_
    * `Correct reuse argument tuple in property descriptor
      <http://bugs.python.org/issue24276>`_
    * `property_descr_get reuse argument tuple
      <http://bugs.python.org/issue23910>`_

  * `Tuple creation is too slow
    <http://bugs.python.org/issue23507>`_
  * `C implementation of functools.lru_cache
    <http://bugs.python.org/issue14373>`_


* Change bytecode to optimize MAKE_FUNCTION, maybe also CALL_FUNCTION:

  * http://comments.gmane.org/gmane.comp.python.devel/157321
  * See also the optimization on CALL_FUNCTION with keyword parameters, but it
    requires FAT Python: https://bugs.python.org/issue26802#msg263775

* More efficient and/or more compact bytecode?

  * [Python-ideas] `Wordcode v2, moved from -dev
    <https://mail.python.org/pipermail/python-ideas/2016-February/038586.html>`_
  * [Python-ideas] `More compact bytecode
    <https://mail.python.org/pipermail/python-ideas/2016-February/038276.html>`_
  * Owner: Demur Rumed? Serhiy Storchaka?
  * Speed-up: unknown.
  * See also `Speed-up oparg decoding on little-endian machines
    <https://bugs.python.org/issue25823>`_ (speedup: 10% faster on
    microbenchmark)

* New peephole optimizer written in pure Python: `bytecode.peephole_opt
  <https://github.com/haypo/bytecode/blob/master/bytecode/peephole_opt.py>`_,
  requires the PEP 511.

  * Speed-up: probably negligible, and the Python optimizer is much slower
    than the C optimizer.

* `INCA: Inline Caching meets Quickening in Python 3.3
  <https://bugs.python.org/issue14757>`_
