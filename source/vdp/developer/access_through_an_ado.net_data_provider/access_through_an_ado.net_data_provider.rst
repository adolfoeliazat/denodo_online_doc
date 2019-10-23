=======================================
Access Through an ADO.NET Data Provider
=======================================

.. toctree::
   :hidden:

   using_kerberos_authentication/using_kerberos_authentication.rst

ADO.Net data providers are software components that allow their users to
develop applications that are independent of the database they want to
use.

Virtual DataPort is compatible with the `.Net Data Provider for PostgreSQL <http://www.npgsql.org/>`_ 
version 2.0. The recommended
versions are 2.0.12, 2.2.0, 2.2.3 and 2.2.7 (you can download it from `its repository <https://github.com/npgsql/npgsql/releases/tag/v2.2.7>`_).

The version 3.x of NPGSQL is supported partially. You can execute SELECT, INSERT, UPDATE and DELETE queries but not inspect the views of the Virtual DataPort databases.

From your application, you can do the following:

#. Create a new object of the class ``NpgsqlConnection``, passing the
   connection string to the constructor. This is what the example of
   :file:`{<DENODO_HOME>}/samples/vdp/vdp-clients-ADO.NET/Program.cs` does.
#. Or, define the ADO.Net provider in the global ``machine.config`` file
   or in the ``.config`` file of the application and from your
   application, request a connection to the Npgsql factory and set the
   appropriate connection string. This option allows you write code that
   is independent of database you are using.

.. code-block:: xml
   :caption: Sample app.config file with the provider definition
   :name: figure-sample-app.config-file-with-the-provider-definition

    <?xml version="1.0" encoding="iso-8859-1" ?>
    <configuration>
        <system.data>
            <DbProviderFactories>
                <add name="Npgsql Data Provider" invariant="Npgsql" support="FF" 
                     description="ADO.Net Data Provider to Denodo"
                     type="Npgsql.NpgsqlFactory, Npgsql, Version=2.0.12.0, Culture=neutral, PublicKeyToken=5d8b90d52f46fda7" />
            </DbProviderFactories>
        </system.data>
    </configuration>


.. code-block:: java
   :caption: Sample ConnectionString to connect to Virtual DataPort

   string connectionString = "Server=acme;" +
        "Port=9996;" +
        "Username=admin;" +
        "Password=admin;" + "Database=admin" +
        "CommandTimeout=80000";

If the name of the database contains non-ASCII characters, they have to
be URL-encoded. For example, if the name of the database is “テスト”,
set the property ``Database`` to ``%E3%83%86%E3%82%B9%E3%83%88``.

The default query timeout of the connection is established in the
``CommandTimeout`` parameter (time in milliseconds). In this connection,
the timeout will be 80 seconds.

The value of the i18n of the connection is set in the ``Database``
parameter of the connection string. The section :ref:`Parameters of the ODBC driver
and their default value` describes this property and its default value.

If SSL/TLS was enabled in the Virtual DataPort server to secure the
communications, add the following parameters to ``ConnectionString``:

.. code-block:: java
  
   "SSL=True;Sslmode=Require"

The page http://www.npgsql.org/doc/connection-string-parameters.html
lists the parameters of the ``ConnectionString``.
