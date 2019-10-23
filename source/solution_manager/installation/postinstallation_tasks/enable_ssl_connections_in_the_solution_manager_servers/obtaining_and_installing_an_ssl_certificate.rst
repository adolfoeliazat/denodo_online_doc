===========================================
Obtaining and Installing an SSL Certificate
===========================================

Before enabling SSL in the Solution Manager servers and clients, you need to create a
*keystore* with a key pair (a public key and associated private key) and a certificate. 

To generate a new keystore, follow one of these options:

a. If you do *not* have an SSL private key, you can :ref:`create a keystore with a self-signed private key<Create a Keystore with a Self-Signed Certificate>`.
#. If you do *not* have an SSL private key, you can :ref:`send a request to a certificate authority (CA) and create a keystore with the certificate reply<Send a Certificate Request to a Certificate Authority (CA) and Create a Keystore with the Reply>`.
#. If you have a PFX file with the private key, :ref:`create a keystore with its content<Create a Keystore from a PFX File (PKCS#12)>`.
#. If you already have a keystore file (usually, this file has the extension "jks"), jump to the :ref:`next<Enabling SSL/TLS in Solution Manager Servers>` section.

If you already generated a keystore for a Denodo server, you can reuse that keystore and its ``cacerts`` file. To do this:

1. Copy the keystore from the Denodo server installation (e.g. ``C:/denodo/denodo_server_key_store.jks``) to the installation of the Solution Manager.
2. Replace the cacerts file of the Solution Manager (:file:`{<SOLUTION_MANAGER_HOME>}/jre/lib/security/cacerts`) with the cacerts file of the Denodo server installation (:file:`{<DENODO_HOME>}/jre/lib/security/cacerts`). This second step is important, to make sure that the Solution Manager trusts the private key inside the keystore.

To use the options one, two or three, we are going to use the tool keytool. The `keytool documentation for Windows <https://docs.oracle.com/javase/8/docs/technotes/tools/windows/keytool.html>`_
and `Linux <https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html>`_ provides more details about the parameters of this tool.
