================================
Export to a File with Properties
================================

To export the metadata of one Server to import it on other Servers that
run in different environments, we recommend using the option “export
with properties”. This option generates two files:

#. A file that contains the VQL statements of all the elements of the
   database. The values of the parameters that depend on the environment
   are variables instead of the actual values.
   For example, in the ``CREATE DATASOURCE JDBC`` statement the value of
   the parameter ``DATABASEURI`` is
   ``${databases.admin.datasources.jdbc.internet_ds.DATABASEURI}``
   instead of the URI of the database.
#. And, a file that contains the values of the variables. The name of
   this file is the same as the first file but with the extension
   ``.properties``.
   For example,
   
   .. code-block:: none
   
      databases.admin.datasources.jdbc.internet_ds.DATABASEURI= jdbc:mysql://acme:3306/incidents_center

To export to a file with properties you can use:

#. The Administration Tool: in the “Export” (or “Export database”)
   dialog, select the **Include properties file** check box.
#. The ``export`` script: pass the parameter
   ``-P includeProperties=yes``.

Then, you have to edit the values of the properties file to match the
configuration of the target environment: change URLs of the JDBC data
sources, user names and passwords, etc.

In this properties file, the passwords are encrypted. So, when you
change the value of a property that is a password, you need to enter its
encrypted value. Use the command ``ENCRYPT_PASSWORD`` to obtain the
password encrypted.

For example, execute this from the VQL Shell:

 
.. code-block:: sql
   :name: Example usage of the command ENCRYPT_PASSWORD
   :caption: Example usage of the command ENCRYPT\_PASSWORD
   
   ENCRYPT_PASSWORD 'my_password' FOR_PROPERTIES_FILE
   
         
With the ``FOR_PROPERTIES_FILE`` option, this command generates the password with certain characters escaped, as needed in these properties files. If you are going to use the result of this command on a dialog of the administration tool, do not add this option.  


Regarding this process, you only have to change the properties file. Do
not modify the file that contains the VQL statements. Afterward, import
these files into the target Server.

|

To import the VQL file with properties, you can use:

#. The Administration Tool: in the “Import” dialog select the **Use
   properties** check box and enter the path to the properties file in
   the **Properties file** box.
#. The ``import`` script: pass the parameter
   ``--properties-file <path to the properties file>``.

The following table lists the parameters whose value is replaced with a
variable, when exporting them to a file with properties.


