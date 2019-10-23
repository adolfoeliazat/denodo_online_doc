==================================
Naming Conventions and Annotations
==================================

The following naming conventions allow the definition of some custom
functions without the need of Java annotations, even if it is
recommended to use them. All the names used in the naming conventions
are case sensitive.



To make a Java class to recognizable as a custom function without Java
annotations, its name must match the following pattern:

-  <FunctionName> + “ItpFunction”



This way, a Java class named *Concat\_SampleItpFunction* will be
interpreted as a function named *Concat\_Sample*.



All Java methods implementing the function signatures must have the name
*execute*. The signature associated with each method will be extracted
from the Java method parameters. For example, a class named
*Concat\_SampleItpFunction* with a method *execute(valueA:String,
valueB:String):String* will generate the function signature
*CONCAT\_SAMPLE(arg1:text, arg2:text)*.



To define a parameter with arity *n* in a custom function, the last
parameter has to be an array. E.g., the class
*Concat\_SampleItpFunction* with a method declared as *public String
execute(String … inputs)*.



Custom functions which return type depends on the type of their input
parameters or return an array or register, can define an additional
method with equivalent signature to the one of *execute*. This
additional method must be named *executeReturnType*. The definition of
this method is optional. If it is not present, the *execute* method will
be called and the return type will be obtained from the results of the
execution. The advantage of defining the method *executeReturnType* is
that in some cases calculating the return type is much less complex and
time consuming than actually executing the function, thus by providing
this method the performance is improved.



Naming conventions only cover a subset of all the possible custom
functions. In order to prevent the limitations using naming conventions
it is recommended to use the Java annotations provided by Denodo in the
jar file :file:`{<DENODO_HOME>}/lib/contrib/denodo-custom.jar`. These
annotations are:


-  ``com.denodo.common.custom.annotations.CustomElement``. Class
   annotation used to define the class as a custom function. The annotation
   requires the parameters

   -  ``name``: name of the custom function.
   -  ``type``: In ITPilot it must be CustomElementType.ITPFUNCTION.

-  ``com.denodo.common.custom.annotations.CustomExecutor``. Method
   annotation used to specify the method as a function signature. This
   method will be executed when using the function with the appropriate
   arguments. The annotation has an optional variable *syntax* in order to
   specify the syntax of the function signature when presenting it to the
   user at the Wrapper Generation Tool.

-  ``com.denodo.common.custom.annotations.CustomExecutorReturnType``.
   Method annotation used to specify the method as the one used to compute
   the return type of a function signature before executing a query.

-  ``com.denodo.common.custom.annotations.CustomParam``. Parameter
   annotation with the parameter *name*, used to make more user friendly
   the auto generated syntax description of the signature. If this
   annotation is not used, the syntax will use the names *arg1, arg2, etc.*
   to represent the input parameters.
