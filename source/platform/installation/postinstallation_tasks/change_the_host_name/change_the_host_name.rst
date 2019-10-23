===================================================
Change the Host Name in the Virtual DataPort Server
===================================================

If this Denodo server runs on your premises and on Windows, jump to the next section.

On the following scenarios, you have to configure the Virtual DataPort server so it can receive connections from external clients:

-  If this Denodo server runs on an instance of Amazon AWS, Google Cloud or other cloud provider, provide the fully qualified domain name (FQDN) of the instance. This host name has to be visible to clients outside this cloud instance. 
-  If this Denodo server runs on Linux, provide the hostname of this host. That is because, when the Virtual DataPort server runs on Linux/Unix, by default it only listens to local connections (the *loopback interface*).

The subsections below explain how to set this host name graphically or from the command line.

Instead of a host name, you can provide an IP address. However, we advise against it because a host name is more stable compared to an IP address that may change in the future.

Changing the Host Name of the Denodo Servers Graphically
=================================================================

Follow these steps to change the host name of the Virtual DataPort server graphically:

1. Open the Denodo Control Center.
#. Click **Configure**.
#. Click **JVM Options**.
#. Click the tab **RMI host**.
#. In the field **Virtual DataPort Server/ITPilot Wrapper Server**, enter the host name.

   You do not need to change the values of the fields below. The web container listens to connections on all the network interfaces of the host. The other Denodo components are only accessed by other components running on the same host (the Scheduler server is only accessed by the Scheduler administration tool, which runs on the local web container).
#. Click **Ok**.
#. Restart the Virtual DataPort server to pick up the changes.

Changing the Host Name of the Denodo Servers from the Command Line
==================================================================

Follow these steps to change the host name of the Virtual DataPort server from the command line:

1. Stop the Virtual DataPort server.

#. Edit the file :file:`{<DENODO_HOME>}/conf/vdp/VDBConfiguration.properties`
#. Set the value of the property ``com.denodo.vdb.vdbinterface.server.VDBManagerImpl.registryURL`` to the IP address or name of this host. You should have one line like this:

::

   com.denodo.vdb.vdbinterface.server.VDBManagerImpl.registryURL=dv-server-1.denodo.com

4. Execute the script :file:`{<DENODO_HOME>}/bin/regenerateFiles.sh`

   This propagates this change to the startup scripts of Virtual DataPort.
   
#. Start the Virtual DataPort server.