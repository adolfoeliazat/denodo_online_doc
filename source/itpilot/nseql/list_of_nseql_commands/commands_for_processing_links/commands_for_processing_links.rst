=============================
Commands for Processing Links
=============================

List of commands for processing links:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

CLICKONANCHORBYHREF
=========================================

Generates a *click* event on a link identified by its href attribute in
the current frame.

**Parameters**

-  String href: href attribute of the link.
-  int position: index of the element in the list of all links with the
   specified href (starting in 0).
-  boolean equals: indicates whether the href parameter should exactly
   match or just be contained in the link href.
-  int pages: number of download pages to be waited for after clicking
   on the link.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


CLICKONANCHORBYHREFREC
=========================================

Generates a *click* event on a link identified by its href attribute. It
recursively searches in all the frames, starting with the current frame.

**Parameters**

-  String href: href attribute of the link.
-  int position: index of the element in the list of all links with the
   specified href (starting in 0).
-  boolean equals: indicates if the href parameter should exactly match
   or be contained in the link href.
-  int pages: number of download pages to be waited for after clicking
   on the link.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


CLICKONANCHORBYPOSITION
=========================================

Generates a *click* event on a link identified by its position in the
current frame.

**Parameters**

-  int position: link position.
-  int pages: number of download pages to be waited for after *clicking*
   on the link.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.


CLICKONANCHORBYTEXT
=========================================

Generates a *click* event on a link identified by its associated text.
Should be in the current frame.

**Parameters**

-  String text: text of the link.
-  int position: index of the element in the list of all links with the
   specified text (starting in 0).
-  boolean equals: indicates if the text of the first parameter should
   exactly match the link text (true) or if it is enough for it to be
   contained in it (false).
-  int pages: number of download pages to be waited for after clicking
   on the link.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


CLICKONANCHORBYTEXTREC
=========================================

Generates a *click* event on a link identified by its associated text.
The search starts in the selected frame and recursively traverses all
the child frames.

**Parameters**

-  String text: text of the link.
-  int position: index of the element in the list of all links with the
   specified text (starting in 0).
-  boolean equals: indicates if the text of the first parameter should
   exactly match the link text (true) or if it is enough for it to be
   contained in it (false).
-  int pages: number of load pages to be waited for after clicking on
   the link.



**Returns**

   (int): time elapsed loading the pages, in milliseconds.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


CLICKONAREABYHREF
=========================================

Generates a *click* event on a map identified by its href attribute.

**Parameters**

-  String name: name of the map.
-  String href: name of the href of the area in the map.
-  boolean equals: indicates if the href parameter should exactly match
   or be contained in the map href.
-  boolean recursive: if it receives the true value, it recursively
   searches in all the child frames of the current frame.

**Returns**

   (boolean): *true* if the operation has been successful, otherwise
   *false*.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


CLICKONAREABYPOSITION
=========================================

Generates a *click* event on a map identified by its position in the
current frame.

**Parameters**

-  String name: map name.
-  int position: position of the area in the specified map.
-  boolean recursive: if the true value is received, it recursively
   searches in all the frames from the current frame.

**Returns**

   (boolean): *true* if the operation has been successful, otherwise
   *false*.


SELECTANCHORBYHREF
=========================================

Selects a link identified by its href attribute within the current
frame, which points to a document to be converted using the
*ConvertPDFToHtml*, *ConvertExcelToHtml* or *ConvertWordToHTML*
commands or saved with the *SaveFile* command.



**Parameters**

-  String href: href of the link.
-  int position: index in the list of all the links with the specified
   href (starting in 0).
-  boolean equals: This indicates whether the href parameter must match
   the link href exactly or be contained therein.
-  int events: Number of navigation start events the command will wait
   for before clicking on the link (OPTIONAL, it can be omitted).

**Returns**

   (boolean): *true* if the operation has been successful, *false*
   otherwise.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


SELECTANCHORBYHREFREC
=========================================

Selects a link identified by its href attribute. This seeks recursively
in all frames starting with the current one, which points to a document
to be converted using the *ConvertPDFToHtml*, *ConvertWordToHTML* or
*ConvertExcelToHTML* commands or saved with the *SaveFile* command.

Parameters:

-  String href: href of the link.
-  int position: index in the list of all the links with the specified
   href (starting in 0).
-  boolean equals: This indicates whether the href parameter must match
   the link href exactly or be contained therein.
-  int events: Number of navigation start events the command will wait
   for before clicking on the link (OPTIONAL, it can be omitted).

Returns:

   (boolean): *true* if the operation has been successful, *false*
   otherwise.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


SELECTANCHORBYPOSITION
=========================================

Selects a link identified by its position within the current frame,
which points to a document to be converted using the *ConvertPDFToHtml*,
*ConvertWordToHTML* or *ConvertExcelToHTML* commands or saved with the
*SaveFile* command.

**Parameters**

-  int position: position of the link.
-  int events: Number of navigation start events the command will wait
   for before clicking on the link (OPTIONAL, it can be omitted).

**Returns**

   (boolean): *true* if the operation has been successful, *false*
   otherwise.


SELECTANCHORBYTEXT
=========================================

Selects a link within the current frame, identified by its associated
text, which points to a document to be converted using the
*ConvertPDFToHtml,* *ConvertWordToHTML* or *ConvertExcelToHTML* commands
or saved with the *SaveFile* command.

**Parameters**

-  String text: text of the link.
-  int position: index of the element in the list of all links with the
   specified text (starting in 0).
-  boolean equals: specifies whether the text passed as the first
   parameter should exactly match the link text (true) or should simply
   be contained (false).
-  int events: Number of navigation start events the command will wait
   for before clicking on the link (OPTIONAL, it can be omitted).

**Returns**

   (boolean): *true* if the operation has been successful, *false*
   otherwise.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


SELECTANCHORBYTEXTREC
=========================================

Selects a link identified by its associated text, which points to a
document to be converted using the *ConvertPDFToHtml* or
*ConvertWordToHTML* commands or saved with the *SaveFile* command. It is
sought recursively in all the frames starting with the current one.

**Parameters**

-  String text: text of the link.
-  int position: index of the element in the list of all links with the
   specified text (starting in 0).
-  boolean equals: specifies whether the text passed as the first
   parameter should exactly match the link text (true) or should simply
   be contained (false).
-  int events: Number of navigation start events the command will wait
   for before clicking on the link (OPTIONAL, it can be omitted).

Returns:

   (boolean): *true* if the operation has been successful, *false*
   otherwise.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


SETTARGETURL
=========================================

Establishes the URL of a PDF, Microsoft Word or Microsoft Excel document
to be converted to HTML (*ConvertXXXToHtml* commands) or the URL of a
resource to be saved in the local filesystem (*SaveFile* command). It
is useful when the document is not accessible via any link or in case a
direct URL is provided and no previous browsing is required.

**Parameters**

-  String URL: string representing the URL of the document to be
   converted or saved.

**Returns**

   (boolean): *true* if the operation has been successful, *false*
   otherwise.