.. table:: Parameters whose value is stored as a variable when exporting with properties file (1)
   :name: Parameters whose value is stored as a variable when exporting with properties file (1)
   
   +---------------------+--------------------------+------------------------+
   | Element Type        | Name of the parameter in | VQL Parameter          |
   |                     | the Administration Tool  |                        |
   +=====================+==========================+========================+
   | BAPI data sources   | System name              | SYSTEMNAME             |
   |                     +--------------------------+------------------------+
   |                     | Host                     | HOSTNAME               |
   |                     +--------------------------+------------------------+
   |                     | Client ID                | CLIENTID               |
   |                     +--------------------------+------------------------+
   |                     | System number            | SYSTEMNUMBER           |
   |                     +--------------------------+------------------------+
   |                     | User name                | USERNAME               |
   |                     +--------------------------+------------------------+
   |                     | Password                 | USERPASSWORD and       |
   |                     |                          | USERPASSWORD ENCRYPTED |
   +---------------------+--------------------------+------------------------+
   | Custom data sources | When exporting a Custom  | ROUTE                  |
   |                     | data source, the Server  |                        |
   |                     | replaces with variables  |                        |
   |                     | the parameters of every  |                        |
   |                     | Custom data source       |                        |
   |                     | parameter that represent |                        |
   |                     | a route (LOCAL, HTTP or  |                        |
   |                     | FTP).                    |                        |
   |                     |                          |                        |
   |                     | These variables are the  |                        |
   |                     | same as for the ROUTE    |                        |
   |                     | clause of the Delimited  |                        |
   |                     | File (DF), JSON and XML  |                        |
   |                     | data sources (see        |                        |
   |                     | table `Parameters whose  |                        |
   |                     | value is stored as a     |                        |
   |                     | variable when exporting  |                        |
   |                     | with a properties file   |                        |
   |                     | (2): data routes`_)      |                        |
   +---------------------+--------------------------+------------------------+
   | Delimited file (DF) | Data route (see table    | ROUTE                  |
   | data sources        | `Parameters whose value  |                        |
   |                     | is stored as a variable  |                        |
   |                     | when exporting with a    |                        |
   |                     | properties file (2):     |                        |
   |                     | data routes`_)           |                        |
   +---------------------+--------------------------+------------------------+
   | Google Search data  | Host name                | GSURI                  |
   | sources             +--------------------------+                        |
   |                     | Port                     |                        |
   |                     +--------------------------+------------------------+
   |                     | Proxy host               | PROXY (HOST)           |
   |                     +--------------------------+------------------------+
   |                     | Proxy port               | PROXY (PORT)           |
   |                     +--------------------------+------------------------+
   |                     | Proxy login              | PROXY (USER)           |
   |                     +--------------------------+------------------------+
   |                     | Proxy password           | PROXY (PASSWORD) and   |
   |                     |                          | PROXY (PASSWORD        |
   |                     |                          | ENCRYPTED)             |
   |                     +--------------------------+------------------------+
   |                     | Automatic proxy          | PACURI                 |
   |                     | configuration URI        |                        |
   +---------------------+--------------------------+------------------------+
   | JDBC data sources   | DB URI                   | DATABASEURI            |
   |                     | Login                    | USERNAME               |
   |                     | Password                 | USERPASSWORD and       |
   |                     |                          | USERPASSWORD ENCRYPTED |
   +---------------------+--------------------------+------------------------+
   | JMS Listeners       | VDP database             | VDPDATABASE            |
   |                     +--------------------------+------------------------+
   |                     | VDP user name            | VDPUSER                |
   |                     +--------------------------+------------------------+
   |                     | Destination              | DESTINATION            |
   |                     +--------------------------+------------------------+
   |                     | Reply to                 | REPLYTO                |
   |                     +--------------------------+------------------------+
   |                     | User name                | USER                   |
   |                     +--------------------------+------------------------+
   |                     | Password                 | PASSWORD and           |
   |                     |                          | PASSWORD ENCRYPTED     |
   |                     +--------------------------+------------------------+
   |                     | Specific configuration   | PROPERTIES             |
   |                     | for each JMS Vendor      |                        |
   +---------------------+--------------------------+------------------------+
   | JSON data sources   | Data route (see table    | ROUTE                  |
   |                     | `Parameters whose value  |                        |
   |                     | is stored as a variable  |                        |
   |                     | when exporting with a    |                        |
   |                     | properties file (2):     |                        |
   |                     | data routes`_)           |                        |
   +---------------------+--------------------------+------------------------+
   | LDAP data sources   | Server URI               | URI                    |
   |                     +--------------------------+------------------------+
   |                     | Login                    | USERNAME               |
   |                     +--------------------------+------------------------+
   |                     | Password                 | USERPASSWORD and       |
   |                     |                          | USERPASSWORD ENCRYPTED |
   +---------------------+--------------------------+------------------------+
   | Multidimensional    | XMLA URI                 | XMLAURI                |
   | data sources:       +--------------------------+------------------------+
   | Mondrian, Microsoft | Login                    | USERNAME               |
   | data sources:       +--------------------------+------------------------+
   | SQL Server Analysis | Password                 | USERPASSWORD and       |
   | and Generic         |                          | USERPASSWORD ENCRYPTED |
   +---------------------+--------------------------+------------------------+
   | Multidimensional    | System name              | SYSTEMNAME             |
   | data sources: SAP   +--------------------------+------------------------+
   | BW (BAPI) and SAP   | Host                     | HOSTNAME               |
   | BI (BAPI)           +--------------------------+------------------------+
   |                     | Client ID                | CLIENTID               |
   |                     +--------------------------+------------------------+
   |                     | System number            | SYSTEMNUMBER           |
   |                     +--------------------------+------------------------+
   |                     | Password                 | USERPASSWORD and       |
   |                     |                          | USERPASSWORD ENCRYPTED |
   +---------------------+--------------------------+------------------------+
   | ODBC data sources   | DSN (when the connection | DSN                    |
   |                     | type is DSN)             |                        |
   |                     +--------------------------+------------------------+
   |                     | File Path (when the      | DATABASEURI            |
   |                     | connection type is       |                        |
   |                     | Direct)                  |                        |
   |                     +--------------------------+------------------------+
   |                     | Driver (when the         |                        |
   |                     | connection type is       |                        |
   |                     | Direct)                  |                        |
   |                     +--------------------------+------------------------+
   |                     | Login                    | USERNAME               |
   |                     +--------------------------+------------------------+
   |                     | Password                 | USERPASSWORD and       |
   |                     |                          | USERPASSWORD ENCRYPTED |
   +---------------------+--------------------------+------------------------+
   | XML data sources    | Data route (see table    | DTD                    |
   |                     | `Parameters whose value  |                        |
   |                     | is stored as a variable  |                        |
   |                     | when exporting with a    |                        |
   |                     | properties file (2):     |                        |
   |                     | data routes`_)           |                        |
   |                     +--------------------------+------------------------+
   |                     | Data route (see table    | SCHEMA                 |
   |                     | `Parameters whose value  |                        |
   |                     | is stored as a variable  |                        |
   |                     | when exporting with a    |                        |
   |                     | properties file (2):     |                        |
   |                     | data routes`_)           |                        |
   +---------------------+--------------------------+------------------------+
   | Web service data    | WSDL                     | WSDLURI                |
   | sources             +--------------------------+------------------------+
   |                     | End point (only when the |                        |
   |                     | Specify option button    |                        |
   |                     | is selected)             | ENDPOINT URI           |
   |                     +--------------------------+------------------------+
   |                     | Login                    | USER                   |
   |                     +--------------------------+------------------------+
   |                     | Password                 | PASSWORD               |
   |                     +--------------------------+------------------------+
   |                     | Domain (only when the    | DOMAIN                 |
   |                     | "Authentication" option  |                        |
   |                     | HTTP NTLM is selected)   |                        |
   |                     +--------------------------+------------------------+
   |                     | Proxy host               | PROXY (HOST)           |
   |                     +--------------------------+------------------------+
   |                     | Proxy port               | PROXY (PORT)           |
   |                     +--------------------------+------------------------+
   |                     | Proxy login              | PROXY (USER)           |
   |                     +--------------------------+------------------------+
   |                     | Proxy password           | PROXY (PASSWORD) and   |
   |                     |                          | PROXY (PASSWORD        |
   |                     |                          | ENCRYPTED)             |
   |                     +--------------------------+------------------------+
   |                     | Automatic proxy          | PACURI                 |
   |                     | configuration URI        |                        |
   +---------------------+--------------------------+------------------------+
   | Server              | When exporting the metadata of the entire Server, |
   | configuration       | its settings are also exported in the form of SET |
   |                     | and ``WEBCONTAINER SET`` clauses.                 |
   |                     |                                                   |
   |                     | When you export to a file with properties, the    |
   |                     | values of these clauses are stored in the         |
   |                     | properties file                                   |
   +---------------------+--------------------------+------------------------+
   | Users               | When you export to a file with properties, the    |
   |                     | parameters’ values of the CREATE USER statements  |
   |                     | are stored in the properties file.                |
   +---------------------+--------------------------+------------------------+




