================================
Overriding AbstractCustomWrapper
================================

The following methods may be overridden when extending
``AbstractCustomWrapper``:

-  ``public CustomWrapperInputParameter[] getInputParameters()``.

   This method defines a series of input parameters accepted by the
   Custom wrapper, represented as an array of objects
   ``CustomWrapperInputParameter``. The default implementation of this
   method returns an empty array.

   `Example implementation of the method getInputParameters`_ shows
   an example implementation of this method.

   The ``CustomWrapperInputParameter`` objects have the following
   properties:
 
   - ``name``: the name of the parameter.
   
   - ``mandatory``: if ``true``, you have to provide this parameter when
     querying the wrapper. If ``false``, the parameter is optional.
     
   - ``environment dependent``: if ``true``, this parameter is considered "environment dependent". This means
     that when the VQL of this wrapper is exported to a file with the option *Export environment specific properties separately*,
     the value of this parameter will be a property, not the value.
      
     If ``false``, the value of the parameter will be included in the VQL 
     file.

   - ``description``: description of the parameter. The Administration Tool
     will show this description as a tooltip in the Custom Data Source
     wizard.

   - ``type``: type of the input parameter. To instantiate an object of the
     class ``CustomWrapperInputParameterType`` invoke the appropriate method
     of the ``CustomWrapperInputParameterTypeFactory`` class. This factory
     has the following methods:

      -  ``booleanType(...)`` to create a boolean parameter.

      -  ``integerType()``, ``longType()``, ``floatType()`` and
         ``doubleType()`` to create a number parameter.

      -  ``stringType()``, ``longStringType()`` to create normal and long text
         parameters.

      -  ``enumStringType(...)`` to create an enumeration parameter. An input
         parameter of this type can only have the values of the enumeration.
         The Administration Tool displays a drop-down list so the user can
         select a valid value.

      -  ``hiddenStringType()`` to create a text parameter that contains
         sensitive information that cannot be written to the Virtual DataPort
         log or displayed in the Administration Tool. The Administration Tool
         hides the values of this type of parameters.

      -  ``routeType(...)`` to create a parameter that stores a path to a
         file. The Administration Tool provides a wizard to build valid routes
         for this type of parameters. These parameters are always 
         considered as environment-dependent regardless of the value of the flag 
         ``environment dependent``.

      -  ``loginType()`` and ``passwordType()``. If the custom wrapper has an
         input parameter created with ``loginType()`` and another one created
         with ``passwordType()``, the Administration Tool allows the user to
         enable pass-through credentials when creating a base view over the
         wrapper. In that case, when a user queries the base view, the values
         of these two parameters will be the credentials of user that executed
         the query.

.. code-block:: java
   :caption: Example implementation of the method getInputParameters
   :name: Example implementation of the method getInputParameters

   @Override
   public CustomWrapperInputParameter[] getInputParameters() {
       return new CustomWrapperInputParameter[] {
           new CustomWrapperInputParameter(STRING_PARAM,
                   "A mandatory parameter of type string",
                   true, true,
                   CustomWrapperInputParameterTypeFactory.
                       stringType()),
           new CustomWrapperInputParameter(BOOLEAN_PARAM,
                   "A mandatory parameter of type boolean with 'false'" +        
                   "as the default value",
                   true, true,
                   CustomWrapperInputParameterTypeFactory.
                       booleanType(false)),
           new CustomWrapperInputParameter(INTEGER_PARAM,
                   "An optional parameter of type integer",
                   false, true,
                   CustomWrapperInputParameterTypeFactory.
                       integerType()),
           new CustomWrapperInputParameter(ROUTE_PARAM,
                   "An optional parameter of type route",
                   false, true,
                   CustomWrapperInputParameterTypeFactory.
                       routeType(RouteType.values())) };
   }


