==========================
Generation Environment
==========================

This environment includes the group of components necessary for creating
wrappers from DEXTL data extraction specifications 
and NSEQL navigation sequences. The
components it uses are as follows:

-  Generation Tools: tools for generating data extraction specifications
   and navigation sequences are graphical applications that allow a
   non-technical user to create Web wrappers. For more information we
   recommend reading the :doc:`/itpilot/generation_environment/index`.
-  Generation Browser Pool: this environment uses a Browser Pool
   internally to check the navigation sequences and final specification.
-  PDF Conversion Server: this environment makes use of an embedded PDF
   conversion Server, which transforms PDF documents to HTML, using
   Adobe Acrobat Professional, and is automatically started. This server
   will be used by the Generation Tool to perform the required
   conversions. The execution time conversions are performed by the
   Execution Environment’s PDF conversion server, which is a separate
   component.



In addition, and although it does not belong to this environment *per
se*, generator tools may need to store the wrapper created. The Wrapper
Server in the Execution Environment is used to do this (see the next section).