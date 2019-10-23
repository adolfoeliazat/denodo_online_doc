================================================
Creating Custom Functions Using Name Conventions
================================================

We recommend developing custom functions using annotations. However, it
is also possible to do it following certain conventions for the name of
the class and its methods.

In order to make a Java class recognizable as a custom function, the
name of the class has to match the following rules:

-  ``<FunctionName>VdpFunction`` for condition functions
-  ``<FunctionName>VdpAggregateFunction`` for aggregation functions

.. note:: These conventions are case sensitive. That means that the name of
   the class has to be like ``function1VdpFunction`` and not
   ``function1VDPFUNCTION``.

This way a Java class named ``Concat_SampleVdpFunction`` will be
interpreted as a condition function named ``Concat_Sample``; and a class
named ``Group_Concat_SampleVdpAggregateFunction``, as an aggregate
function named ``Group_Concat_Sample``.

All Java methods implementing the function signatures must have the name
``execute``. The signature associated with each method will be extracted
from its method parameters.

For example, a class named ``Concat_SampleVdpFunction`` with a method
``execute(valueA:String, valueB:String):String`` will generate the
function signature ``CONCAT_SAMPLE(arg1:text, arg2:text)``.

The way to define an arity n in a custom function is with an array as
the last parameter in the method. I.e. a class
``Concat_SampleVdpFunction`` with a method declared as
``public String execute(String [] inputs)``.

A custom function has to define a method named ``executeReturnType``
with the same parameters as the associated ``execute`` method if:

-  The return type of the function is an array or a register.
-  Or, the return type of the function depends on the type of the input
   parameters.

See the section :doc:`Custom Function Return Type <./custom_function_return_type>` for more details about
this method.