-  ``public CustomWrapperConfiguration getConfiguration()``

   This method defines the CUSTOM wrapper’s configuration (details on
   how to configure a CUSTOM wrapper are given in the section :ref:`Configuring a
   Custom Wrapper`). The default implementation of this method returns
   an instance of ``CustomWrapperConfiguration`` with all the available
   configuration parameters set to their default values.
   
-  ``public boolean stop()``

   The Execution Engine invokes this method
   when a query involving this custom wrapper is cancelled. The class
   ``AbstractCustomWrapper`` provides a default implementation of this
   method that does not do anything and only returns ``false``.
   
   If the tasks executed by this wrapper can be cancelled, override this
   method and cancel them. If this wrapper opens any connection to other
   systems, opens files, etc., close these resources from this method.
   
   If this method returns ``true``, the wrapper must guarantee that it
   will finish after this method is invoked. If the wrapper will not
   finish after invoking this wrapper, return ``false``.
   If this wrapper does not overwrite this method, the Execution Engine
   will try to interrupt its execution. Therefore, overwriting this
   method is not mandatory, although recommended.

Custom wrappers can provide support for Insert, Delete and Update
operations. By implementing/overriding the appropriate methods, the
CUSTOM wrapper will be automatically configured so Virtual DataPort knows that
it has insert, delete or update capabilities. The following methods may
be overridden to provide support for IDU operations:

-  ``public int insert(Map<CustomWrapperFieldExpression, Object> insertValues, Map<String, String> inputValues) throws CustomWrapperException``

   This method defines how the CUSTOM wrapper inserts data in its
   associated data source. The wrapper’s input parameters’ values are
   passed as an argument to be used if necessary. The data to be
   inserted is provided as a map between
   ``CustomWrapperFieldExpressions`` and ``Objects``. A
   ``CustomWrapperFieldExpression`` has a name and an optional list of
   sub-fields, in case the field is compound. The method
   ``getStringRepresentation`` of ``CustomWrapperFieldExpression``
   provides a default text version of a field. That can be just the
   field’s name, or something more elaborate like
   ``myfield.myarray[10].myinteger`` (this example represents a compound
   field ``myfield`` with an array-type sub-field ``myarray`` with a
   sub-field ``myinteger``). This method returns the number of
   successfully inserted values. The default implementation does nothing
   and returns ``0``.
   
-  ``public int delete(CustomWrapperConditionHolder condition, Map<String, String> inputValues) throws CustomWrapperException``

   This method defines how the CUSTOM wrapper deletes data from its
   associated data source. Deletion conditions and input parameters’
   values are passed as arguments (see section :ref:`Dealing with
   Conditions` for details about dealing with conditions). The number
   of successfully deleted values is returned. The default
   implementation of this method does nothing and returns ``0``.
   
-  ``public int update(Map<CustomWrapperFieldExpression, Object> updateValues, CustomWrapperConditionHolder condition, Map<String, String> inputValues) throws CustomWrapperException``

   This method defines how the CUSTOM wrapper updates data in its
   associated data source. Update conditions, update values and input
   parameters’ values are provided as arguments (see section :ref:`Dealing
   with Conditions` for details about dealing with conditions and see
   the explanation of the insert method in this section for details on
   update values). The number of successfully updated values is
   returned. The default implementation of this method does nothing and
   returns ``0``.

CUSTOM wrappers can provide support for distributed transactions. By
implementing/overriding the appropriate methods, the CUSTOM wrapper will
be automatically configured so Virtual DataPort knows that it has transactional
capabilities. The following three methods must be overridden for the
CUSTOM wrapper to support distributed transactions:

-  ``public void prepare()``

   This method defines how the CUSTOM
   wrapper performs a *prepare* operation in the context of a
   distributed transaction. The default implementation does nothing.
   
-  ``public void commit()``

   This method defines how the CUSTOM
   wrapper performs a *commit* operation in the context of a distributed
   transaction. The default implementation does nothing.
   
-  ``public void rollback()``

   This method defines how the CUSTOM
   wrapper performs a *rollback* operation in the context of a
   distributed transaction. The default implementation does nothing.
