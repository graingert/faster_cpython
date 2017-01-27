.. _optimize-cpython37:

********************************
Projects to optimize CPython 3.7
********************************

See also :ref:`Projects to optimize CPython 3.6 <optimize-cpython36>`.

* **MERGED**: `Issue #26110: LOAD_METHOD and CALL_METHOD
  <http://bugs.python.org/issue26110>`_

  * `Issue #29263: Implement LOAD_METHOD/CALL_METHOD for C functions
    <http://bugs.python.org/issue29263>`_

* `Issue #28158: Implement LOAD_GLOBAL opcode cache
  <http://bugs.python.org/issue28158>`_

  * `Issue #26219: implement per-opcode cache in ceval
    <http://bugs.python.org/issue26219>`_
  * `Issue #10401: Globals / builtins cache
    <http://bugs.python.org/issue10401>`_

* `Free list for single-digits ints <http://bugs.python.org/issue24165>`_
* FASTCALL

  * **PENDING**: tp_fastcall: `Issue #29259: Add tp_fastcall to PyTypeObject:
    support FASTCALL calling convention for all callable objects
    <http://bugs.python.org/issue29259>`_
  * **REJECTED**: tp_fastnew and tp_fastinit

* Convert more C functions to METH_FASTCALL and Argument Clinic

  * `Argument Clinic should understand *args and **kwargs parameters
    <http://bugs.python.org/issue20291>`_
  * `Argument Clinic: Fix signature of optional positional-only arguments
    <http://bugs.python.org/issue29299>`_
  * `_struct module <http://bugs.python.org/issue29300>`_
  * **DONE**: `print() function <http://bugs.python.org/issue29296>`_.
    TODO: convert to Argument Clinic (need ``*args``).
  * `Search for Argument Clinic open issues
    <http://bugs.python.org/issue?%40search_text=&ignore=file%3Acontent&title=&%40columns=title&id=&%40columns=id&stage=&creation=&creator=&activity=&%40columns=activity&%40sort=activity&actor=&nosy=&type=&components=31&versions=&dependencies=&assignee=&keywords=&priority=&status=1&%40columns=status&resolution=&nosy_count=&message_count=&%40group=&%40pagesize=50&%40startwith=0&%40sortdir=on&%40queryname=&%40old-queryname=&%40action=search>`_

* Better bytecode/AST?

  * `Issue #1346238: A constant folding optimization pass for the AST
    <http://bugs.python.org/issue1346238>`_
  * `Issue #11549: Build-out an AST optimizer, moving some functionality out of
    the peephole optimizer
    <http://bugs.python.org/issue11549>`_

