==========================================
Creating Custom Functions with Annotations
==========================================

A Custom function created with annotations is a Java class with several
annotations, which indicate Virtual DataPort that:

#. The Java class contains the code of a custom function.
#. Which method(s) contain the code that Virtual DataPort will have to
   run when the custom function is invoked.

Each Java class has to contain one and only one custom function, which
may have one or more signatures. For example, in the same class you can
define the function ``function1`` with the signatures
``function1(int)``, ``function1(int, text)``, etc.

To develop custom functions, add the library
:file:`{<DENODO_HOME>}/lib/contrib/denodo-commons-custom.jar` to the classpath
of your development environment.

Then, follow these steps:

#. Create a Java class and annotate it with ``@CustomElement`` (package
   ``com.denodo.common.custom.annotations``), which has the following
   parameters:

   -  ``name``. Name of the function.
   -  ``type``. Type of the function. Its value can be either:

      -  ``CustomElementType.VDPFUNCTION``: if the function is scalar.
      -  Or, ``CustomElementType.VDPAGGREGATEFUNCTION``: if this is an
         aggregation function.

#. Add a method for each signature that you want the function to have.

   For example, to develop the custom function ``function1`` with the
   signatures ``function1(int)``, ``function1(int, text)``, add two
   methods:

   a.

      .. code-block:: java

         @CustomExecutor
         public Integer method1(Integer i) { ... }

   #.

      .. code-block:: java

         @CustomExecutor
         public Integer method2(Integer i, String s) { ... }

   The type of the method parameters has to be a basic Java type (i.e.
   ``String``, ``Integer``, ``Long``, ``Float``, etc.). A parameter cannot
   have a primitive type.

   The methods that represent a signature of the function have to have the
   annotation ``@CustomExecutor`` (package
   ``com.denodo.common.custom.annotations``).

   At runtime, the Server will run the appropriate method depending on the
   parameters passed to the function. For example, if a query invokes the
   function ``function1(int)``, the Server will run the code of the first
   method. If a query invokes the function ``function1(int, text)``, the
   Server will run the code of the second method.

   The class can have any number of methods, but it has to have at least
   one per signature. In addition, these methods have to be in the same
   class, but the custom function can invoke code of other classes.

#. Optionally, you can add the parameter ``syntax`` to the
   ``@CustomExecutor`` annotations. The Administration Tool will use the
   value of this parameter when displaying each signature of the custom
   function to the user (e.g. in the auto-completion feature of the
   expressions editor).

   The value of the ``syntax`` parameter takes preference over the value of
   the ``syntax`` parameter of the ``@CustomParam`` annotations (see
   below). Therefore, use one, or the other.


#. If you want this custom function to be pushed-down to a database, add
   the parameters ``delegationPatterns`` and ``implementation`` to the
   ``@CustomExecutor`` annotations. The section :ref:`Developing Custom
   Functions that Can Be Delegated to a Database` explains in more detail
   how to develop this type of functions.

#. In the methods that have the ``@CustomExecutor`` annotation, you can add
   the annotation ``@CustomParam`` with the ``syntax`` parameter, to each
   parameter of the method.

   The value of the ``syntax`` parameter gives a user-friendly name to the
   parameter of this function’s signature when the autocomplete feature of
   the Administration Tool displays it. If this annotation is not used, the
   syntax of the method will be displayed as *arg1, args2…*

   The value of this parameter will be ignored if the annotation
   ``@CustomExecutor`` of the method has the parameter ``syntax``.

   The value of the parameter ``mandatory`` of the ``@CustomParam``
   annotation is ignored. It is only used when this annotation is used to
   develop Custom Policies.


#. If you are developing an aggregation function, mark the parameters
   that represent aggregation fields with the annotation
   ``@CustomGroup``. The type of these parameters has to be
   ``CustomGroupValue``.

   The ``groupType`` parameter is the type of the elements of the group.

   For example,

   .. code-block:: java

      @CustomExecutor
      public String aggregationFunction(
             @CustomGroup(name="textField", groupType=String.class) CustomGroupValue<String> textField) {

            ...
      }


