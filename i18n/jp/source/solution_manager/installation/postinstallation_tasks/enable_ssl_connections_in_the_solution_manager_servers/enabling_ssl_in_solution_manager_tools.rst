:orphan:

======================================
Enabling SSL in Solution Manager Tools
======================================

When SSL is enabled in the Solution Manager server, its
Administration Tool needs to be configured to access the Server.

Also, all the Solution Manager server's clients have
to trust the public key of the server. This includes the administration
tools of the Solution Manager.

If, instead of importing it into the
:file:`{<SOLUTION_MANAGER_HOME>}/jre/lib/security/cacerts` *TrustStore*, you
created a new *TrustStore*, do the following steps. Otherwise, jump to the
next section.

#. Open the following files:
   
   -  :file:`{<SOLUTION_MANAGER_HOME>}/conf/vdp-admin/VDBAdminConfiguration.properties`
      (configuration file of the Virtual DataPort Administration Tool)
   -  :file:`{<SOLUTION_MANAGER_HOME>}/tools/monitor/denodo-monitor/conf/ConfigurationParameters.properties`
      (configuration file of the Denodo Monitor Tool)

#. In the files opened in the previous step, uncomment the following
   property and change its value:

   -  ``com.denodo.security.ssl.trustStore=``\ path to the new *TrustStore*.

The scripts of the Denodo Tools do not have a configuration file. To
redefine the default *TrustStore* that they use, you have to define the
``javax.net.ssl.trustStore`` Java system property. For example:

-  For Windows:

.. code-block:: batch

   SET JAVA_OPTS=-Djavax.net.ssl.trustStore=<SOLUTION_MANAGER_HOME>/jre/lib/security/cacerts

-  For Unix:

.. code-block:: bash
 
   export JAVA_OPTS=-Djavax.net.ssl.trustStore=<SOLUTION_MANAGER_HOME>/jre/lib/security/cacerts
