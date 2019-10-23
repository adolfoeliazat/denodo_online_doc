==================================================================
Enabling SSL/TLS in the Administration Tool and Others
==================================================================

When SSL/TLS is enabled in a Denodo Platform server, the Administration Tool, the ITPilot Wrapper Generator Tool and the Denodo Monitor need to trust the public key of the server. Otherwise, they will refuse connecting to the Denodo servers.

If the certificate you set up in the Denodo server is signed by a public Certification Authority (CA) like Symantec, skip this and go to the next section. All clients will accept the certificate presented by the Denodo server.

If when you connect from the administration tool, you get an error saying that the certificate cannot be trusted, it is because the certificate used by the server is signed by a private authority. In this case you have two alternatives:

a. Send the file :file:`{<DENODO_HOME>}/jre/lib/security/cacerts` of the installation of the Denodo server to the users that want to use the administration tool to connect to it.

   Then, the users have to **replace** the ``cacerts`` file of their installation of the administration tool, with the file you send them. It is located in the same path both in the server and in the client installations (:file:`{<DENODO_HOME>}/jre/lib/security/cacerts`).

   It is safe to send this file to any user because it is the *TrustStore* so it does not contain private keys; it only contains the public certificates.
   
b. Or you can send the users the public key (the .cer file) and they can install it by executing the following in the command line in their computer:

   .. code-block:: batch

      cd <DENODO_HOME>
      .\jre\bin\keytool -importcert -alias "denodo_server" -file denodo_server_public_key.cer -keystore .\jre\lib\security\cacerts -storepass "changeit" -noprompt

   Option b) is more cumbersome.

After replacing the file ``cacerts`` or adding a certificate to it (option a) or b) respectively), the user will need to restart the Administration tool and the other Denodo tools.


.. rubric:: Additional Information about the *TrustStore* (cacerts file)

**Although not recommended**, you can configure the Virtual DataPort Administration Tool, the ITPilot Wrapper Generator Tool and the Denodo Monitor to use a *TrustStore* that is not the default one.

We do not recommend doing this because it makes the configuration more complex unnecessarily. If you need to do it, follow these steps:

#. Open the following files:
   
   i. :file:`{<DENODO_HOME>}/conf/itp-admin-tool/ITPAdminConfiguration.properties`
      (configuration file of the ITPilot Wrapper Generator Tool)
   #. :file:`{<DENODO_HOME>}/conf/itpilot-client/ConfigurationParameters.properties`
   #. :file:`{<DENODO_HOME>}/conf/vdp-admin/VDBAdminConfiguration.properties`
      (configuration file of the Virtual DataPort Administration Tool)
   #. :file:`{<DENODO_HOME>}/tools/monitor/denodo-monitor/conf/ConfigurationParameters.properties`
      (configuration file of the Denodo Monitor Tool)

#. In these files, uncomment the following
   property and change its value:

   -  ``com.denodo.security.ssl.trustStore=``\ path to the new *TrustStore*.
