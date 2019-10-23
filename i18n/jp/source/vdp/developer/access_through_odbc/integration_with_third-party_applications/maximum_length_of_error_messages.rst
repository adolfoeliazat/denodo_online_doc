================================
Maximum Length of Error Messages
================================

There are applications that fail when the length of an error message
exceeds a certain length. To work around this problem, Virtual DataPort
provides options to set a limit on the length of the error messages. To
do this, you have two options:

#. Configure the ODBC interface of the Server to limit the length of the
   error messages.
   
   To do this, execute this command from the VQL Shell using an
   administrator account:

.. code-block:: sql

   SET 'com.denodo.vdb.vdbinterface.server.odbc.errorMaxLength' = '200';

   The statement above sets the limit to 200 characters.

   This change affects *all* the ODBC clients.

2. Configure an individual connection:

   a. For connections established using the Denodo ODBC driver, add the
      parameter ``errorMaxLength`` to the “Connect settings” of the DSN.
      For example:

.. code-block:: sql
      
   SET ErrorMaxLength TO 200;
   SET QueryTimeout TO 3600000;
   
..

   b. For connections established using the ADO.Net provider (see section
      :ref:`Access Through an ADO.NET Data Provider`), add the parameter
      ``errorMaxLength`` after the name of the database. For example:

.. code-block:: sql

   support?errorMaxLength=200

The option set in the connection (option 2) overrides the option set in
the Server (option 1). For example, if you configure the ODBC interface
to limit the length of the error messages to 150 and in the DSN, to 100,
the limit will be 100.
