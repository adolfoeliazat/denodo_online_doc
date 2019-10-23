===========================================
Use of the Import/Export Scripts for Backup
===========================================

The import and export scripts are available in the ``tools/scheduler``
directory of the platform. They are provided in two versions:
``import.sh`` and ``export.sh`` (for Linux systems) and ``import.bat``
and ``export.bat`` (for Windows systems).



Export
------

The export script allows for all metadata and configuration of a
Scheduler server to be exported to a zip file. The data exported is the
same as the obtained with the equivalent option of the administration
tool (see section :ref:`Scheduler Server Configuration`).

 

The format in which the script is invoked is as follows:

 
.. code-block:: bnf

   export -h host -p port -l login -P password [ -vdpauth ][ -L <project1, project2...> ]
   [ -dependencies ] [ -config ] [ -drivers ] [ -plugins ] [ -exportWithProperties ] [ -permissions ] -f outputFilename

 

where:

``-h host`` indicates the name or IP address of the machine where the
server is launched.

``-p port`` indicates the port number at which the server is launched.

``-l login`` indicates the login name used to connect to the server.

``-P password`` indicates the password used to connect to the server.
You can encrypt your password using the script ``encrypt_password``.
That way you can avoid entering it in plain text. If the password is
encrypted, prefix it with ``encrypted:`` E.g.
``-P encrypted:Gr16MjvuXhRzPtPH/yTXHw==``

``-vdpauth`` is an optional argument. Using it causes the given user to be authenticated
against the Virtual DataPort server configured in :ref:`Virtual DataPort settings <Virtual DataPort Settings>`.
Otherwise, the authentication is local to Scheduler.

``-L p1, p2...`` is an optional argument. Using it causes named projects
to be exported; otherwise all projects are exported.

``-dependencies`` is an optional argument. Using it includes the elements
on which the selected items depend.

``-config`` is an optional argument. Using it causes server
configuration to be exported.

``-drivers`` is an optional argument. Using it causes JDBC adapters to
be exported. When the *export all projects* option is selected all JDBC
adapters are exported, otherwise only the adapters used by the selected
projects are exported.

``-plugins`` is an optional argument. Using it causes plugins to be
exported. When the *export all projects* option is selected all plugins
are exported, otherwise only the plugins used by the selected projects
are exported.

``-exportWithProperties`` is an optional argument. Using it causes 
the zip exported file to contain two files: one that contains the 
metadata and another one that contains the values of the parameters 
whose value depend on the environment where the Server is running.

``-permissions`` is an optional argument. Using it causes the roles 
and permissions to be exported.

``-f outputFilename`` indicates the name of the zip file that will
contain the exported metadata.

 

The line below is an example of running the export command:

 
.. code-block:: bash

   export -h localhost -p 8000 -l admin -P admin -L default -f backup.zip

 

This command exports the metadata of the *default* project of the
Scheduler server being run in the local machine on port 8000. Access to
the server is made using the login ``admin`` with the password
``admin``. The result of the export is saved to a file known as
``backup.zip``.



Import
------

The import script allows importing the metadata and configuration
contained in a zip file obtained by using the export utility.

 

The format used to invoke the script is as follows:

 
.. code-block:: bnf

   import -h host -p port -l login -P password 
   -f inputFilename [ -pf <propertiesFile> ] [ -vdpauth ] [ -replace | [ -replaceJobs ] [ -replaceDataSources ] 
   [ -replaceFilterSequences ] [ -replacePlugins ] [ -replaceDrivers ] ] [ -legacy ]

 

where:

``-h host`` indicates the name or IP address of the machine where the
server is launched.

``-p port`` indicates the port number at which the server is launched.

``-l login`` indicates the login name used to connect to the server.

``-P password`` indicates the password used to connect to the server.
You can encrypt your password using the script ``encrypt_password``.
That way you can avoid entering it in plain text. If the password is
encrypted, prefix it with ``encrypted:`` E.g.
``-P encrypted:Gr16MjvuXhRzPtPH/yTXHw==``

``-vdpauth`` is an optional argument. Using it causes the given user to be
authenticated against the Virtual DataPort server configured in :ref:`Virtual DataPort settings <Virtual DataPort Settings>`.
Otherwise, the authentication is local to Scheduler.
 
``-f inputFile`` is the path to the file that contains the metadata to be imported.

``-pf propertiesFile`` is the path to the properties file with the values of the parameters 
that depend on the environment where the Server is running. If the file specified in the
``-f`` argument already contains a properties file inside, they will be ignored if the
``-pf`` option is used.

``-replace`` is an optional argument that specifies that the elements
included in the imported file will replace existing elements with the
same name. If the ``-replace`` option is used, the ``-replaceXXX``
options (explained below) will be ignored.

``-replaceJobs`` is an optional argument that specifies that the jobs
included in the imported file will replace existing jobs with the same
name.

``-replaceDataSources`` is an optional argument that specifies that the
data sources included in the imported file will replace existing data
sources with the same name.

``-replaceFilterSequences`` is an optional argument that specifies that
the filter sequences included in the imported file will replace existing
filter sequences with the same name.

``-replacePlugins`` is an optional argument that specifies that the
plugins included in the imported file will replace existing plugins with
the same name.

``-replaceDrivers`` is an optional argument that specifies that the
drivers included in the imported file will replace existing drivers with
the same name.

``-legacy`` is an optional argument that must be used to import metadata
exported with Denodo Platform 4.6 and an update previous to 20120905 or
Denodo Platform 4.7 and an update previous to 20120802 (if this option
is not used, the metadata could be not properly imported). Anyway, if
the exported metadata are still in the server, to import them in Denodo
Platform |version| it is preferable to install the last update of the
corresponding version (4.6 or 4.7), export them again and import them
without this option. Also note that this option should only be used with
metadata exported with previous versions of the Denodo Platform (4.6 or
4.7) and an update previous to the mentioned ones (in other case the
metadata could be not properly imported).

 

For example:

 
.. code-block:: bash

   import -h localhost -p 8000 -l admin -P admin -f backup.zip -vdpauth -replace

 

This sentence imports the metadata contained in ``backup.zip`` to the
server running in the local machine on port 8000. Access to the server
uses the login ``admin`` with the password ``admin``, authenticated against a Virtual DataPort server.
Information and warning messages returned by the server as a result of the import are written to the console.

.. note::
   when using the ``-vdpauth`` parameter, it is necessary that the user used to import/export
   the backup has the role *scheduler_admin* assigned in Virtual DataPort in order to have the
   permissions to perform these actions.

These scripts exit with code 0 when there are no errors; otherwise, they return 1.