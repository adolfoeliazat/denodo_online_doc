====================
Extraction Functions
====================

-  ``GETPDFFIELDS``: it extracts input fields from PDF forms. It has two
   signatures:

   -  ``GETPDFFIELDS(string inputFile)``: it returns an array of registers.
      Each register will include two fields (NAME and VALUE) with the name
      and value of each input field of the PDF form included in the input
      file.
   -  ``GETPDFFIELDS(string inputFile, string ... outputFields)``: it
      returns a single register including only the form input fields
      specified in the input parameter ‘outputFields’. The field names of
      the output register will be the PDF form field names and the field
      values will be the PDF form field values.

-  ``XPATH``: It returns a character string containing the outer HTML of
   the node identified by the XPath expression (``string``-type) received
   as first argument on the ``Page``-type object received as second
   argument If the XPath expression returns more than one element, the
   first one will be selected. This function can only be used with ``Page``
   objects obtained via Denodo Browser or Browser pool sequence types.

   The syntax of the XPath expressions supported by this function is a
   subset of the XPath 1.0 specification (https://www.w3.org/TR/xpath). The
   supported features are the same as the ones explained for the NSEQL
   command :ref:`FindElementByXPath <itpilot_nseql_guide_findelementbyxpath>` in the NSEQL Manual. The Sequences Generator Toolbar includes a tool

-  ``XPATHLIST``: It returns a list of records (with one field named
   ‘Content’) containing the outer HTML of the nodes identified by the
   XPath expression (``string``-type) received as first argument, on the
   ``Page``-type object received as second argument. This function can only
   be used with ``Page`` objects obtained via Denodo Browser or Browser
   pool sequence types.

-  ``GETPDFFIELDS(string inputFile)``: it returns an array of registers.
   Each register will include two fields (NAME and VALUE) with the name
   and value of each input field of the PDF form included in the input
   file.
-  ``GETPDFFIELDS(string inputFile, string ... outputFields)``: it
   returns a single register including only the form input fields
   specified in the input parameter ‘outputFields’. The field names of
   the output register will be the PDF form field names and the field
   values will be the PDF form field values.

-  ``XPATH``: It returns a character string containing the outer HTML of
   the node identified by the XPath expression (``string``-type) received
   as first argument on the ``Page``-type object received as second
   argument If the XPath expression returns more than one element, the
   first one will be selected. This function can only be used with ``Page``
   objects obtained via Denodo Browser or Browser pool sequence types.

   The syntax of the XPath expressions supported by this function is a
   subset of the XPath 1.0 specification (https://www.w3.org/TR/xpath). The
   supported features are the same as the ones explained for the NSEQL
   command :ref:`FindElementByXPath <itpilot_nseql_guide_findelementbyxpath>` in the NSEQL Manual.

