============================
Developing Stored Procedures
============================

.. toctree::
   :hidden:

   using_datetime_values_in_denodo_stored_procedures.rst
   required_libraries_to_develop_stored_procedures.rst
   
Virtual DataPort provides an API to develop custom stored procedures
written in Java.

After developing a stored procedure, you have to import it into the
Virtual DataPort server. The section :doc:`Importing Stored Procedures <../../../administration/stored_procedures/importing_stored_procedures/importing_stored_procedures>` of
the Administration Guide explains how to do it.

The Denodo Platform provides examples of stored procedures and their
source code. They are located in
:file:`{<DENODO_HOME>}/samples/vdp/storedProcedures`. The README file in
this path contains instructions to compile and install them.

We strongly recommend using the Denodo4E plugin for Eclipse to develop
stored procedures. The file ``README`` in
:file:`{<DENODO_HOME>}/tools/denodo4e` explains how to install this plugin.

|

The classes and interfaces for developing stored procedures are located
in the package ``com.denodo.vdb.engine.storedprocedure``

This section describes briefly the use of its main classes. See the `Javadoc documentation <../../../javadoc/index.html?com/denodo/vdb/engine/storedprocedure/package-summary.html>`_ of the API for further details on these classes and their methods.

To create a stored procedure, create a new Java class that extends
``com.denodo.vdb.engine.storedprocedure.AbstractStoredProcedure``

.. note:: Every time a stored procedure is invoked, the Execution Engine
   creates an instance of this class. Therefore, this class may have
   attributes that store the state of the procedure during its execution,
   such as the number of processed rows if the query processes a result
   set.

You have to override the following methods:

-  ``public String getName()``
   
   This method has to return the name of the stored procedure.
      
   It cannot return ``NULL``.

-  ``public String getDescription()``

   This method has to return the description of the stored procedure.
   
   It cannot return ``NULL``.

-  ``public StoredProcedureParameter[] getParameters()``.
   
   This method is invoked once every time the procedure is invoked.

   It has to return an array with the input and output parameters of the
   stored procedure. Each parameter is represented with a
   ``StoredProcedureParameter`` object. A ``StoredProcedureParameter``
   object specifies the name, type, direction (input and/or output) and
   “nullability” (if accepts a ``NULL`` value or not) of a parameter.

   If a parameter is a compound type, an array of
   ``StoredProcedureParameter`` objects must be specified to describe its
   fields.

   This method cannot return ``NULL``. If does not have input nor output
   parameters, it has to return an empty array.

   **Example**: the `Definition of the parameters of a stored procedure
   with compound fields`_ contains a method ``getParameters()`` of a
   stored procedure that has the following parameters:


   -  An input parameter of type ``text``
   -  An output parameter that is an array of registers. These registers
      have two fields: ``field1`` (``text``) and ``field2`` (``int``)

.. code-block:: java
   :caption: Definition of the parameters of a stored procedure with compound fields
   :name: Definition of the parameters of a stored procedure with compound fields

   public StoredProcedureParameter[] getParameters() {

       return new StoredProcedureParameter[] {
               new StoredProcedureParameter("parameter1", Types.VARCHAR, StoredProcedureParameter.DIRECTION_IN),
               new StoredProcedureParameter("compound_field", Types.ARRAY, StoredProcedureParameter.DIRECTION_OUT,
                       true,
                       new StoredProcedureParameter[] {
                               new StoredProcedureParameter("field1", Types.VARCHAR,
                                       StoredProcedureParameter.DIRECTION_OUT),
                               new StoredProcedureParameter("field2", Types.INTEGER,
                                       StoredProcedureParameter.DIRECTION_OUT) }) };
   }

-  ``public void doCall(Object[] inputValues)``
   
   The Execution Engine invokes this method when this procedure is
   called.

   If the procedure has to return results, invoke the method
   ``getProcedureResultSet():StoredProcedureResultSet`` of the
   superclass to obtain a reference to the list of rows that this
   procedure will return. Then invoke the method ``addRow(...)`` of
   ``StoredProcedureResultSet`` for each row you want to return.

   **Example**: let us say that the procedure has a single output
   parameter called ``compound_field`` as the one defined in `Definition
   of the parameters of a stored procedure with compound fields`_. The
   following code snippet builds a row and adds it to the result set:

.. code-block:: java
   :caption: Stored procedures: building a row of a compound type
   :name: Stored procedures: building a row of a compound type

   @Override
   protected void doCall(Object[] inputValues) throws 
               SynchronizeException, StoredProcedureException {
       ...
       ...
       ...
       Object[] row = new Object[1];
       List<Struct> compoundField = new ArrayList<Struct>(values.size());
       List<String> fieldsNames = Arrays.asList("field1", "field2");
       /*
        * 'values' was generated before
        */
       for (Map.Entry<String, Integer> value : values.entrySet()) {
   
           List structValues = Arrays.asList(value.getKey() 
                                             , value.getValue());
           Struct struct = super.createStruct(fieldsNames, structValues);
           compoundField.add(struct);
       }
   
       row[0] = createArray(compoundField, Types.STRUCT);
       getProcedureResultSet().addRow(row);
   }


