==================================================================
Deploying the Web Administration Tool in an External Web Container
==================================================================

The Denodo Platform provides a war file that contains the Web
Administration Tool thus it can be deployed in an external Web
container.

The file is located at
:file:`{<DENODO_HOME>}/webapps/admintool/denodo-webadmintool.war`.

This application manages the ITPilot and Scheduler servers.

After deploying the application in the Web container, there are still
several configuration tasks required before the application can be run:

#. Edit the ``web.xml`` file located in the application ``WEB-INF``
   directory
#. Locate the ``<context-param>`` whose ``<param-name>`` is
   ``users.location.userList`` and update its value with the absolute
   path to the file, located in the ``WEB-INF/classes`` directory.

   Example: ``<WEBAPP_DIR>/WEB-INF/classes/users-configuration.xml``
#. Locate the ``<context-param>`` whose ``<param-name>`` is
   ``servers.location.itpilot`` and update its value with the absolute
   path to the file.

   Example: ``<WEBAPP_DIR>/WEB-INF/classes/itpilot-servers.xml``
#. Do the same with the values of the ``<context-param>`` elements whose
   ``<param-name>`` is ``servers.location.scheduler``.
#. Save the changes and restart the Web application.

By following these steps, you will end with one Web Administration Tool
for the two modules: ITPilot and Scheduler.

There is also a way to generate a Web Administration Tool for each
module, which is directly deployable in the Web Container, requiring no
manual actions (except deploying it). Thus, if you want to manage the
three servers, you will end up with three different .war files.

-  To generate the war file for ITPilot: execute the script located at
   :file:`{<DENODO_HOME>}/setup/webadmintool/rewrite_itp_webapp.{bat|sh}`
-  To generate the war file for Scheduler: execute the script located at
   :file:`{<DENODO_HOME>}/setup/webadmintool/rewrite_sched_webapp.{bat|sh}`

These scripts modify the same file located at
:file:`{<DENODO_HOME>}/webapps/admintool/denodo-webadmintool.war`, so it
must be deployed in the Web Container after executing each of the
scripts (obviously, with a different name for each module).
