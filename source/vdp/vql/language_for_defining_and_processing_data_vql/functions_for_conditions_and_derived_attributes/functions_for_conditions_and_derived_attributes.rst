===============================================
Functions for Conditions and Derived Attributes
===============================================

Derived attribute functions are used to generate new attributes in the
schema of a view, applying an expression in function of the other
attributes of the view, constants and/or the result of evaluating other
functions. Expressions using these functions can also be used as
operands in the conditions.

A function is defined as an identifier and a list of arguments that can
in turn be constants, fields or other functions.

Virtual DataPort provides predefined functions. The subsections of the
appendix :ref:`Syntax of Condition Functions` lists these functions, their
syntax and examples.

-  Arithmetic functions: see appendix :doc:`/vdp/vql/appendix/syntax_of_condition_functions/arithmetic_functions`.
-  Functions for text processing: see appendix :doc:`/vdp/vql/appendix/syntax_of_condition_functions/text_processing_functions`.
-  Functions for date processing: see appendix :doc:`/vdp/vql/appendix/syntax_of_condition_functions/date_processing_functions`.
-  Functions for processing XML values: see appendix :ref:`XML Processing
   Functions`.
-  Type conversion functions: see appendix :doc:`/vdp/vql/appendix/syntax_of_condition_functions/type_conversion_functions`.
-  Aggregation functions: see appendix :ref:`Aggregation Functions`.
-  Analytic functions (window functions): see appendix :ref:`Analytic
   Functions (Window Functions)`.
-  Other functions: see appendix :ref:`Other Functions`.

You can develop your own functions in Java using the Virtual DataPort API. The section :ref:`Developing Custom Functions` 
of the Developer Guide explains how to do it.

