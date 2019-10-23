============================
Change the Default Passwords
============================

.. rubric:: Solution Manager

To authenticate users, the Solution Manager uses the Virtual DataPort server of its installation.
For security, you should create a new administrator user and then, remove the user ``admin`` from this Virtual DataPort server. To do it, follow these steps:

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


.. rubric:: Web Panel

The Web Panel has an administrator account to control its settings. For security, change the password of this account. To do it, follow these steps:

#. Assuming the Web Panel is started, open \https://denodo-server.acme.com:19090/webpanel/#/admin
#. Click **Login** and then, **Login as local user**.
#. In the **Password** field, enter "admin" and click **Sign in**.
#. Click on the user circle - on the top right of the page - and then, click **Change password**.
#. Enter the "old" password (i.e. ``admin``) and the new password.
#. Click **Change Password**.

See more about *local-based authentication* in the section :ref:`web_panel_authentication`.