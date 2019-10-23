============================
Change the Default Passwords
============================

.. rubric:: Virtual DataPort

Virtual DataPort has a default administrator account. For security, create a new administrator user and then, 
remove the user ``admin``. To do it, follow these steps:

#. Open the administration tool of Virtual DataPort and log into the Server with the default credentials:

   -  **Login**: admin
   -  **Password**: admin
   -  **Server URI**: //denodo-server.acme.com:9999/admin
   
#. Click the menu **Administration** > **User management**.
#. Click **New** and fill in the details:

   -  **Login**: the name of the new account.
   -  **User type**: select **Administrator**.
   -  **Authentication type**: select **Normal**.
   -  Enter the password.
   -  Click **Ok**.
#. Click the menu **File** > **Disconnect** and log in with the new user account.
#. Click the menu **Administration** > **User management**.
#. Select the user *admin* and click **Delete**. 

.. rubric:: Data Catalog

The Data Catalog has an administrator account to control its settings (this user account does not have access to any data). For security, change the password of this account. To do it, follow these steps:

#. Assuming the Data Catalog is started, open to \http://denodo-server.acme.com:9090/denodo-data-catalog/LoginLocal
#. Enter ``admin`` (default password).
#. Click on the user circle - on the top right of the page - and then, click **Change password**.
#. You will have to enter the "old" password (i.e. ``admin``) and the new password.
#. Click **Change Password**.
