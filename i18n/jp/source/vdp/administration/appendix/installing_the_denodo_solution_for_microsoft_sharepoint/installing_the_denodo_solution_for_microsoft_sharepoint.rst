=======================================================
Installing the Denodo Solution for Microsoft SharePoint
=======================================================

.. note:: Publishing views as widgets is a deprecated feature and it may be removed in future
   major versions of the Denodo Platform. 

   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

The Denodo Solution for Microsoft Office SharePoint is used by the Web
Parts widgets published with Virtual DataPort (see section :ref:`Export as
Microsoft Web Part`). It contains a DLL that, among other things, will
connect to the auxiliary Web Service used by those widgets to obtain the
data from a specific view. This solution can be deployed in SharePoint
2007 and SharePoint 2010.

.. important:: You only need to deploy this solution the first time you
   want to deploy a Web Part.

Follow these steps to deploy the solution:


#. Copy the file :file:`{<DENODO_HOME>}/webapps/webpart-core/DenodoWebPart.wsp` 
   to your SharePoint server.

#. Execute the following commands in your SharePoint server console:
   
   .. code-block:: batch
   
      cd C:\Program Files\Common Files\Microsoft Shared\web server extensions\12\bin
      stsadm.exe -o addsolution -filename "DenodoWebPart.wsp"
      stsadm.exe -o deploysolution -name "denodowebpart.wsp" -immediate -allcontenturls -allowFullTrustBinDlls
      stsadm.exe -o execadmsvcjobs
      
   The ``allowFullTrustBinDlls`` parameter is 
   necessary to allow connections to an external server (auxiliary Web Service), write 
   log information to files, read the XML configuration file of each Web Part, etc.
   
   You can omit the command "stsadm.exe -o execadmsvcjobs" if the “Microsoft SharePoint Timer” service is
   enabled.

To list the existing solutions, execute:

.. code-block:: batch

   stsadm.exe -o enumsolutions

To undeploy and delete the solution execute these commands in your
SharePoint server console:

.. code-block:: batch

   stsadm.exe -o retractsolution -name "denodowebpart.wsp" -immediate -allcontenturls
   stsadm.exe -o execadmsvcjobs   
   stsadm.exe -o deletesolution -name "denodowebpart.wsp"

You can omit the command "stsadm.exe -o execadmsvcjobs" if the “Microsoft SharePoint Timer” service is enabled.

 

