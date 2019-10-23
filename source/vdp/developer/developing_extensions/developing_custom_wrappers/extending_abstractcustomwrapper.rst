===============================
Extending AbstractCustomWrapper
===============================

The simplest way to create a new Custom wrapper is to implement the
following two methods of the abstract class
``com.denodo.vdb.engine.customwrapper.AbstractCustomWrapper``:

-  ``public CustomWrapperSchemaParameter[] getSchemaParameters(Map<String, String> inputValues)``

   This method has to return the output schema of the Custom wrapper,
   which is the schema of the data obtained by querying the wrapper.
   You can develop Custom wrappers whose output schema depends on the
   values of their input parameters. To do this, implement the method
   ``getInputParameters`` (see section :ref:`Overriding
   AbstractCustomWrapper`) to define the input parameters of the
   wrapper. Then, the parameter ``inputValues`` will contain the values
   for these parameters.
   The output schema is represented as an array of
   ``CustomWrapperSchemaParameters`` objects, which represent fields of
   the schema. A ``CustomWrapperSchemaParameter`` has a name, a type and
   several other properties (as mandatoriness, nullability, etc.), and
   an optional array of other ``CustomWrapperSchemaParameters`` in case
   the represented field is compound. In a ``CustomWrapperSchemaParameter``
   you can also specify the "source type properties" of the fields returned by the wrapper. The 
   section :ref:`Source Type Properties` of the Administration Guide explains why defining these properties may be beneficial to the performance of the queries.

-  ``public void run( CustomWrapperConditionHolder condition, List<CustomWrapperFieldExpression> projectedFields, CustomWrapperResult result, Map<String, String> inputValues)``

   Virtual
   DataPort invokes this method when a user queries the wrapper.
   Depending on the wrapper’s configuration (see section :ref:`Configuring a
   Custom Wrapper`), the ``condition`` and ``projectedFields``
   arguments may be taken into account (conditions will be explained in
   section :ref:`Dealing with Conditions`). These two parameters encapsulate
   the conditions and the list of projected fields of the query to the
   wrapper.
   ``inputValues`` contains the input parameters of the wrapper. It only
   contains a textual representation of the values and does not contain
   information about their type. The method
   ``getInputParameterValue(String name)`` returns an instance of
   ``CustomWrapperInputParameterValue`` that provides full information
   about the parameter value and its type.
   Usually, an implementation of the method ``run`` involves analyzing
   the passed conditions, projected fields and input values, querying
   the wrapper’s data source and returning the retrieved data to Virtual
   DataPort. This is done by invoking the method ``addRow`` of the
   ``result`` argument. The arguments of ``addRow`` are an array of
   ``Objects`` and a list of projected fields. The array passed to
   ``addRow`` must contain a series of ``Objects`` matching the list of
   projected fields. Also, the types of the Objects must match the
   schema defined in the method ``getSchemaParameters``
   
   See the Javadoc of the method `getParameterClass() <../../../javadoc/com/denodo/vdb/engine/customwrapper/CustomWrapperSchemaParameter.html#getParameterClass-->`_ of the class "CustomWrapperSchemaParameter" to check the appropriate Java class for a "CustomWrapperSchemaParameter" according to its type.

   To obtain the schema of the current execution of the custom wrapper, invoke the method `getSchema() <../../../javadoc/com/denodo/vdb/engine/customwrapper/CustomWrapperResult.html#getSchema-->`_ of the object "CustomWrapperResult" passed as parameter of the method "run()".

   This is the schema returned by the method ``getSchemaParameters()`` of the custom wrapper. Invoking this method can be useful in custom wrappers whose output (i.e. what the method ``getSchemaParameters()`` returns) changes depending on the input parameters of the wrapper.

By implementing these two methods, you can create a Custom wrapper.
However, in some scenarios you may need to override some methods of the
``AbstractCustomWrapper`` class to access more advanced features. The
next section lists these methods and their default behavior.

There are other useful methods in ``AbstractCustomWrapper`` like
``log(int level, String logMessage)`` to log information to the server
logging files or ``getCustomWrapperPlan()`` to access the execution plan
and add some information to the trace (see section :ref:`Updating the Custom
Wrapper Plan`).
