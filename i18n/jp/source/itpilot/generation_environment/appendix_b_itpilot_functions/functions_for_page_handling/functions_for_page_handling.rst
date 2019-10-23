===========================
Functions for Page Handling
===========================


-  ``CONVERTEXCELTOHTML``: this function receives as an argument a
   character string with the URL of the Microsoft Excel resource that is to be
   converted to HTML.

-  ``CONVERTPDFTOHTML``: this function receives two arguments. The first
   one is a character string with the URL of the resource in PDF format
   that is to be converted to HTML. The second is another character string
   indicating the type of converter to use. ITPilot provides the following
   converters:

   -  ACR\_TXT: Acrobat Text uses Adobe Acrobat Professional (which must be
      installed) to convert to plain text from which ITPilot generates an
      HTML file.
   -  ACR\_HTML: Acrobat HTML uses Adobe Acrobat Professional (which must
      be installed) to convert to HTML.
   -  PDFBOX\_HTML and PDFBOX\_0\_7\_3\_HTML: use the version 0.7.3 of
      PDFBox (`Apache PDFBox - A Java PDF Library <https://pdfbox.apache.org/>`_) to generate the HTML.
   -  PDFBOX\_1\_X\_HTML: uses the latest 1.x version of the PDFBox library
      supported
      by the tool, to generate the HTML (to maintain backward compatibility
      the option *PDFBOX\_1\_5\_HTML* can be still used, but with the same
      meaning as *PDFBOX\_1\_X\_HTML*). Currently the latest version which
      is supported is PDFBox 1.6.

-  ``CONVERTWORDTOHTML``: this function receives, as an argument, a
   character string with the URL of the Microsoft Word resource that is to be
   converted to HTML.

-  ``GETCOOKIES``: This function receives a ``Page``-type object as input
   argument and returns a character string with the current “cookies”. This
   function can only be used in Denodo Browser or browser pool connections
   (neither ftp nor local access ones).

-  ``GETLASTURL``: This function receives a ``Page``-type object as input
   argument and returns its URL as a character string. This function can
   only be used in Denodo Browser or browser pool connections (neither ftp
   nor local access ones).

-  ``GETLASTURLMETHOD``: This function receives a ``Page``-type object as
   input argument and returns its access method (GET or POST) as a
   character string. This function can only be used in Denodo Browser or
   browser pool connections (neither ftp nor local access ones).

-  ``GETLASTURLPOSTPARAMETERS``: This function receives a ``Page``-type
   object as input argument and returns a character string which represents
   the POST parameters that have been used to access that page. This
   function can only be used in Denodo Browser or browser pool connections
   (neither ftp nor local access ones).

-  ``GETMIMETYPE``: This function receives a ``Page``-type object as input
   argument and returns the MIME type of the page in a character string.

-  ``GETPAGETYPE``: This function receives a ``Page``-type object as input
   argument and returns an integer representing the type of “connection”
   used to obtain the page. The possible values are:

   -  1: The Page was obtained using a MSIE Browser.
   -  2: The Page was obtained using a Denodo Browser with the option
      “Never execute JavaScript” enabled.
   -  3: The Page was obtained using a Denodo Browser with the option
      “Never execute JavaScript” disabled.
   -  4: The Page was obtained using an FTP connection.
   -  5: The Page was obtained using a Local connection.

-  TOPAGE: This function returns a Page-type object whose state is
   represented by the input arguments. The function has the following
   signature:

   ``TOPAGE(int connection_type, string url, string url_method, string post_parameters, string cookies)``
   
   , where:

   -  ``connection_type``: type of connection that will be used to obtain the
      page. The possible values are the same as the ones returned by the
      ``GETPAGETYPE`` function.

   -  ``url``: Depending on the ``connection-type`` value it can specify the
      following:

      -  A HTTP or HTTPS URL pointing to a web page (``connection_types`` 1, 2
         or 3).
      -  A FTP, FTPS or SFTP URL pointing to an FTP resource
         (``connection_type`` 4), with the following format:
      -  ``protocol://[user[:pass]]@host[:port][/path]``
      -  A path to a local file (``connection_type`` 5).

   -  ``url_method``: HTTP access method (“GET” or “POST”) used to access to
      the page when it represents a web page. Its value is only taken into
      account when ``connection_type`` is 1, 2 or 3.

   -  ``post_parameters``: HTTP parameters used to access to the page when it
      represents a web page and the HTTP POST method is used. Its value is
      only taken into account when ``connection_type`` is 1, 2 or 3 and
      ``url_method`` is equals to “POST”.

   -  ``cookies``: HTTP cookies needed to access to the page when it
      represents a web page. Its value is only taken into account when
      ``connection_type`` is 1, 2 or 3.
