==================
Exporting Metadata
==================

The ``DESC VQL DATABASE`` statement exports all the metadata
from a Virtual DataPort database or from the entire Server. This is very
useful for backup and migration purposes.

The syntax of the statement is:



.. code-block:: bnf
   :caption: Syntax of the DESC VQL DATABASE statement to export a database
   :name: Syntax of the DESC VQL DATABASE statement to export a database

   DESC VQL DATABASE [ <database_name> ]
       (
       '<property name>' = { 'yes' | 'no' } 
       [ ,'<property name>' = { 'yes' | 'no' } ]* 
       )


If the ``DESC VQL DATABASE`` statement includes the parameter
``<database_name>``, all the metadata from that database will be
exported. It will *not* include the metadata of user definitions and
their privileges.

If the ``DESC VQL DATABASE`` statement does not include the
parameter ``<database_name>``, the entire Server’s metadata will be
exported. That is:

-  The metadata from the entire Server, along with their
   ``CREATE DATABASE`` statements.
-  User definitions and privileges.
-  Server settings
-  …

The user that executes this statement needs administrator privileges.

The configuration parameters (``<property name>``) of this command are:

-  ``includeCreateDatabase``. If ``yes`` and you are exporting a
   database, the output will include a ``CREATE DATABASE`` statement to
   create the database that you are exporting.
-  ``includeJars``. If ``yes``, the output will include the jars that
   contain the Java classes associated with extensions. The section
   :doc:`/vdp/developer/developing_extensions/developing_extensions` of the Developer Guide explains what these
   extensions are and how to develop them.
-  ``includeEnvSpecificElements`` and ``includeNonEnvSpecificElements``.
   Use these options to obtain only the VQL statements of the elements
   that depend on the environment
   (``'includeEnvSpecificElements' = 'yes'``), or the statements of the
   elements that are independent of the environment
   (``'includeNonEnvSpecificElements' = 'yes'``)
   
   For example, if a user has created new views and wants to obtain
   their VQL statements without the VQL of the elements that depend on
   the environment (e.g. data sources), she has to use the following
   options:
   ``('includeNonEnvSpecificElements' = 'yes', 'includeEnvSpecificElements' = 'no')``.
   
   The section :ref:`Exporting Environment-Dependent and Independent Elements to 
   Different Files` of the Administration Guide lists which elements are considered 
   dependent on the environment and which are considered independent.
   
   .. note:: These two options are deprecated and may be removed in future versions of the Denodo Platform. Use the `includeProperties` option instead.
   
-  ``includeScanners``. If ``yes``, the output will include the binary files of the ITPilot scanners used
   by the WWW wrappers.

-  ``includeStatistics``. If ``yes``, the output will include the
   statistics gathered for the view. These are the statistics that are
   used during the cost-based optimization process. See more about this
   in the section :ref:`Cost-based Optimization` of the Administration Guide.
   
-  ``includeCustomComponents``. If ``yes``, the output file will include
   the ITPilot custom components used by the existing WWW data sources.
   
-  ``dropElements``. If ``yes`` (default value), the
   output will include a command ``DROP ... CASCADE`` before each
   command ``CREATE``. If ``no``, the result will not include a
   ``DROP`` sentence before each ``CREATE`` one. 
   
-  ``replaceExistingElements``. If ``yes``, in the output, the VQL statements to 
   create elements will be like ``CREATE OR REPLACE ...``.
   
   If ``no`` (default value), the VQL statements will be like
   ``CREATE ...`` (without ``OR REPLACE``). Therefore, if an element of the same type already exists, the command will fail. Usually, this parameter is 
   used in combination with ``dropElements``.

   For example:
   
   .. code-block:: vql

      DESC VQL DATASOURCE JDBC oracle_ds ('dropElements' = 'no', 'replaceExistingElements' = 'yes');
      
   will return ``CREATE OR REPLACE DATASOURCE JDBC oracle_ds...``. When this command is executed, it will create this JDBC data source if it does not exist. If a JDBC data source with this name already exists, it will replace this data source.

   If you execute the following:
      
   .. code-block:: vql

      DESC VQL DATASOURCE JDBC oracle_ds;

   the result will be like 
   
   .. code-block:: vql
   
      DROP DATASOURCE JDBC oracle_ds CASCADE;
      
      CREATE DATASOURCE JDBC oracle_ds...

   When the first command is executed, the data source *and all the views and other elements that depend on it* will be deleted. Then, the data source
   will be created.

-  ``exclude_database_elements``. If ``yes``, the result will not include
   the elements of the database, only the privileges and the database
   configuration. If ``no``, the output file will
   include all elements of the database.


.. rubric:: Example

The following statement exports the database "customer360", including the statistics of the views of this database. 

.. code-block:: vql

   DESC VQL DATABASE customer360  ('includeCreateDatabase' = 'yes', 'includeStatistics' = 'yes', 'dropElements' = 'no', 'replaceExistingElements' = 'yes')

Without the parameter ``'includeStatistics' = 'yes'``, DESC VQL does not return the statistics of the views you export.