Optionally, you can override the following methods:

-  ``public void initialize(DatabaseEnvironment environment)``

   The Execution Engine invokes this method once every time a query
   executes this stored procedure. This method can be overridden to
   perform initialization tasks.

   The object `DatabaseEnvironment <../../../javadoc/index.html?com/denodo/vdb/engine/storedprocedure/DatabaseEnvironment.html>`_ provides several methods that can be useful for the execution of the procedure.
      
   -  Execute queries with the methods ``executeQuery`` and
      ``executeUpdate``. 
      
      Some of the signatures of these methods have a parameter ``Object[] parameterValues``. In these methods, you can indicate
      the parameters in the VQL statement with the placeholder ``?`` and then, pass its value in the array. 
     
      To cancel all the queries executed from the stored procedure, execute
      this:
   
      ``((DatabaseEnvironmentImpl) this.environment).cancelQueries()``
      
   -  Execute VQL commands with the method ``executeVqlCommand``.
   
      Some of the signatures of this method have a parameter ``String databaseName`` which indicates the database where the command is executed.
     
      We strongly advise against executing commands that modify the metadata of a production server. 

   -  To obtain a reference to other stored procedures, invoke
      ``lookupProcedure(...)``.
      
   -  To obtain a reference to functions, invoke ``lookupFunction(...)``.
  
   -  To create a new transaction, invoke ``createTransaction(...)``.
 
   -  To add a stored procedure to the current transaction, invoke
      ``joinTransaction(...)``.
  
   -  To write a message to the Server’s log, invoke ``log(...)``.
  
   -  To obtain the value of a Server’s property, invoke
      ``getDatabaseProperty(...)`` The properties that can requested
      are ``CURRENT_USER`` (user name of the current user) and
      ``CURRENT_DATABASE`` (current database).

   You can obtain a reference to ``DatabaseEnvironment`` from other methods
   by invoking ``super.getEnvironment()``.

-  ``public boolean stop()``

   The Execution Engine invokes this method
   when a query involving this stored procedure is cancelled. The class
   ``AbstractStoredProcedure`` provides a default implementation of this
   method that does not do anything and only returns ``false``.

   If the tasks executed by this procedure can be cancelled, override
   this method and cancel them. If this procedure opens any connection to
   other systems, opens files, etc., close these resources from this
   method.

   If this method returns ``true``, the procedure must guarantee that it
   will finish after this method is invoked. If the procedure will not
   finish after invoking this procedure, return ``false``.
  
   If this procedure does not overwrite this method, the Execution Engine
   will try to interrupt the execution of this procedure and the queries
   started by it. Therefore, overwriting this method is not mandatory,
   although recommended.

-  ``public void prepare()``

   The Execution Engine invokes this method when it is about to begin a
   transaction involving this procedure.

-  ``public void commit()``

   The Execution Engine invokes this method to confirm the current
   transaction.

-  ``public void rollback()``

   The Execution Engine invokes this method to undo the current
   transaction.

-  ``public boolean caseSensitiveParameters()``

   If the name of the
   input and output parameters defined by the stored procedure are case
   sensitive, override this method and return ``true``.

-  ``public void log(level, message)``

   Log a message to the Virtual
   DataPort logging system. The message will be added to the log category
   with the name of the class of this procedure. I.e. if the class of the
   procedure is ``com.acme.procedure1``, the message is added to the
   category ``com.acme.procedure1``.

   If this log category is enabled, the message will be logged to
   :file:`{<DENODO_HOME>}/logs/vdp/vdp.log`.
   
   To enable a log category, modify the
   :file:`{<DENODO_HOME>}/conf/vdp/log4j.xml` or invoke the ``LOGCONTROLLER``
   stored procedure. See more about this in the section :doc:`Configuring the
   Logging Engine <../../../administration/appendix/configuring_the_logging_engine/configuring_the_logging_engine>` 
   of the Administration Guide.

|

``AbstractStoredProcedure`` provides other useful methods:

-  ``static java.sql.Struct createStruct(Collection values, int type)``: This
   method creates a ``struct`` SQL-type object. Invoke this method to
   create a register of elements. See an example in `Stored procedures:
   building a row of a compound type`_.
   
-  ``static java.sql.Array createArray(Collection values, int type)``: This
   method returns an array. Invoke this method to create an array of
   elements. The elements of the list have to be of the type
   ``java.sql.Struct``. You can create them by invoking the method
   ``createStruct(...)``. See an example in `Stored procedures: building
   a row of a compound type`_.