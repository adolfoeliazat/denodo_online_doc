====================
Informative Commands
====================

List of informative commands:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

FINDCOOKIES
=========================================

Obtains the current cookies.

**Parameters**

   None.

**Returns**

   (String): list of cookies. The name and value of each is displayed
   separated by ‘;’.


GETEXTENDEDSOURCECODE
=========================================

Obtains the source code of the current frame, returning visualization
information along with the HTML tags: it adds four attributes to each
HTML tag with the X and Y positions (height and width).

**Parameters**

   None.

**Returns**

   -  (String): source code of the current page, selected frame or last
      loaded frame as result of clicking on a link.

.. note:: The Denodo browser executes this command like a regular
   GetSourceCode, ignoring the visualization information.


GETLASTURL
=========================================

Obtains the value of the last URL accessed by the browsing sequence.

**Parameters**

   None.

**Returns**

   -  (String) last URL that was reached in the sequence.


GETLASTURLPOSTPARAMETERS
=========================================

Returns a string containing the parameters of the http POST method for
the last URL accessed. If the last request was made using the http GET
method, an empty string will be returned.

**Parameters**

   None.

**Returns**

   (String) string with the POST parameters.


GETLOCATION
=========================================

Obtains the URL of the current frame.

**Parameters**

   None.

**Returns**

   (String): the URL of the current frame.


GETMIMETYPE
=========================================

Returns the MIME type for the current page.

**Parameters**

   None.

**Returns**

   (String): MIME type for the current page.


GETSELECTOPTIONS
=========================================

This obtains the index of the option selected in a SELECT-type element.

**Parameters**

-  String name: name of the SELECT-type element.
-  int position: position within the form of the same name.

**Returns**

   (int): index of the value selected


GETSOURCECODE
=========================================

Obtains the source code of the current frame.

**Parameters**

   None.

**Returns**

   (String): the source code of the current page or the last selected frame
   or the last frame loaded as a result of clicking on a link.


GETTITLE
=========================================

Obtains the title of the current frame.

**Parameters**

   None.

**Returns**

   (String): the title of the current frame.


ISDOWNLOADING
=========================================

Informs whether the current page has been fully loaded or not.

**Parameters**

   None.

**Returns**

   (boolean): true if the page is still being downloaded, false if it has
   been downloaded.


QUERYDOWNLOADEDPAGES
=========================================

Obtains the number of pages downloaded since the last navigation.

**Parameters**

   None.

**Returns**

   (int): the number of pages downloaded since the last navigation. If the
   Denodo browser is used, this command will always return 1.

