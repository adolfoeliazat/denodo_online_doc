==============================
Other Version Control Commands
==============================

This section describes commands that are common to all the supported Version
Control Systems.

.. contents::
   :depth: 1
   :local:
   :backlinks: none

VCSDISCARD
==========

``VCSDISCARD`` reverts uncommitted changes in the selected element.

.. code-block:: bnf
   :caption: Syntax of the VCSDISCARD statement
   :name: Syntax of the VCSDISCARD statement

   VCSDISCARD <element-path:literal> [ WITH_GLOBAL_ELEMENTS ]

When discarding changes at the database level on a database which contains
modified elements depending on modified global elements, the operation will be 
aborted unless the optional ``WITH_GLOBAL_ELEMENTS`` clause is included in the
command.

The section :ref:`Discard` of the Administration Guide provides more details about this operation.
