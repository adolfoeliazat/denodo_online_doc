==========================================================
Configuring Several Instances of a Virtual DataPort Server
==========================================================

It is possible to launch two or more instances of a Virtual DataPort
Server at the same time, **in the same host**. Each instance is executed
independently, which means that each one is a different process of the
operating system. However, all of them share the same metadata (data
sources, views, etc.) and the same embedded Web container.

When executing two or more instances of the same Server, one of the
instances is the *Master* and the others are the *Slaves*. The *Master*
instance has to be launched before the *Slave* instances.

Only the *Master* instance can modify the metadata of the Server (create
/ edit / remove data sources, views, etc.).

When modifying the metadata, you have to restart all the *Slave*
instances so they “see” the changes. That is because the *Slaves* only
read the metadata from the *Master* when they are launched.

When defining a JMS listener, the Master instance is the only one that
opens the connection to the JMS server.

Virtual DataPort uses an embedded Derby database to store the metadata
created by its users (data sources, views, etc.) The *Slave* instances
have access to the metadata of the elements by connecting to the
embedded database of the *Master* Server.

How to Configure the Slave Instances
====================================

Let us say that we want to add two new *Slave* instances called
``aux_1`` and ``aux_2``. To do this, follow these steps:

#. Log in as an administrator, open the VQL Shell and execute the following commands:

.. code-block:: properties

   SET 'vdp.instances' = 'aux_1,aux_2';

   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_1.registryURL' = 'localhost';
   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_1.registryPort' = '19999';
   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_1.shutdownPort' = '19998';
   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_1.factoryPort' = '19997';
   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_1.odbcPort' = '19996';

   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_2.registryURL' = 'localhost';
   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_2.registryPort' = '29999';
   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_2.shutdownPort' = '29998';
   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_2.factoryPort' = '29997';
   SET 'com.denodo.vdb.vdbinterface.server.VDBManagerImpl.aux_2.odbcPort' = '29996';


Note that for each element of the property ``vdp.instances``, you have to define the following properties:

.. code-block:: none

   com.denodo.vdb.vdbinterface.server.VDBManagerImpl.<name of the instance>.registryURL
   com.denodo.vdb.vdbinterface.server.VDBManagerImpl.<name of the instance>.registryPort
   com.denodo.vdb.vdbinterface.server.VDBManagerImpl.<name of the instance>.shutdownPort
   com.denodo.vdb.vdbinterface.server.VDBManagerImpl.<name of the instance>.factoryPort
   com.denodo.vdb.vdbinterface.server.VDBManagerImpl.<name of the instance>.odbcPort


2. Optionally, you can configure the options of the Java Virtual Machine for each new
   instance. To do this, execute the following commands:

.. code-block:: properties

   SET 'java.env.aux_1.DENODO_OPTS_START' = '-mx2048m -XX:MaxPermSize=256m';
   SET 'java.env.aux_1.DENODO_OPTS_STOP' = '-mx64m';
   
   SET 'java.env.aux_2.DENODO_OPTS_START' = '-mx2048m -XX:MaxPermSize=256m';
   SET 'java.env.aux_2.DENODO_OPTS_STOP' = '-mx64m';

..

   The property ``DENODO_OPTS_START`` has the JVM options that the instance
   will launched with.
   
   The property ``DENODO_OPTS_STOP`` has the JVM options that the script
   that stops the Server will be launched with.
   
   The Servers defined in the property ``vdp.instances`` that do not have
   the variables ``DENODO_OPTS_START`` and ``DENODO_OPTS_STOP`` defined,
   will be launched with the JVM options of the *Master* server.
  

3. In the installation, execute the script :file:`{<DENODO_HOME>}/bin/regenerateInstanceScripts`.
   
   This script will create the directory
   :file:`{<DENODO_HOME>}/bin/instances`, which will contain the scripts to
   launch the *Slave* instances of the *Master* Server.
   
   For each instance, there will be:

   a. A script to launch the instance:
      ``vqlserver_<name of the instance>_startup``
   b. A script to stop the instance:
      ``vqlserver_<name of the instance>_shutdown``
      
      To stop the instance with the “Safe shutdown mode”, execute
      ``vqlserver_<name of the instance>_shutdown safe``
      
      When doing this, the instance will stop accepting new connections and
      it will not shut down until all the queries have finished.
   
   c. If running the Server in Windows, a script to create a new Windows
      service, remove it, launch and stop the service:
      ``vdpservice_<name of the instance>.bat``


#. Launch the *Master* instance before the *Slave* instances. You can do it
   from the Denodo Control Center or executing the script
   :file:`{<DENODO_HOME>}/bin/vqlserver_startup.bat`.


#. Launch the *Slave* instances by executing the following scripts:
   :file:`{<DENODO_HOME>}/bin/instances/vqlserver_aux_1_startup <DENODO_HOME>/bin/instances/vqlserver_aux_2_startup`


#. In the slave instances, disable the Cache Maintenance Task.
   
   To do this, open the dialog “Cache” of the “Administration > Server
   configuration” menu and select **Maintenance Off**.


#. To stop the instances, you have to stop the *Slave* instances first and
   after this, the *Master*.


Now you can connect to any of the three instances (the *Master* or the
*Slaves*) to query views. However, you have to connect to the *Master*
instance to modify the metadata of the Server. That is, to
create/edit/delete data sources, views, publish Web services, etc.
