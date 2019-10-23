===================
Conversion Commands
===================

List of conversion commands:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

CONVERTEXCELTOHTML
=========================================

Converts the Microsoft Excel contents of the previously selected link
(*SelectAnchorByXXX*), established target URL (*SetTargetURL*) or
canceled navigation (*CancelNextNavigation*) to HTML, loading them in
the browser for subsequent processing.

**Parameters**

   None.

**Returns**

   (String): html code of the page generated.


CONVERTWORDTOHTML
=========================================

Converts the Microsoft Word contents of the previously selected link
(*SelectAnchorByXXX*), established target URL (*SetTargetURL*) or
canceled navigation (*CancelNextNavigation*) to HTML, loading them in
the browser for subsequent processing.

**Parameters**

   None.

**Returns**

   (String): html code of the page generated.


CONVERTPDFTOHTML
=========================================

Converts the PDF contents of the previously selected link
(*SelectAnchorByXXX*), established target URL (*SetTargetURL*) or
canceled navigation (*CancelNextNavigation*) to HTML, loading them in
the browser for subsequent processing.

**Parameters**

-  converterType: indicates the type of converter to be used. In the
   current version of ITPilot, the possible options are:

   1. *PDFBOX\_HTML* and *PDFBOX\_0\_7\_3\_HTML*: these two options
      configure the command to make use of version 0.7.3 of the PDFBox
      library (`Apache PDFBox - A Java PDF Library <https://pdfbox.apache.org/>`_) to
      generate the HTML code.
      
   2. *PDFBOX\_1\_X\_HTML*: configures the command to use the latest 1.x
      version of the PDFBox library supported by the tool (to maintain
      backward compatibility the option *PDFBOX\_1\_5\_HTML* can be still
      used, but with the same meaning as *PDFBOX\_1\_X\_HTML*). Currently
      the latest version which is supported is PDFBox 1.7.
      
   3. *ACR\_HTML*: configures the command to use the HTML converter of the
      Adobe Acrobat Professional software (this product must be installed).
      
   4. *ACR\_TXT*: configures the command to use the text converter of the
      Adobe Acrobat Professional software (this product must be installed).
      The obtained text is used by ITPilot to generate the resulting HTML
      code.
      
-  wordSeparator: This parameter indicates the separator written by the
   PDFBox extraction algorithm between “distant” words. It is an
   optional parameter; by default a space character is used.

**Returns**

   (String): html code of the page generated.