#. For each method annotated with ``@CustomExecutor`` that meets at least
   one of the following conditions, you have to add another method and
   annotate it with ``@CustomExecutorReturnType``:

   -  The return type of the function is an ``array`` or a ``register``.
   -  Or, the return type of the function depends on the type of the input
      parameters.

   See the section :doc:`Custom Function Return Type <./custom_function_return_type>` for more details about
   this method.


Developing Custom Functions that Can Be Delegated to a Database
===============================================================

This section explains how to develop custom functions that, besides
being executable by the Virtual DataPort server, can be delegated to
JDBC data sources. That means that when possible, instead of executing
the Java code of the custom function, the Server invokes a function of
the database.

To do this, you just have to add the following parameters to the
annotation(s) ``@CustomExecutor`` of the method(s) that implement the
function:

-  ``implementation``: if ``true``, it means that the code of the function
   also can return the proper result. The Server will execute this code
   when the function cannot be delegated to the database.
   If ``false``, it means that the code of the custom function is not valid
   and the Server will never execute it. Therefore, the Server will return
   an error if it cannot delegate the function to the database.


-  ``delegationPatterns``: array of ``DelegationPattern`` annotations that
   represent the configuration of each database that the function can be
   delegated to.
   ``DelegationPattern`` has the following attributes:

   -  ``databaseName``: the name of the database that support this
      function.

      This value corresponds with the value of the parameter
      ``DATABASENAME`` of the ``CREATE DATASOURCE JDBC`` statement that
      creates the JDBC data sources that you want to delegate the function
      to.

   -  ``databaseVersions`` (optional): array of versions of the database
      that support this function. When this parameter is not present, it
      means that the function can be delegated to any version of the
      database indicated in ``databaseName``.
      The values of this array correspond with the values of the parameter
      ``DATABASEVERSION`` of the ``CREATE DATASOURCE JDBC`` statement that
      creates the JDBC data sources that you want to delegate the function
      to.

   -  ``pattern``: expression that will be delegated to the database. This
      parameter is necessary as the function may have a different name and
      signature in each database.

      This string is some sort of regular expression where ``$0``
      represents the first parameter passed to the custom function, ``$1``
      the second, etc.

      If a parameter has a variable number of arguments (“varargs”), you
      can use a pattern such as ``$0[, $i]{1, n}``.

      For example, if the signature of the function is
      ``f1(Integer I, String... param)``, the value of pattern could be
      like:
      ``pattern="FUNCTION_IN_DB($0, $1[, $i]{2, n})"``.
      The *example 2* below shows how to define the pattern when one of the
      parameters has a variable number of arguments.

      In the ``pattern`` parameter, you can only use the ``[``
      character to indicate a variable number of arguments (e.g.
      ``$0[, $i]{1, n}``). This character cannot be used as a literal.

.. note:: You cannot develop custom functions that are delegable to a
   database using name conventions (described in the section :ref:`Creating Custom
   Functions Using Name Conventions`). You have to do it with annotations.

|

.. rubric:: Examples

**Example 1**: custom scalar function that can be delegated to a database

Let us say that we have developed a custom function called ``MAX_VALUE``
that returns the maximum number of three numbers; that Microsoft SQL
server has a function called ``MAXIMUM_N`` that calculates the same and
that Oracle has the same function, but is called ``TOP_N`` in the
versions 10g, and 11g but not in the previous versions [#f1]_.

By adding a few parameters to the annotation ``@CustomExecutor``,
Virtual DataPort will delegate the execution of this function to Oracle
10g and 11g and to any version of SQL Server, whenever is possible.

.. code-block:: java
   :caption: Example of how to annotate a custom function so it can be delegated to a database

   @CustomElement(type = CustomElementType.VDPFUNCTION, name = "MAX_VALUE")
   public class CustomFunctionMaxNumber {
        @CustomExecutor(implementation = true, delegationPatterns = {
                @DelegationPattern(databaseName = "sqlserver",
                                   pattern = "MAXIMUM_N($0, $1, $2)"),
                @DelegationPattern(databaseName = "oracle",
                                   databaseVersions = { "10g", "11g" },
                                   pattern = "TOP_N($0, $1, $2)") })
        public Double Max(
                @CustomParam(name = "arg0") Double arg0,
                @CustomParam(name = "arg1") Double arg1,
                @CustomParam(name = "arg2") Double arg2) {

            /*
            * If the function is not delegated to any of the databases above (e.g. if you use it on a query to
            * base view from Teradata), the execution engine executes this code.
            */
        }
   }

The first ``@DelegationPattern`` annotation indicates that when the
Server can delegate the function to SQL Server (any version), it will
delegate it as the function ``MAXIMUM_3``.

The second ``@DelegationPattern`` indicates that when the Server can
delegate the function to the versions 10g and 11g of Oracle (the
function cannot be delegated to other versions), it will delegate it as
the function ``TOP_3``.

|

**Example 2**: Custom scalar function with a variable number of arguments and that can be delegated to a database

Let us say that we want do develop the same function but with a variable
number of arguments. In this case, you have to define the parameter as
“varargs” (note the ``...`` after the type of the parameter).

.. code-block:: java
   :caption: Example of how to annotate a custom function so it can be delegated to a database (2)

   @CustomElement(type = CustomElementType.VDPFUNCTION, name = "MAX_VALUE")
   public class CustomFunctionMaxNumber {
       @CustomExecutor(implementation = true, delegationPatterns = {
               @DelegationPattern(databaseName = "sqlserver",
                                  pattern = "MAXIMUM_N($0[, $i]{1, n})"),
               @DelegationPattern(databaseName = "oracle",
                                  databaseVersions = { "10g", "11g" },
                                  pattern = "TOP_N($0[, $i]{1, n})") })
       public Double Max(
               @CustomParam(name = "values") Double... arg0) {

            /*
            * If the function is not delegated to any of the databases above (e.g. if you use it on a query to
            * base view from Teradata), the execution engine executes this code.
            */
       }
   }


By adding ``...`` to the type of the parameter, the function
admits one or more values. The ``pattern`` parameter, which defines how
the function is delegated to the database, is ``$0[, $i]{1, n}``. This
means that if you pass the value ``2`` to the function, the Server
will delegate ``TOP_N(2)`` to Oracle. If you pass the parameters
``2, 3, 4``, the Server will delegate ``TOP_3(2, 3, 4)`` to
Oracle.

|

**Example 3**: Custom aggregation function that can be delegated to a database

.. code-block:: java
   :caption: Custom aggregation function that can be delegated to a database

   @CustomElement(type = CustomElementType.VDPAGGREGATEFUNCTION, name = "MAX_AGGR_VALUE")
   public class CustomAggregationFunction {
       @CustomExecutor(implementation = true, delegationPatterns = {
               @DelegationPattern(databaseName = "sqlserver",
                                  pattern = "aggregation_func_sql_server( $0 )"),
               @DelegationPattern(databaseName = "mysql",
                                  pattern = "aggregation_func_mysql( $0 )"),
               @DelegationPattern(databaseName = "oracle",
                                  databaseVersions = { "10g", "11g" },
                                  pattern = "aggregation_func_oracle( $0 )") })
       public String CustomAggregationFunctionSignature1(
               @CustomGroup(name = "field", groupType = String.class)
               CustomGroupValue<String>... textField) {

           /*
            * If the function is not delegated to any of the databases above (e.g. if you use
            * it on a query tobase view from Teradata), the execution engine executes this
            * code.
            */

           return null;
       }
   }

--------------

.. rubric:: Footnotes

.. [#f1] SQL Server and Oracle do not have these functions. We made them
   up for the sake of the example.
