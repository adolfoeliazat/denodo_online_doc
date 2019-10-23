=================
Stored Procedures
=================

.. toctree::
   :hidden:

   importing_stored_procedures/importing_stored_procedures.rst
   executing_stored_procedures/executing_stored_procedures.rst
   use_of_stored_procedures_in_creating_views/use_of_stored_procedures_in_creating_views.rst


Virtual DataPort provides an API to develop custom stored procedures
written in Java.

This section explains how to import them into the Server.

The section :doc:`Developing Stored Procedures <../../developer/developing_extensions/developing_stored_procedures/developing_stored_procedures>`
of the Developer Guide
explains how to develop them.

The :file:`{<DENODO_HOME>}/samples/vdp/storedProcedures` path contains several
examples of stored procedures. The ``README`` file in this path
describes what they do and contains instructions to compile and install
them. The stored procedure ``CalculateAvgRevenue``, included in these
examples, will be used in this section.

To follow this example, you must:

#. Execute the script
   :file:`{<DENODO_HOME>}/samples/vdp/storedProcedures/scripts/compile_storedprocedures`
#. Load the following Jar file using the Extension Management option:
   :file:`{<DENODO_HOME>}/samples/vdp/storedProcedures/target/jars/denodo-demo-storedprocedures.jar`. This
   option is located in the menu **File** > **Extension Management**. See section
   :ref:`Importing Extensions` for more information.
