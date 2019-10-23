============================
Publishing SOAP Web Services
============================

.. toctree::
   :hidden:
   
   operations_tab.rst
   settings_tab_soap.rst
   advanced_tab_soap.rst
   

This section explains how to publish a view as a SOAP Web service.
Virtual DataPort supports the version 1.1 of the SOAP protocol 
(`Simple Object Access Protocol - SOAP - version 1.1 <https://www.w3.org/TR/2000/NOTE-SOAP-20000508/`>_)
and the version 1.1 of
WSDL (`Web Services Description Language - WSDL - version 1.1 <https://www.w3.org/TR/2001/NOTE-wsdl-20010315>`_).

To publish a view as a SOAP Web service, right-click on the view or
stored procedure you want to publish, in the Server Explorer and click
**SOAP Web service** on the **New** > **Data service** menu.

The “Create SOAP Web service” dialog has the following tabs:

-  **Operations**: lists the operations of the Web service. You can add
   more by dragging a view or a stored procedure to this dialog. See
   section :doc:`operations_tab`.
-  **Settings**: manages the configuration of the SOAP Web service. See
   section :ref:`Settings tab (SOAP)`.
-  **Advanced**: manages the parameters of the connection between the
   Web service and the Virtual DataPort Server and other advanced
   settings. See section :ref:`Advanced Tab (SOAP)`.
-  **Metadata**: select the target folder of the Web service and provide
   a description for it.

After configuring everything, click **Save** to create the Web service.
The Tool will display the “Web service container status” dialog that
lists the existing Web services. Then, you can deploy the Web service in
the embedded Web container or generate the war file and deploy it in an
external Web container (see section :ref:`Web Service Container Status
Table`).
