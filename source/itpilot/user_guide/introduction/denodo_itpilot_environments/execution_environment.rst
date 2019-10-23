=========================
Execution Environment
=========================

This is the continued operation environment, in which the user can use
previously created wrappers to launch queries on isolated sources. This
use may be direct (through an API or publishing the wrapper as a Web
Service) or through other products such as Denodo Virtual DataPort, with
which Denodo ITPilot is fully integrated. The components that make up
this environment are as follows:

-  Wrapper Server: this is the component responsible for storing
   wrappers for accessing. These include a remote interface for
   statement execution.
-  Browser Pool: when a wrapper is executed, a browser type can be
   selected: MSIE browser (automatic navigation module based on
   Microsoft Internet Explorer) or Denodo browser (based in
   HTTP as an access method). In this case, the Wrapper Server uses the
   Browser Pool to minimize the time required to create browser
   instances. This pool can be configured from the administration tool.
-  PDF Conversion Server: this is the component responsible for
   transforming PDF documents to HTML using Adobe Acrobat Professional,
   so their content can be extracted by Denodo ITPilot.