.. table:: Parameters whose value is stored as a variable when exporting with a properties file (2): data routes
   :name: Parameters whose value is stored as a variable when exporting with a properties file (2): data routes

   +------------------------+----------------------------+---------------------+
   | Element type           | Name of the parameter in   | VQL Parameter       |
   |                        | the Administration Tool    |                     |
   +========================+============================+=====================+
   | Local data route (the  | Local path                 | ROUTE LOCAL         |
   | file is located in the +----------------------------+---------------------+
   | local file system)     | Decrypt input filter       | DECRYPT PASSWORD    |
   +------------------------+----------------------------+---------------------+
   | HTTP client data       | URL                        | ROUTE HTTP          |
   | route (the file is     +----------------------------+---------------------+
   | retrieved using an     | Login                      | USER                |
   | HTTP connection)       +----------------------------+---------------------+
   |                        | Password                   | PASSWORD            |
   |                        +----------------------------+---------------------+
   |                        | Domain (only when the      | DOMAIN              |
   |                        | NTLM option is selected in |                     |
   |                        | the “Authentication” list) |                     |
   |                        +----------------------------+---------------------+
   |                        | Proxy host                 | PROXY (HOST)        |
   |                        +----------------------------+---------------------+
   |                        | Proxy port                 | PROXY (PORT)        |
   |                        +----------------------------+---------------------+
   |                        | Proxy login                | PROXY (USER)        |
   |                        +----------------------------+---------------------+
   |                        | Proxy password             | PROXY (PASSWORD)    |
   |                        |                            | and PROXY (PASSWORD |
   |                        |                            | ENCRYPTED)          |
   |                        +----------------------------+---------------------+
   |                        | Automatic proxy            | PACURI              |
   |                        | configuration URI          |                     |
   |                        +----------------------------+---------------------+
   |                        |  Decrypt input filter      | DECRYPT PASSWORD    |
   +------------------------+----------------------------+---------------------+
   | FTP / SFTP / FTPS      | Login                      | ROUTE FTP           |
   | Client data route      +----------------------------+                     |
   |                        | Password                   |                     |
   |                        +----------------------------+                     |
   |                        | URL                        |                     |
   |                        +----------------------------+---------------------+
   |                        | Decrypt input filter       | DECRYPT PASSWORD    |
   +------------------------+----------------------------+---------------------+