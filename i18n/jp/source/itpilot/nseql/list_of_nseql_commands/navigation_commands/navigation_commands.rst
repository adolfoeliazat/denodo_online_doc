===================
Navigation Commands
===================

List of navigation commands:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

CANCELNEXTNAVIGATION
=========================================

There are times in which the URL of a link, or the one that is
accessed after pressing a button, is unknown before executing the
navigation event. The reason may be that, for instance, a JavaScript
function is responsible for transforming the initial URL to another.
When the final URL must be obtained before executing the navigation
(e.g. to avoid the execution of a transaction before making sure of what
is the URL to be accessed), ITPilot provides the NSEQL command
CancelNextNavigation; this command indicates that the following
navigation (or more than one, by configuring an optional parameter) as
stated by other NSEQL commands, will be canceled just in the moment in
which the navigation is starting, and once every scripting language
pre-processing of that page has finished.

The two main reasons for using this command are the following:

#. To obtain data from a page (method +URL + parameters) without really
   performing the navigations. There are times in which the URL cannot
   be directly obtained from the HTML code since there is some script
   language processing. For instance, it might be useful to obtain the
   URL which is pointing to a link (generated via JavaScript), so that
   the linked document can be later downloaded without having to reach
   the document in the browser again.
#. To use along with the PDF-to-HTML and Word-to-HTML conversion
   commands (see section :ref:`CONVERSION COMMANDS`) and the *SaveFile*
   command (see section :ref:`COMMANDS FOR SAVING FILES`). These commands
   will perform the conversion of the document pointed out by the last
   cancelled navigation, or will save it to the local file system.

**Parameters**

-  int cancellations: number of windows in which the navigation will be
   canceled (the first ones that ITPilot will try to browse to). This
   parameter is useful if an action in a window causes more navigation
   processes in others, which must also be canceled (e.g. when the
   navigation involves a regular window and a pop-up window). (OPTIONAL,
   it can be omitted)

**Returns**

   (boolean): *true* if the operation has been successful, otherwise
   *false*.

.. note:: In the event of having to access cancelled information from a
   window different from the currently selected, a SelectWindow command
   must be used to select the correct window before finishing the sequence.

.. note:: If a CancelNextNavigation is used, a new window is opened
   (with the canceled navigation) and a new window is selected, cookies
   will not be appropriately returned. The sequence in the main window must
   be finished, cookies must be retrieved with an Expression component, a new sequence must be used to select the new
   window and, finally, an Expression component must be used to retrieve
   the desired page by using the ``TOPAGE`` and ``GETLASTURL`` method, plus
   the cookies retrieved by the previous Expression component.


CANCELNAVIGATION
=========================================

This command is similar to the CancelNextNavigation command but, instead
of canceling the following n navigations, it allows the n-1 first
navigations and cancels the n-th one. This command is useful to cancel
“automatic navigations” that are started while loading a new page.
Suppose we want to navigate to a page that contains a load event handler
which navigates to another page, but we want to cancel this “automatic
navigation”. Using the command CancelNavigation(2) the navigation to the
first page is allowed and the automatic navigation started from the load
event handler is canceled.

**Parameters**

-  int navigationNumber: the navigation to cancel (1 for the next
   navigation, 2 for the second…).

**Returns**

   (boolean): *true* if the operation has been successful, otherwise
   *false*.

.. note:: In the event of having to access canceled information from a
   window different from the currently selected, a SelectWindow command
   must be used to select the correct window before finishing the sequence.

.. note:: If a CancelNavigation is used, a new window is opened (with
   the canceled navigation) and a new window is selected, cookies will not
   be appropriately returned. The sequence in the main window must be
   finished, cookies must be retrieved with an Expression component (see
   :doc:`/itpilot/generation_environment/index`), a new sequence must be used to select the new
   window and, finally, an Expression component must be used to retrieve
   the desired page by using the ``TOPAGE`` and ``GETLASTURL`` method, plus
   the cookies retrieved by the previous Expression component.


EXTENDEDWAITPAGES
=========================================

Waits for a certain number of pages to download. The difference with
respect to the command WaitPages -below- is that this command can wait
for the required pages without the need to define the exact number.

**Parameters**

-  int pages: number of downloaded pages to wait for. A “-1” value tells
   the system to wait for any number of pages that this navigation
   requires.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.


GOBACK
=========================================

Returns to the previous page. Equivalent to clicking the browser ‘Back’
button.

**Parameters**

-  int pages: number of download pages to be waited for.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.


GOFORWARD
=========================================

Advances to the next page in the browser log. Equivalent to clicking the
browser ‘Forward’ button.

**Parameters**

-  int pages: number of download pages to be waited for.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.


NAVIGATE
=========================================

Accesses a URL

**Parameters**

-  String url: URL to be browsed.
-  int pages: number of download pages to be waited for.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.

.. note:: Using the prefix “javascript:” it is possible to specify a
   JavaScript expression (which must return a URL) as the value of the url
   parameter. In those cases the JavaScript expression is evaluated and the
   browser accesses to the resulting URL. For example:
   
   ::
   
      Navigate(javascript:return document.location.url + "search?q=java",0);
   
.. _nseql_guide_ignore_navigation_errors:

IGNORENAVIGATIONERRORS
=========================================

Allows users to decide if they want to ignore the navigation error
events returned by the browser. This kind of event is notified by the
browser when a browsing error happens while a “frame” is loading.
However, sometimes this error does not affect the browsing itself; in
these cases, it is useful to ignore it.

**Parameters**

-  boolean ignore: “true” if the error is to be ignored, “false”
   otherwise (by default).

**Returns**

   Nothing.


SETMAXDOWNLOADTIME
=========================================

Configures the maximum page download time; if a page takes longer to
download, a timeout error will be raised.

**Parametrers**

-  int maxDownloadTime: maximum page download time, in milliseconds.

**Returns**

   Nothing


NAVIGATEEXTENDED
=========================================

Accesses a URL

**Parameters**

-  String url: URL to be browsed.
-  String frame: name of the frame to be browsed (OPTIONAL, use \_NULL\_
   if no value is required).
-  String headers: additional http headers to be sent (OPTIONAL, use
   \_NULL\_ if no value is required).
-  int pages: number of download pages to be waited for.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.


POSTDATA
=========================================

Executes an http *post* request.

**Parameters**

-  String url: URL.
-  String postdata: field-value pairs with the format c1=v1&c2=v2&….
-  String headers: additional headers.
-  String target\_frame: target frame.
-  int pages: number of download pages to be waited for after posting.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.


REFRESH
=========================================

Reloads the current page. Equivalent to clicking on the browser
‘Refresh’ or ‘Reload’ button.

**Parameters**

-  int pages: number of download pages to be waited for.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.


STOP
=========================================

Stops browsing. Equivalent to clicking on the browser ‘Stop’ button.

**Parameters**

   None.

**Returns**

   Nothing.


WAIT
=========================================

This command pauses the navigation sequence for the desired amount of
time before continuing browsing.

**Parameters**

-  int ms: number of milliseconds that the command will pause before
   continuing with the browsing sequence.

**Returns**

   Nothing.


WAITPAGES
=========================================

Waits for a certain number of pages to download.

**Parameters**

-  int pages: number of download pages to be waited for.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.
