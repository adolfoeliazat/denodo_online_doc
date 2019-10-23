============================
Change the Default Passwords
============================

Denodo Scheduler, Denodo Scheduler Administration Tool, Denodo Scheduler-Index and the Denodo Scheduler-Index Administration Tool have a default administrator account:

-  User name: ``admin``
-  Password: ``admin``

For security, change the password of these accounts. Here you have listed how to do this for each server/tool:

- :ref:`Scheduler Server <Change The Local User Password>`
- :doc:`Scheduler Web Administration Tool </scheduler/administration/administration/web_configuration/user_configuration/user_configuration>`
- Scheduler-Index:
    
    1. Run script :file:`{<DENODO_HOME>}/bin/arn_webadmin_startup.bat/.sh`.
    #. Go to http://localhost:9090/webadmin/denodo-aracne-admin/ and connect to the Indexer server with the default credentials.
    #. Go to the *Configuration* section and use the "Change password" option to update the password.
    #. Stop this tool.
- Scheduler-Index Administration Tool:

    1. Run script :file:`{<DENODO_HOME>}/bin/arn_webadmin_startup.bat/.sh`.
    #. Go to http://localhost:9090/webadmin/denodo-aracne-admin/ and click *Settings*.
    #. Connect with the default credentials.
    #. Use the "Change password" option to update the password.
    #. Stop this tool.