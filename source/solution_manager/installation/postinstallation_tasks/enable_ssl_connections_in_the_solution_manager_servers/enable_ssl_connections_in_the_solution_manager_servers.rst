==========================================================
Enable SSL/TLS in the Solution Manager
==========================================================

This section explains how to secure with SSL/TLS the connections between the
Solution Manager servers, the administration tools and their clients.

Note that if you enable SSL in the Solution Manager servers, you also
have to do it in their clients.

SSL requires configuring certificate repositories. There are two types
of certificate repositories:

-  KeyStore
-  TrustStore

**KeyStore**

An application that listens to incoming SSL connections needs a public
key and a private key in order to allow clients to access the server. In
Java, these keys are stored in a repository called *KeyStore*.

**TrustStore**

During the initialization of an SSL connection, the server sends its SSL
certificate to the client. The client must then decide if it trusts this
or not. To do this, the client checks if the certificate has been signed
by a trusted certification authority (CA). The *TrustStore* is a
repository of the certificates of trusted certification authorities.

Every Java installation comes with a *TrustStore* that the JRE uses by
default (:file:`{<SOLUTION_MANAGER_HOME>}/jre/lib/security/cacerts` file). If the server’s
certificate is not signed by a trusted authority (i.e. one that is not
registered in the Java’s *TrustStore*), you have to store the
certificate of the authority in the ``cacerts`` file of the Java Runtime Environment (JRE) used to launch the Solution Manager
servers and their tools (:file:`{<SOLUTION_MANAGER_HOME>}/jre/lib/security/cacerts`).

The Java Runtime Environment (JRE) included with the Solution Manager has a utility called
*keytool* that manages the *Certificate Repositories*.

These are the steps to enable SSL:

.. toctree::
   :maxdepth: 1
   
   obtaining_and_installing_an_ssl_certificate.rst
   enabling_ssl_in_solution_manager_servers.rst
