=====================
Ownership of Elements
=====================

All the elements of a database have an owner (data sources, views, web services, folders, etc.). The user that creates an element automatically becomes its owner. Later, the ownership of that element can be granted to another user with the command ``CHOWN``.

.. code-block:: bnf
   :caption: Syntax of the statement CHOWN
   :name: Syntax of the statement CHOWN
   
   ; To change the owner of a data source
   CHOWN <user:identifier>
      DATASOURCE { ARN | CUSTOM | DF | ESSBASE | GS | JDBC | JSON | LDAP | ODBC |
                   OLAP | SALESFORCE | SAPBWBAPI | SAPERP | WS | XML } <name:identifier>

   ; To change the owner of a wrapper
   CHOWN <user:identifier>
      WRAPPER { ARN | CUSTOM | DF | ESSBASE | GS | ITP | JDBC | JSON | LDAP | ODBC |
             OLAP | SALESFORCE | SAPBWBAPI | SAPERP | WS | XML } <name:identifier>

   ; To change the owner of a view, stored procedure, web service (SOAP and REST), widget and JMS listener
   CHOWN <user:identifier>
      { VIEW | PROCEDURE | WEBSERVICE | WIDGET | LISTENER JMS } <name:identifier>

   ; To change the owner of a folder
   CHOWN <user:identifier> FOLDER <path:literal>

To change the owner of an element you need to be a global administrator or an administrator of the database to which this element belongs.

|

There are several ways of obtaining the owner of an element:

1. From the administration tool: right-click an element and then, click **Properties**.
#. Using the command ``DESC`` with the parameter ``includeUserPrivileges``. For example:

   .. code-block:: vql
   
      DESC VQL VIEW invoice ('includeUserPrivileges'='yes')
      
   The output will contain a ``CHOWN`` statement.
   
3. Using one of the stored procedures that return information about the elements. E.g. :ref:`GET_VIEWS` returns the owner of the views and :ref:`GET_ELEMENTS` returns the owner of any type of element.