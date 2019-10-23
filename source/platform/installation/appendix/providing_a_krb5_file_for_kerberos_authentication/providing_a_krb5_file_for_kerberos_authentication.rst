=================================================
Providing a Krb5 File for Kerberos Authentication
=================================================

To enable Kerberos authentication on a component of the Denodo server, you need to obtain 
a Kerberos configuration file (krb5 file) when one or more of these conditions is met:

-  The host where the Denodo server runs does not belong to a Windows domain.
-  The Denodo server runs on Linux.
-  The user account in Active Directory used by the Denodo server components has the option 
   *constrained delegation* enabled.

For the Virtual DataPort administration tool, you need the Kerberos configuration file when 
Active Directory does not return “forwardable” tickets by default.

If any of these conditions are met, check if there is a krb5 file in the
default path of the operating system (see table `Default location of the
krb5 file depending on the operating system`_).


.. table:: Default location of the krb5 file depending on the operating system
   :name: Default location of the krb5 file depending on the operating system

   +-------------------+-------------------------------------------------------+
   | Operating System  | Default Path for the krb5 file                        |
   +===================+=======================================================+
   | Windows           | <Windows directory>\\krb5.ini (the system directory   |
   |                   | usually is C:\\Windows).                              |
   |                   |                                                       |
   |                   | Note that in Windows, the name of the file is         |
   |                   | krb5.ini and not krb5.conf.                           |
   +-------------------+-------------------------------------------------------+
   | Linux             | /etc/krb5.conf                                        |
   +-------------------+-------------------------------------------------------+
   | Solaris           | /etc/krb5/krb5.conf                                   |
   +-------------------+-------------------------------------------------------+


If the file exists, make sure it has the property ``forwardable = true``
in the ``[libdefaults]`` section of the file.

If the file does not exist, create it in the default path. The figure
`Sample krb5 file`_ is an example of a krb5 file.

 
.. code-block:: dosini
   :caption: Sample krb5 file
   :name: Sample krb5 file
   
   [libdefaults]
       default_realm = CONTOSO.COM
       forwardable = true

   [realms]
   CONTOSO.COM = {
       kdc = dc-01.contoso.com
       default_domain = CONTOSO.COM
   }

   [domain_realm]
       .contoso.com = CONTOSO.COM

With the property ``forwardable = true``, the system will request “forwardable” tickets 
to the Kerberos server. These tickets can be used by the other applications (in this case, 
the Virtual DataPort server) to request service tickets on behalf of the user. These 
service tickets will be used to perform Kerberos requests to other services (e.g. databases) 
on behalf of the Virtual DataPort client (i.e. the Administration Tool, JDBC clients 
and ODBC clients).
