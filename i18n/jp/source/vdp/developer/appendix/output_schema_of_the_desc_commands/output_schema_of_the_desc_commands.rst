==================================
Output Schema of the DESC Commands
==================================

There are two ways of describing an object from the JDBC client:

1. With a ``DESC VQL`` statement. It returns the VQL statements that create an element. For example:

.. code-block:: none

   -- Obtains the VQL of a view and its dependencies.
   DESC VQL VIEW incident; 
   
   -- Obtains the VQL of a JDBC data source.
   DESC VQL DATASOURCE JDBC ds_jdbc_phone_inc; 

..

   The section :ref:`Describing Catalog Elements` of the VQL Guide lists all the modifiers of the DESC VQL statement.
   
2. With a ``DESC`` statement. It returns information about an element in a structured way.

   The table below lists the output of the DESC commands when executed from a JDBC or an ODBC client. When these commands are executed from the “VQL Shell” of the Administration Tool, they return information that should only be used for debugging purposes and may change in the future.
   
   The output of these commands does not include any password for security purposes.
   
   .. note:: When you execute these commands from the “VQL Shell” of the Administration Tool instead of a JDBC client, they return information that should only be used for debugging purposes and can change between updates of the Denodo Platform.

.. table:: Output schema of the ``DESC`` command depending on its parameters
   :name: Output schema of the ``DESC`` command depending on its parameters
                                                       
   +---------------------------------------------------+---------------------------------------------------+
   | Statement                                         | Column of the Result                              |
   +===================================================+===================================================+
   | DESC DATABASE <database name>                     | name                                              |
   |                                                   |                                                   |
   |                                                   | description                                       |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE ARN <data source name>            | name                                              |
   |                                                   |                                                   |
   | (deprecated)                                      | aracne server route                               |
   |                                                   |                                                   |
   |                                                   | user name                                         |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE CUSTOM <data source name>         | name                                              |
   |                                                   |                                                   |
   |                                                   | class name                                        |
   |                                                   |                                                   |
   |                                                   | classpath                                         |
   |                                                   |                                                   |
   |                                                   | jars                                              |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE DF <data source name>             | name                                              |
   |                                                   |                                                   |
   |                                                   | begin delimiter                                   |
   |                                                   |                                                   |
   |                                                   | end delimiter                                     |
   |                                                   |                                                   |
   |                                                   | column delimiter                                  |
   |                                                   |                                                   |
   |                                                   | end of line delimiter                             |
   |                                                   |                                                   |
   |                                                   | header pattern                                    |
   |                                                   |                                                   |
   |                                                   | tuple pattern                                     |
   |                                                   |                                                   |
   |                                                   | data route                                        |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE ESSBASE <data source name>        | name                                              |
   |                                                   |                                                   |
   |                                                   | database version                                  |
   |                                                   |                                                   |
   |                                                   | uri                                               |
   |                                                   |                                                   |
   |                                                   | user name                                         |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE GS <data source name>             | name                                              |
   |                                                   |                                                   |
   |                                                   | google mini server route                          |
   |                                                   |                                                   |
   |                                                   | proxy host                                        |
   |                                                   |                                                   |
   |                                                   | proxy port                                        |
   |                                                   |                                                   |
   |                                                   | proxy user                                        |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE JDBC <data source name>           | name                                              |
   |                                                   |                                                   |
   |                                                   | database uri                                      |
   |                                                   |                                                   |
   |                                                   | driver class name                                 |
   |                                                   |                                                   |
   |                                                   | user name                                         |
   |                                                   |                                                   |
   |                                                   | classpath                                         |
   |                                                   |                                                   |
   |                                                   | database name                                     |
   |                                                   |                                                   |
   |                                                   | database version                                  |
   |                                                   |                                                   |
   |                                                   | ping query                                        |
   |                                                   |                                                   |
   |                                                   | initial size                                      |
   |                                                   |                                                   |
   |                                                   | max active                                        |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE JSON <data source name>           | name                                              |
   |                                                   |                                                   |
   |                                                   | data route                                        |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE LDAP <data source name>           | name                                              |
   |                                                   |                                                   |
   |                                                   | uri                                               |
   |                                                   |                                                   |
   |                                                   | login                                             |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE ODBC <data source name>           | name                                              |
   |                                                   |                                                   |
   |                                                   | dsn                                               |
   |                                                   |                                                   |
   |                                                   | database uri                                      |
   |                                                   |                                                   |
   |                                                   | driver class name                                 |
   |                                                   |                                                   |
   |                                                   | user name                                         |
   |                                                   |                                                   |
   |                                                   | classpath                                         |
   |                                                   |                                                   |
   |                                                   | database name                                     |
   |                                                   |                                                   |
   |                                                   | database version                                  |
   |                                                   |                                                   |
   |                                                   | ping query                                        |
   |                                                   |                                                   |
   |                                                   | initial size                                      |
   |                                                   |                                                   |
   |                                                   | max active                                        |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE OLAP <data source name>           | name                                              |
   |                                                   |                                                   |
   |                                                   | database name                                     |
   |                                                   |                                                   |
   |                                                   | database version                                  |
   |                                                   |                                                   |
   |                                                   | xmla uri                                          |
   |                                                   |                                                   |
   |                                                   | user name                                         |
   |                                                   |                                                   |
   |                                                   | max active                                        |
   |                                                   |                                                   |
   |                                                   | initial size                                      |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE SALESFORCE <data source name>     | name                                              |
   |                                                   |                                                   |
   |                                                   | database name                                     |
   |                                                   |                                                   |
   |                                                   | base url                                          |
   |                                                   |                                                   |
   |                                                   | api version                                       |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE SAPBWBAPI <data source name>      | name                                              |
   |                                                   |                                                   |
   |                                                   | system name                                       |
   |                                                   |                                                   |
   |                                                   | host name                                         |
   |                                                   |                                                   |
   |                                                   | client id                                         |
   |                                                   |                                                   |
   |                                                   | system number                                     |
   |                                                   |                                                   |
   |                                                   | user name                                         |
   |                                                   |                                                   |
   |                                                   | read block size                                   |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE SAPERP <data source name>         | name                                              |
   |                                                   |                                                   |
   |                                                   | system name                                       |
   |                                                   |                                                   |
   |                                                   | host name                                         |
   |                                                   |                                                   |
   |                                                   | client id                                         |
   |                                                   |                                                   |
   |                                                   | system number                                     |
   |                                                   |                                                   |
   |                                                   | user name                                         |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE WS <data source name>             | name                                              |
   |                                                   |                                                   |
   |                                                   | wsdl route                                        |
   |                                                   |                                                   |
   |                                                   | authentication type:                              |
   |                                                   |                                                   |
   |                                                   |    empty = No authentication                      |
   |                                                   |                                                   |
   |                                                   |    ``1`` = HTTP Basic authentication              |
   |                                                   |                                                   |
   |                                                   |    ``2`` = HTTP Digest authentication             |
   |                                                   |                                                   |
   |                                                   |    ``3`` = NTLM authentication                    |
   |                                                   |                                                   |
   |                                                   |    ``10`` = WSS Basic authentication              |
   |                                                   |                                                   |
   |                                                   |    ``11`` = WSS Digest authentication             |
   |                                                   |                                                   |
   |                                                   | authentication user                               |
   |                                                   |                                                   |
   |                                                   | proxy host                                        |
   |                                                   |                                                   |
   |                                                   | proxy port                                        |
   |                                                   |                                                   |
   |                                                   | proxy user                                        |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC DATASOURCE XML <data source name>            | name                                              |
   |                                                   |                                                   |
   |                                                   | data route                                        |
   |                                                   |                                                   |
   |                                                   | schema route                                      |
   |                                                   |                                                   |
   |                                                   | dtd route                                         |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC MAP SIMPLE <map name>                        | key                                               |
   |                                                   |                                                   |
   | DESC MAP I18N <map name>                          | value                                             |
   |                                                   |                                                   |
   |                                                   | (One row for each entry of the map)               |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC PROCEDURE <procedure name>                   | name                                              |
   |                                                   |                                                   |
   |                                                   | type (parameter type)                             |
   |                                                   |                                                   |
   |                                                   | direction = ``IN`` (input parameter) or ``OUT``   |
   |                                                   | (output parameter)                                |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC ROLE <role name>                             | description                                       |
   |                                                   |                                                   |
   |                                                   | roles: name of a role assigned to this role. If a |
   |                                                   | role has other rows assigned, this command        |
   |                                                   | returns a row for each role assigned to this      |
   |                                                   | role.                                             |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC SESSION                                      | database                                          |
   |                                                   |                                                   |
   |                                                   | user                                              |
   |                                                   |                                                   |
   |                                                   | i18n                                              |
   |                                                   |                                                   |
   |                                                   | activeTransaction: ``true`` if this connection    |
   |                                                   | has started a transaction. ``false`` otherwise.   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC TYPE <type name>                             | field                                             |
   |                                                   |                                                   |
   |                                                   | type: if the type is complex, it returns a row    |
   |                                                   | for each component of the type.                   |
   |                                                   | If it is ``simple`` (e.g. ``text``),              |
   |                                                   | only the name of the type.                        |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC USER <user name>                             | name                                              |
   |                                                   |                                                   |
   |                                                   | description                                       |
   |                                                   |                                                   |
   |                                                   | admin: ``true`` if the user is                    |
   |                                                   | an Administrator. ``false`` otherwise.            |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC VIEW <view name>                             | fieldname                                         |
   |                                                   |                                                   |
   |                                                   | fieldtype: a row for each field of the view.      |
   |                                                   |                                                   |
   |                                                   | fieldTypeCode                                     |
   |                                                   |                                                   |
   |                                                   | fieldPrecision                                    |
   |                                                   |                                                   |
   |                                                   | fieldDecimals                                     |
   |                                                   |                                                   |
   |                                                   | fieldRadix                                        |
   |                                                   |                                                   |
   |                                                   | isPrimaryKey                                      |
   |                                                   |                                                   |
   |                                                   | isNotNull                                         |
   |                                                   |                                                   |
   |                                                   | isUnique                                          |
   |                                                   |                                                   |
   |                                                   | isAutoincrement                                   |
   |                                                   |                                                   |
   |                                                   | isGeneratedColumn                                 |
   |                                                   |                                                   |
   |                                                   | The fields fieldTypeCode, fieldPrecision,         |
   |                                                   | fieldDecimals and fieldRadix are the values       |
   |                                                   | of the “Source type properties” of                |
   |                                                   | the fields of the view. Their value               |
   |                                                   | correspond with the properties                    |
   |                                                   | ``SOURCETYPEID``, ``SOURCETYPESIZE``,             |
   |                                                   | ``SOURCETYPEDECIMALS`` and ``SOURCETYPERADIX``    |
   |                                                   | respectively of the CREATE TABLE / VIEW           |
   |                                                   | statement that created the view.                  |
   |                                                   |                                                   |
   |                                                   | See more about these properties in                |
   |                                                   | the section :ref:`Viewing the Schema of a Base    |
   |                                                   | View` of the Administration Guide and in the      |
   |                                                   | section :ref:`JDBC Wrappers` of the VQL Guide.    |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC [ SOAP | REST ] WEBSERVICE <name>            | wsname                                            |
   |                                                   |                                                   |
   |                                                   | wstypes                                           |
   |                                                   |                                                   |
   |                                                   | operationtype: ``1`` = SELECT, ``10`` = INSERT,   | 
   |                                                   | ``11`` = UPDATE, ``12`` = DELETE                  |
   |                                                   |                                                   |
   |                                                   | operationname                                     |
   |                                                   |                                                   |
   |                                                   | fieldname                                         |
   |                                                   |                                                   |
   |                                                   | fieldtype                                         |
   |                                                   |                                                   |
   |                                                   | fielddirection: input or output parameter         |
   |                                                   |                                                   |
   |                                                   | This statement returns a row for each parameter   |
   |                                                   | of the Web Service.                               |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER ARN <wrapper name>                   | name                                              |
   |                                                   |                                                   |
   | (deprecated)                                      | type = ``arn``                                    |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | handler name                                      |
   |                                                   |                                                   |
   |                                                   | filter main terms                                 |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER CUSTOM <wrapper name>                | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``custom``                                 |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | parameters                                        |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER DF <wrapper name>                    | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``df``                                     |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER ESSBASE <wrapper name>               | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``esbase``                                 |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | server name                                       |
   |                                                   |                                                   |
   |                                                   | application name                                  |
   |                                                   |                                                   |
   |                                                   | cube name                                         |
   |                                                   |                                                   |
   |                                                   | MDX sentence                                      |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER GS <wrapper name>                    | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``gs``                                     |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | client                                            |
   |                                                   |                                                   |
   |                                                   | languages                                         |
   |                                                   |                                                   |
   |                                                   | site collections                                  |
   |                                                   |                                                   |
   |                                                   | number of key match                               |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER ITP <wrapper name>                   | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``itp``                                    |
   |                                                   |                                                   |
   |                                                   | creation date                                     |
   |                                                   |                                                   |
   |                                                   | maintenance                                       |
   |                                                   |                                                   |
   |                                                   | old                                               |
   |                                                   |                                                   |
   |                                                   | sequence                                          |
   |                                                   |                                                   |
   |                                                   | substitutions                                     |
   |                                                   |                                                   |
   |                                                   | model content                                     |
   |                                                   |                                                   |
   |                                                   | scanners                                          |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER { JDBC | ODBC } <wrapper name>       | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``jdbc`` or ``odbc``                       |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | relation name                                     |
   |                                                   |                                                   |
   |                                                   | sql sentence                                      |
   |                                                   |                                                   |
   |                                                   | aliases                                           |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER JSON <wrapper name>                  | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``json``                                   |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | tuple root                                        |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER LDAP <wrapper name>                  | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``ldap``                                   |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | object classes                                    |
   |                                                   |                                                   |
   |                                                   | recursive search                                  |
   |                                                   |                                                   |
   |                                                   | ldap expression                                   |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER OLAP <wrapper name>                  | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``olap``                                   |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | catalog name                                      |
   |                                                   |                                                   |
   |                                                   | schema name                                       |
   |                                                   |                                                   |
   |                                                   | cube name                                         |
   |                                                   |                                                   |
   |                                                   | mdx sentence                                      |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER SALESFORCE <wrapper name>            | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``salesforce``                             |
   |                                                   |                                                   |
   |                                                   | data source database                              |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | object name                                       |
   |                                                   |                                                   |
   |                                                   | soql sentence                                     |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER SAPBWBAPI <wrapper name>             | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``sapbwbapi``                              |
   |                                                   |                                                   |
   |                                                   | schema name                                       |
   |                                                   |                                                   |
   |                                                   | cube name                                         |
   |                                                   |                                                   |
   |                                                   | mdx sentence                                      |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER SAPERP <wrapper name>                | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``saperp``                                 |
   |                                                   |                                                   |
   |                                                   | schema name                                       |
   |                                                   |                                                   |
   |                                                   | bapi name                                         |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER WS <wrapper name>                    | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``ws``                                     |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | service name                                      |
   |                                                   |                                                   |
   |                                                   | port name                                         |
   |                                                   |                                                   |
   |                                                   | operation name                                    |
   |                                                   |                                                   |
   |                                                   | input message                                     |
   |                                                   |                                                   |
   |                                                   | output message                                    |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
   | DESC WRAPPER XML <wrapper name>                   | name                                              |
   |                                                   |                                                   |
   |                                                   | type = ``xml``                                    |
   |                                                   |                                                   |
   |                                                   | data source name                                  |
   |                                                   |                                                   |
   |                                                   | tuple root                                        |
   |                                                   |                                                   |
   |                                                   | output schema                                     |
   |                                                   |                                                   |
   +---------------------------------------------------+---------------------------------------------------+
