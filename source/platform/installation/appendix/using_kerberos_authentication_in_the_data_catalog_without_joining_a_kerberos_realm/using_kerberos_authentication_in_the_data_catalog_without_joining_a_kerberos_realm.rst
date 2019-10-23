===================================================================================================
Using Kerberos Authentication in the Data Catalog Without Joining a Kerberos Realm
===================================================================================================

The Data Catalog can use the authentication method provided by a Kerberos realm (e.g. a Windows Active Directory domain), even if the server where the Data Catalog runs does not join this realm. To be able to do this, you have to add some properties to the Denodo configuration scripts.
Follow these steps:

1. Open the Denodo Control Center.

#. Click **Configure**.

#. Click **JVM Options**.

#. In the Web Container box, add the following (do *not* remove the existing content of this field):

.. code-block:: none

   -Djava.security.krb5.realm=<domain realm> -Djava.security.krb5.kdc=<Key distribution center 1>[:<key distribution center>]+
..

   For example,

.. code-block:: none

   -Djava.security.krb5.realm=CONTOSO.COM -Djava.security.krb5.kdc=dc-01.contoso.com

..
   
   If there is more than one key distribution center (kdc) in your domain, add it to 
   the property ``java.security.krb5.kdc`` separated by a colon. For example:

.. code-block:: none

   -Djava.security.krb5.realm=CONTOSO.COM -Djava.security.krb5.kdc=dc-01.contoso.com:dc-02.contoso.com

5. To apply these changes, stop all the Denodo Platform servers and once they are all stopped, start them again.

   It is important to stop them all so the Denodo web container is stopped as well. If for example, you leave the Data Catalog started the web container will not shut down and these changes will not take effect.
   
|
   
If the Data Catalog server is running on a “headless” host (i.e. a host without graphical support), you cannot launch the Control Center. Instead, to set the Kerberos system properties do the following:

1. Edit the file ``<DENODO_HOME>/resources/apache-tomcat/tomcat.properties``

#. Add to the ``java.env.DENODO_OPTS_START`` property, the properties ``java.security.krb5.realm`` and 
   ``java.security.krb5.kdc`` with the values explained above. 

#. Execute <DENODO_HOME>/bin/regenerateFiles.sh

#. To apply these changes, stop all the Denodo Platform servers and once they are all stopped, start them again.
   It is important to stop them all so the Denodo web container is stopped as well. If for example, you leave the Data Catalog started the web container will not shut down and these changes will not take effect.

As already stated, you only have to run these steps in the Data Catalog server installations. 

After performing these steps, configure the Data Catalog to use
Kerberos authentication. The section :doc:`../../../../vdp/data_catalog/administration/kerberos_configuration/kerberos_configuration` of the Data Catalog Administration Guide explains how to do so.
