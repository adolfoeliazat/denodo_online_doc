=======================================================================
Transforming Incoming/Outgoing Soap/Rest Messages with XSLT Stylesheets
=======================================================================

As explained in the section :ref:`XSLT Transformations`, we can define XSLT
stylesheets (`XSL Transformations <https://www.w3.org/TR/xslt>`_)
to transform incoming/outgoing SOAP and
XML messages. Thus, we avoid any modifications to existing clients.

This section explains a possible way to generate these XSLT stylesheets
more easily leveraging on some popular open-source tools.

Before starting, we need to obtain some tools that will help us to
define these transformations:

-  `SoapUI <https://www.soapui.org>`_).
-  Eclipse Web Tools. (http://www.eclipse.org/webtools). Set of
   Eclipse plugins that includes editors to create and test XSLT
   transformations. Specifically, we will use the “XML Editor Tools” and
   the “XSL Developer Tools”.

First, we have to publish a new SOAP Web service from Virtual DataPort
(section :doc:`Publication of Web Services </vdp/administration/publication_of_web_services/publication_of_web_services>` explains how to do it). Make the
structure of the new Web service as similar as possible to the existing
Web service. By doing this, the XSLT stylesheets will be simpler:

-  The operations of the new Web service should have the same parameters
   as the existing one.
-  Change the names of operations and parameters so they match with the
   names of the existing Web service.
-  You can also use FLAT / NEST operations (see section :ref:`Creating
   Flatten Views`) to adjust the hierarchical structure of the
   published Web service to the structure of the target Web service.

For each operation, we need:


-  For the “Input XSLT Transformation”:

   -  A SOAP request sent by the existing Web service client.
   -  A SOAP request expected by the new Web service.


-  For the “Output XSLT Transformation”:

   -  A SOAP response expected by the existing Web service client.
   -  A SOAP response sent by the new Web service.

To obtain these sample messages, we need the WSDL documents (`Web
Services Description Language (WSDL) 1.1. <https://www.w3.org/TR/2001/NOTE-wsdl-20010315>`_)
of the existing and the new Web services.

If we have deployed the new Web service in the embedded Web container,
we can obtain its WSDL at
``http://localhost:9090/server/<VDP database name>/<name of service>/services/<name of service>?wsdl``

#. For both WSDL files, we have to obtain a sample SOAP request and a
   sample SOAP response. We can use SoapUI to do this:
#. Launch SoapUI.
#. Create a new project for each WSDL. In the “New Project” dialog,
   select the options “Create Requests” and “Create MockService”.
   After creating the project, the left-side panel contains a list of
   the operations of the Web service.
#. For each operation, double-click on “Request” to open the template of
   the request.
#. For each operation, double-click on “Response” to open the template
   of the response expected by the client.
#. Save each template: Right-click on each template’s window and then,
   on “Save as…”

After this, we should have four SOAP files for each operation:

#. A request sent by the existing Web service client.
#. A response sent by the existing Web service.
#. A request expected by the new Web service
#. A response sent by the new Web service.

Now, for each operation, we have to generate two XSLT stylesheets:

#. One XSLT stylesheet to transform the request sent by the existing
   client into what the new Web service expects.
#. Another XSLT stylesheet to transform the response sent by the new Web
   service into what the existing client expects.

There are many XML/XSLT editors to write XSLT transformations but in
this appendix, we focus on “XML Editor Tools” and “XSL Developer Tools”
from the Eclipse Web Tools. We can load the initial SOAP messages into
the editor and create the XSLT stylesheets using some useful features of
these editors: syntax highlighting and transformation
execution/debugging.

Once the XSLT transformations are finished, we have to execute them and
validate their output against the schema defined in the WSDLs.

There are two ways to validate the output of XSLT transformations:

#. Using Eclipse: extract the schema of the message from the WSDL and
   paste it in an ``xsd`` file. Then, using the XML editor, execute the
   XSD validation of the transformed XML (just the content inside the
   <Body> tag)
#. Using SoapUI: in the appropriate window, replace the old XML with the
   result of the transformation. Then, open the contextual menu and
   click on “Validate”.

.. important:: If the schema of a Web service changes, their XSLT
   stylesheets have to be modified accordingly. Changes in the schema of a
   Web service include:

   -  Renaming existing operations.
   -  Removing operations.
   -  Adding / renaming / removing parameters of existing operations.

In addition, if we add an operation to an existing Web service, its XSLT
transformation will be empty.
