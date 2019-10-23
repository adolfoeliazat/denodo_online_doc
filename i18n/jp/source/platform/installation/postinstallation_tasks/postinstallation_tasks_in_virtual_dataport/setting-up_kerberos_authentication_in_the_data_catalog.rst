=======================================================================
Setting-up Kerberos Authentication in the Data Catalog
=======================================================================

The Data Catalog provides support to authenticate its clients 
using the Kerberos authentication protocol.

If you are not going to use the Data Catalog, go to the next post-installation action.

Once you set-up the Virtual DataPort server to use Kerberos, it is important 
to distinguish these two scenarios: 

1. :ref:`The Data Catalog and the Virtual DataPort Server are in Different Machines`
#. :ref:`The Data Catalog and the Virtual DataPort Server are in the Same Machine`

The Data Catalog and the Virtual DataPort Server are in Different Machines
===========================================================================================

In this scenario, you will have to perform the :ref:`same post-installation tasks <Setting-up Kerberos Authentication>` you did to enable Kerberos on Virtual DataPort:

1. In the Active Directory, :ref:`create a user of type “User” <Creating a User in the Active Directory>`.

#. :ref:`Declare a Service Principal Name (SPN) <Declaring a Service Principal
   Name (SPN)>` and associate it with this new user.

#. :ref:`Generate a keytab file <Generating a Keytab File>` for this SPN.

#. Copy the keytab file to the host where the Data Catalog runs.

After performing these steps, you have to configure the Data Catalog to use Kerberos authentication. The section :doc:`/vdp/data_catalog/administration/kerberos_configuration/kerberos_configuration` of the Data Catalog Guide explains how to do this.

|

The Data Catalog and the Virtual DataPort Server are in the Same Machine
=========================================================================================

If the Data Catalog runs on the same host than the Virtual DataPort server, it has to use the same keytab file and the same Service Principal Name as in the Virtual DataPort server. That way, you do not have do anything extra and the Server will be easier to manage.