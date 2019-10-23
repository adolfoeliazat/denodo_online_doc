==================================================================================
Using Kerberos Authentication in Solution Manager Without Joining a Kerberos Realm
==================================================================================

The Solution Manager server and the License Manager server can use the 
authentication method provided by a Kerberos 
realm (e.g. a Windows Active Directory domain), even if the host where the 
Solution Manager server and the License Manager server run does not join this 
realm. To be able to do this, 
you have to add some properties to the Solution Manager configuration scripts.
Follow these steps:

#. Open the Denodo Control Center.
#. Click **Configure**.
#. Click **JVM Options**.
#. In the **Solution Manager Server box** and the **License Manager Server box**, 
   add the following (do *not* remove the existing content of this field):

   .. code-block:: bash

      -Djava.security.krb5.realm=<domain realm> -Djava.security.krb5.kdc=<Key distribution center 1>[:<key distribution center>]+


   For example,

   .. code-block:: bash

      -Djava.security.krb5.realm=CONTOSO.COM -Djava.security.krb5.kdc=dc-01.contoso.com


   
   If there is more than one key distribution center (kdc) in your domain, add it to 
   the property ``java.security.krb5.kdc`` separated by a colon. For example:

   .. code-block:: bash

      -Djava.security.krb5.realm=CONTOSO.COM -Djava.security.krb5.kdc=dc-01.contoso.com:dc-02.contoso.com
   
#. To apply these changes, stop all the Solution Manager servers and once they are all stopped, start them again.
   
|
   
If the Solution Manager server and the License Manager server are running on 
a “headless” host (i.e. a host without graphical support), you cannot launch 
the Control Center. Instead, to set the Kerberos system properties do the following:

#. For the Solution Manager server, edit the file ``<SOLUTION_MANAGER_HOME>/conf/solution-manager/SMConfigurationParameters.properties``

#. For the License Manager server, edit the file ``<SOLUTION_MANAGER_HOME>/conf/license-manager/LMConfigurationParameters.properties``

#. Add to the ``java.env.DENODO_OPTS_START`` property of each file, the properties ``java.security.krb5.realm`` and 
   ``java.security.krb5.kdc`` with the values explained above. 

#. Execute <SOLUTION_MANAGER_HOME>/bin/regenerateFiles.sh

#. To apply these changes, stop all the Solution Manager servers and once they are all stopped, start them again.

After performing these steps, please check :doc:`Authenticating with Kerberos</solution_manager/administration/authentication_and_authorization/authenticating_with_kerberos/authenticating_with_kerberos>` of the Administration Guide in order to use Kerberos authentication.
