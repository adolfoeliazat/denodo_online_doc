============================================================
Connecting to Virtual DataPort Using Kerberos Authentication
============================================================

The Denodo JDBC driver supports the Kerberos authentication protocol. However, to use it you need to add certain properties to the connection. If not, even if the Virtual DataPort server is configured to use Kerberos
authentication, the client will still use standard
authentication.

The tables below list the driver properties you have to set when creating the JDBC connection.

|

.. rubric:: Kerberos Authentication with User and Password

This is the easiest option to set up. With this option, the driver will request a ticket-granting ticket (TGT) to the Active Directory and send it to the Server. This is valid for standalone applications that always use the same credentials to connect to Denodo.

.. table:: Driver properties for “User/password” authentication with Kerberos

   +--------------------------------------+--------------------------------------+
   | Property                             | Value                                |
   +======================================+======================================+
   | useKerberos                          | true                                 |
   +--------------------------------------+--------------------------------------+
   | debug                                | true                                 |
   |                                      |                                      |
   |                                      | Remove this property if there        |
   |                                      | are no issues with the Kerberos      |
   |                                      | authentication                       |
   +--------------------------------------+--------------------------------------+
   | user and password                    | User name and password of the        |
   |                                      | user. When using a JDBC client such  |
   |                                      | as DbVisualizer, you can enter the   |
   |                                      | credentials in the “User name” and   |
   |                                      | “password” boxes, instead of         |
   |                                      | providing them as a property.        |
   +--------------------------------------+--------------------------------------+

|

.. rubric:: Kerberos Constrained Delegation

The Denodo driver supports Kerberos constrained delegation. This allows the Denodo server to obtain service tickets to a restricted list of other services after the client has presented a service ticket.

.. table:: Driver properties to pass the Kerberos credential as an object
    
   +--------------------------------------+--------------------------------------+
   | Property                             | Value                                |
   +======================================+======================================+
   | useKerberos                          | true                                 |
   +--------------------------------------+--------------------------------------+
   | userGSSCredential                    | Java object of the class             |
   |                                      | ``org.ietf.jgss.GSSCredential``      |
   +--------------------------------------+--------------------------------------+

In the driver property ``userGSSCredential`` you have to pass a GSSCredential object. The following sample code shows how to do this:

.. code:: java

   GSSCredential userCredential = ...;
   Driver driver = (Driver) Class.forName("com.denodo.vdp.jdbc.Driver").newInstance();
   Properties properties = new Properties();
   properties.put("userGSSCredential", userCredential);
   Connection conn = driver.connect("jdbc:vdb://denodo1.acme.com:9999/customer?userAgent=myApplication", properties);

|

.. rubric:: Single Sign-On

With this option, the client application does not have to provide the credentials. The driver automatically obtains the Kerberos credential of the user that launches the application and uses it to connect to Denodo. The user does not have to enter the credentials.

.. todo: for 8.0, rewrite the following paragraphs to remove references to previous updates.

If the client application runs on Windows, consider this:

-  If you use the driver of the **update 20190903 or newer**, you need to load the following libraries in addition to the Denodo driver:
   
   -  :file:`<DENODO_HOME>/lib/contrib/jna.jar`
   -  :file:`<DENODO_HOME>/lib/contrib/jna-platform.jar`
   
   For example, in DB Visualizer, in the Driver Manager you have to load the jar file of the driver and these two jars.
   
   You do not need to modify the Windows registry anymore.

-  If you use the driver of the **update 20190312 or older**, you need to modify the Windows registry of the host in which this client application runs. Otherwise, the driver will not be able to obtain the Kerberos credential. The section :doc:`../../../../platform/installation/postinstallation_tasks/postinstallation_tasks_in_virtual_dataport/setting-up_kerberos_authentication` explains how to do this.

.. table:: Driver properties to obtain the Kerberos credential from the ticket cache of the system “Windows Single Sign-On (SSO)”
    
   +--------------------------------------+--------------------------------------+
   | Property                             | Value                                |
   +======================================+======================================+
   | useKerberos                          | true                                 |
   +--------------------------------------+--------------------------------------+
   | debug                                | true                                 |
   |                                      |                                      |
   |                                      | Remove this property if there        |
   |                                      | are no issues with the Kerberos      |
   |                                      | authentication                       |
   +--------------------------------------+--------------------------------------+
   | useTicketCache                       | true                                 |
   +--------------------------------------+--------------------------------------+
   | renewTGT                             | true                                 |
   +--------------------------------------+--------------------------------------+


|

.. rubric:: Using the Kerberos Credentials Stored in a Ticket Cache

With this option, the driver will obtain the Kerberos credential from a Kerberos credential cache.  

.. table:: Driver properties to obtain the Kerberos credential from a ticket cache file

   +--------------------------------------+--------------------------------------+
   | Property                             | Value                                |
   +======================================+======================================+
   | useKerberos                          | true                                 |
   +--------------------------------------+--------------------------------------+
   | debug                                | true                                 |
   |                                      |                                      |
   |                                      | Remove this property if there        |
   |                                      | are no issues with the Kerberos      |
   |                                      | authentication                       |
   +--------------------------------------+--------------------------------------+
   | useTicketCache                       | true                                 |
   +--------------------------------------+--------------------------------------+
   | renewTGT                             | true                                 |
   +--------------------------------------+--------------------------------------+
   | ticketCache                          | Path to the Ticket cache file        |
   +--------------------------------------+--------------------------------------+

Before using this authentication mode, you need to generate a “ticket
cache file” on the host where the JDBC application will run. That is,
manually obtain and cache on a file a ticket-granting ticket (TGT). To
do this, open a command line and execute the following:

.. code-block:: batch

   <DENODO_HOME>\jre\bin\kinit.exe -f -c "<DENODO_HOME>\conf\vdp-admin\ticket\cache"

The option ``-f`` requests the Key Distribution Center (KDC) to return
“forwardable” tickets.


When the Client Application Does Not Belong to the Domain
=========================================================

Applications that want to connect to Denodo using Kerberos authentication need information about the Active Directory domain. If the client application runs on a host that is part of an Active Directory domain, usually you do not have to do anything because the application obtains this information from the environment. However, if the client is not part of the domain, you need to provide this information. To do this, you need to obtain a krb5.conf/ini file. 

.. 
   csantos@2017/10/30 we comment all these because placing a krb5.ini/conf file is a much more easy way of doing this.
   .. code-block:: none
   
      -Djava.security.krb5.realm=<domain realm>
      -Djava.security.krb5.kdc=<Key distribution center 1>[:<key distribution center>]+
   
   .. 
   
      For example,
   
   .. code-block:: none
   
      -Djava.security.krb5.realm=CONTOSO.COM 
      -Djava.security.krb5.kdc=dc-01.contoso.com
   
   .. 
   
      If there is more than one key distribution center (kdc) in your domain, add it to the property ``java.security.krb5.kdc`` separated by a colon. For example:
     
      
   .. code-block:: none
   
      -Djava.security.krb5.realm=CONTOSO.COM 
      -Djava.security.krb5.kdc=dc-01.contoso.com:dc-02.contoso.com