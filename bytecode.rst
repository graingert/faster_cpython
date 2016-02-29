+++++++++++++++
Python bytecode
+++++++++++++++

.. _cpython-peephole:

CPython peephole optimizer
==========================

Implementation: Python/peephole.c

Optmizations:

* :ref:`Constant folding <const-fold>`
* :ref:`Dead code elimination <dead-code>`
* Some other optimizations more specific to the bytecode, like removal
  of useless jumps and optimizations on conditional jumps

Latest enhancement::

    changeset:   68375:14205d0fee45
    user:        Antoine Pitrou <solipsis@pitrou.net>
    date:        Fri Mar 11 17:27:02 2011 +0100
    files:       Lib/test/test_peepholer.py Misc/NEWS Python/peephole.c
    description:
    Issue #11244: The peephole optimizer is now able to constant-fold
    arbitrarily complex expressions.  This also fixes a 3.2 regression where
    operations involving negative numbers were not constant-folded.

Compiler enhancement to reduce the number of stupid jumps::

    changeset:   92460:c0ca9d32aed4
    user:        Antoine Pitrou <solipsis@pitrou.net>
    date:        Thu Sep 18 03:06:50 2014 +0200
    files:       Lib/test/test_dis.py Misc/NEWS Python/compile.c
    description:
    Closes #11471: avoid generating a JUMP_FORWARD instruction at the end
    of an if-block if there is no else-clause.

    Original patch by Eugene Toder.

Should be rewritten as an :ref:`AST optimizer <ast-optimizers>`.


Bytecode
========

* `byteplay <https://github.com/serprex/byteplay>`_:
  `byteplay documentation <https://wiki.python.org/moin/ByteplayDoc>`_
  (see also the `old byteplay hosted on Google Code
  <http://code.google.com/p/byteplay/>`_)
* `diving-into-byte-code-optimization-in-python
  <http://www.slideshare.net/cjgiridhar/diving-into-byte-code-optimization-in-python>`_
* `BytecodeAssembler <http://pypi.python.org/pypi/BytecodeAssembler>`_

