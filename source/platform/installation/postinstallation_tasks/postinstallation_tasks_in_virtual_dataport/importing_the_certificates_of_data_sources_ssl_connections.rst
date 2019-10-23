================================================================
Importing the Certificates of Data Sources (SSL/TLS Connections)
================================================================

When Virtual DataPort establishes an SSL or TLS connection with a data source,
the data source presents a certificate. Virtual DataPort relies on the
Java Cryptography Architecture (JCA) to check if the certificate is
valid. JCA accepts certificates signed by any known Certificate Authority [#f1]_
included with the Oracle Java Runtime Environment version 8.
However, if the certificate used by the server is signed by an authority
not present in this list, you have to import this certificate into the
list of trusted certificates (called *TrustStore*).

To import a certificate into the *TrustStore* of the Java Runtime
Environment (JRE), execute the following commands:

.. rubric:: For Windows

.. code-block:: batch

   cd <DENODO_HOME>
   cd jre\bin
   keytool -importcert -alias <name of the certificate> -file <new certificate>.crt -keystore ..\lib\security\cacerts -storepass changeit

.. rubric:: For Linux

.. code-block:: bash

   cd <DENODO_HOME>
   cd jre/bin
   keytool -importcert -alias <name of the certificate> -file <new certificate>.crt -keystore ../lib/security/cacerts -storepass changeit


Explanation of the parameters:

-  ``alias``: this parameter is mandatory. The certificate will be
   stored in the *TrustStore* identified by this alias. If the
   *TrustStore* already contains a certificate with this alias, use
   another alias.
-  ``keystore``: path to the *TrustStore* where the certificate will be
   stored. :file:`{<DENODO_HOME>}/jre/lib/security/cacerts` is the path of the
   TrustStore of the JRE included in the Denodo Platform.

   If you have uncommented the property
   ``com.denodo.security.ssl.trustStore`` of the file
   :file:`{<DENODO_HOME>}/conf/vdp/VDBConfiguration.properties`, the value of
   this parameter has to be the value of this property, instead of
   :file:`{<DENODO_HOME>}/jre/lib/security/cacerts`. That is because, if this
   property is uncommented, Virtual DataPort will use the *TrustStore*
   set in this property of the ``VDBConfiguration.properties`` file,
   instead of the JRE one.

   If you are going to launch Virtual DataPort with a JRE not included
   in the Denodo Platform and the property
   ``com.denodo.security.ssl.trustStore`` is commented, the value of
   this parameter has to be the path to the ``cacerts`` file of this
   other JRE, which is located in the directory ``lib/security`` of the
   JRE.

To check that the certificate has been imported correctly, execute this
command:

.. code-block:: bash

   <DENODO_HOME>\jre\bin\keytool -list -v -alias <name of the certificate> -keystore
   <DENODO_HOME>\jre\lib\security\cacerts

After adding a certificate, you have to restart the Virtual DataPort
server in order for the changes to take effect.

The `keytool documentation for Windows <https://docs.oracle.com/javase/8/docs/technotes/tools/windows/keytool.html>`_
and `Linux <https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html>`_
provides much more details about the parameters of this tool.

-------------------------------------------------------------------------------

**Footnotes**

.. [#f1] To obtain the list of Certificate Authorities recognized by Java, open a command line and execute this:

.. rubric:: For Windows

.. code-block:: batch

   cd <DENODO_HOME>
   cd jre\bin
   keytool -list -storepass changeit -keystore ..\lib\security\cacerts


.. rubric:: For Linux

.. code-block:: bash

   cd <DENODO_HOME>
   cd jre/bin
   ./keytool -list -storepass changeit -keystore ../lib/security/cacerts
