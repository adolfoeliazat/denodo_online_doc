================================
Supporting Queries with Brackets
================================

Some applications such as Microsoft Power Pivot, in the queries sent to
Denodo, surround the schema and the name of the view with brackets (i.e.
``[`` and ``]``) instead of double quotes. For example, they send a
query like this one:

.. code-block:: sql

   SELECT [customer_360].[customer].* FROM [customer_360].[customer]

Instead of sending one like this one:

.. code-block:: sql

   SELECT "customer_360"."customer".* FROM "customer_360"."customer"

To configure a DSN to allow brackets instead of the double quotes to
surround names of schemas and views, add the following to the connection
settings of the DSN:

.. code-block:: sql

   SET identifierdelimiter=brackets;

To configure this on Windows, open the configuration dialog of the DSN,
click Data source and then, Page 2. Add this to the Connect settings
box.

To configure this on Linux, add this to the property ``ConnSettings``
property of the file used to register the DSN, delete the DSN from the
driver manager and add it again.

The sections :ref:`Set up a DSN on Windows` and :ref:`Register the Denodo ODBC
Driver in unixODBC` explain in detail how to configure a DSN on Windows
and Linux respectively.
