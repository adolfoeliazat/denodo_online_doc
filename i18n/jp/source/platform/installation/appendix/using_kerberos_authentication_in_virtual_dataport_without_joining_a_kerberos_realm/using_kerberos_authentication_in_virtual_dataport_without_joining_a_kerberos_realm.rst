==================================================================================
Using Kerberos Authentication in Virtual DataPort Without Joining a Kerberos Realm
==================================================================================

Virtual DataPort and the Data Catalog can use the authentication method provided by a
Kerberos realm (e.g. a Windows Active Directory domain), even if the
server where Virtual DataPort runs does not join this realm. To be able
to do this, you have to add some properties to the Denodo configuration
scripts.

Follow these steps:

#. Open the Denodo Control Center
#. Click **Configure**
#. Click **JVM Options**
#. In the **Virtual DataPort server / ITPilot wrapper server** box, add
   the following (do *not* remove the existing content of this field):

.. code-block:: bash

   -Djava.security.krb5.realm=<domain realm> 
   -Djava.security.krb5.kdc=<Key distribution center 1>[:<key distribution center>]+
   

For example,

.. code-block:: bash

   -Djava.security.krb5.realm=CONTOSO.COM -Djava.security.krb5.kdc=dc-01.contoso.com


If there is more than one key distribution center (kdc) in your domain,
add it to the ``java.security.krb5.kdc`` property separated by a colon.
For example:

.. code-block:: bash

   -Djava.security.krb5.realm=CONTOSO.COM 
   -Djava.security.krb5.kdc=dc-01.contoso.com:dc-02.contoso.com

5. Add the same properties you added in the previous step, to the
   **Virtual DataPort Administration Tool** box.
   
   .. important:: Perform this last step in all the hosts that run an 
      Administration Tool that need to use Kerberos authentication. Not just 
      in the host where the Virtual DataPort server runs.
   
#. Restart the Virtual DataPort server and its Administration Tools.

|

If the Virtual DataPort Server and/or the Data Catalog are running on a “headless” host (i.e. a
host without graphical support), you cannot launch the Control Center.
Instead, to set the Kerberos system properties do the following:

1. Edit the :file:`{<DENODO_HOME>}/conf/vdp/VDBConfiguration.properties` file.
#. Add to the ``java.env.DENODO_OPTS_START`` property, the properties
   ``java.security.krb5.realm`` and ``java.security.krb5.kdc`` with the
   values explained above.
#. Execute :file:`{<DENODO_HOME>}/bin/regenerateFiles.sh`
#. Restart the Virtual DataPort server.

   Even if the Server runs in a headless environment, you still have to set 
   these properties in the hosts where the Administration Tools run.

|

Note that you have to run these steps in:

-  The Virtual DataPort server installations.
-  The Data Catalog installations.
-  The installation of all the Administration Tools that will use
   Kerberos authentication.
-  You have to define these system properties in the Java applications
   that will connect to Virtual DataPort using Kerberos authentication.

After performing these steps, configure the Virtual DataPort server and
its clients to use Kerberos authentication. The section 
:doc:`Kerberos Authentication<../../../../vdp/administration/server_administration_-_configuring_the_server/kerberos_authentication/kerberos_authentication>`
of the Virtual DataPort Administration Guide explains
how to do so.
