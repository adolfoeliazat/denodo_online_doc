===============================================
Setting-up Kerberos Authentication in Scheduler
===============================================

Scheduler provides support to authenticate its clients 
using the Kerberos authentication protocol.

If you are not going to use the Scheduler, go to the next post-installation action.

Once you set-up the Virtual DataPort server to use Kerberos, it is important 
to distinguish these two scenarios: 

1. :ref:`Scheduler and the Virtual DataPort Server are in Different Machines`
#. :ref:`Scheduler and the Virtual DataPort Server are in the Same Machine`

Scheduler and the Virtual DataPort Server are in Different Machines
===========================================================================================

In this scenario, you will have to perform the :ref:`same post-installation tasks <Setting-up Kerberos Authentication>` you did to enable Kerberos on Virtual DataPort:

1. In the Active Directory, :ref:`create a user of type “User” <Creating a User in the Active Directory>`.

#. :ref:`Declare a Service Principal Name (SPN) <Declaring a Service Principal
   Name (SPN)>` and associate it with this new user.

#. :ref:`Generate a keytab file <Generating a Keytab File>` for this SPN.

#. Copy the keytab file to the host where the Scheduler runs.

After performing these steps, you have to configure the Scheduler to use Kerberos authentication. The section :doc:`../../../../scheduler/administration/administration/web_configuration/kerberos_configuration/kerberos_configuration` of the Scheduler Guide explains how to do this.

.. important:: If you want to configure the Scheduler server and the Scheduler administration tool with different user accounts, you will have
   to perform these actions for each one.
   
|
   
Scheduler and the Virtual DataPort Server are in the Same Machine
=========================================================================================

If the Scheduler runs on the same host than the Virtual DataPort server, we recommend using the same keytab file and the same Service Principal Name as in the Virtual DataPort server. That way, you do not have do anything extra and the Server will be easier to manage.

If you want this tool to use a different service principal name (SPN), the new SPN has to follow the same rules described in the previous section (:ref:`Declaring a Service Principal Name (SPN)`). In addition, you have to create another user account because two SPNs cannot share the same user account. 