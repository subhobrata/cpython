.. _architecture-overview:

CPython Architecture Overview
=============================

This document presents a brief overview of the main components of the
CPython implementation.  The diagrams below use simple ASCII art so they
can be viewed in any environment.  Future revisions may replace them with
graphical images.

Parser and AST
--------------

The front-end consists of the tokenizer, parser and abstract syntax tree
(AST) builder.  Source text is converted into tokens which are parsed into
an AST representing the program structure.

.. code-block:: text

    Source code
        |
        v
    Tokenizer --\
        |        |
        v        |
      Parser -----/
        |
        v
        AST

Bytecode Compiler
-----------------

The compiler transforms the AST into bytecode.  Bytecode instructions are
stored in code objects which can be executed by the interpreter.

.. code-block:: text

        AST
        |
        v
    Compiler
        |
        v
     Bytecode  ->  ``PyCodeObject``

Interpreter Execution
---------------------

The evaluation loop interprets bytecode one instruction at a time.  Each
running function has a frame object that stores the execution state.

.. code-block:: text

     Bytecode
        |
        v
    +-----------+
    |  Frame    |
    |  Stack    |
    +-----------+
        |
        v
    Evaluation Loop
        |
        v
    Results

Object Model and Memory Management
----------------------------------

CPython uses reference counting with optional cycle detection to manage
memory.  Each object starts with a reference count of one when created.

.. code-block:: text

    PyObject
      |
      v
    +---------------+
    | refcnt = 1    |
    +---------------+
      |
      v
    Used by code -> refcnt++ / refcnt--
      |
      v
    Refcount reaches zero -> object deallocated

Module System
-------------

Modules are loaded via the import machinery.  Importers locate module
sources and loaders create module objects which are cached in
``sys.modules``.

.. code-block:: text

    Import request
        |
        v
    Finder -> Loader -> Module object -> ``sys.modules``

Further Reading
---------------

See the rest of the CPython documentation for detailed descriptions of the
APIs and internals referenced above.
