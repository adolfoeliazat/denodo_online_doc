=============================
Commands for Handling Windows
=============================

List of commands for handling windows:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

ALLOWOPENNEWWINDOWS
=========================================

Enables or disables the browser to open new windows.

**Parameters**

-  bool permit: true to allow the browser to open new windows and false
   to forbid it.

**Returns**

   (boolean): *true* if the operation was executed correctly, otherwise
   *false*.


CLOSEWINDOW
=========================================

Closes an active window in the current navigation sequence.

**Parameters**

-  int numberWindow: position of the window in the active window vector
   of the current navigation sequence. Window numbering starts at 0.

**Returns**

   (boolean): *true* if the operation was executed correctly, otherwise
   *false*.


GETWINDOWNAME
=========================================

Obtains the name of the current window.

**Parameters**

   None.

**Returns**

   (String): name of the current window.


OPENALERTDIALOGSINNEWWINDOW
=========================================

Redefines the JavaScript *alert()* function so that, instead of opening
a dialog, a new window is opened. This way, the content of the dialog
can be easily extracted with ITPilot.

**Parameters**

-  (boolean): if true, this indicates that the redefinition of the
   JavaScript alert() function is enabled so that a new window is
   opened. If false, the behavior will be the default one.

**Returns**

   None.


OPENNEWWINDOW
=========================================

Opens a new window in the current navigation sequence, placing it last
in the active window vector.

**Parameters**

-  String url: URL the new window will navigate to when it is first
   opened. If the URL contains the protocol (for example, \http:// or
   \https://) then the URL will be interpreted as absolute; otherwise,
   the URL will be relative to the URL of the selected window.
-  String name: currently unused.
-  String features: currently unused.
   int replace. Indicates if the new window should be opened on the same
   window (value 1) or not (value 0).

**Returns**

   Nothing.


OPENNEWWINDOWSUNNAMED
=========================================

Redefines the JavaScript *open()* function, so when new popup windows
are opened, they are always unnamed.

This is necessary because when there are multiple browsers, each of them
open their own popup windows. When a popup window is opened with the
*open()* function with a name for it, if there already exists another
window with that same name in the machine, the popup window will not
open (since it is considered that it already exists).

**Parameters**

-  bool open: when true, the open() function is redefined so the popup
   windows are opened with no name attached. When false, the behavior is
   the default one.

**Returns**

   Nothing.


OPENWEBPAGEDIALOGSINNEWWINDOW
=========================================

Redefines the JavaScript functions ``showModalDialog`` and
``showModelessDialog``, so their content is displayed in a new window.
By doing this, it is possible to find HTML elements, fire events on
them, etc., on “Web page” dialogs.

It cannot be used when the page that invokes these JavaScript functions
needs to:

-  Pass a value to the new dialog.
-  Obtain the value returned by the ``showModalDialog`` function.

.. note:: Execute the command ``AllowOpenNewWindows(true)`` before
   executing this one, to allow opening new windows. Otherwise this command
   will not work.

**Parameters**

-  bool open: if true, the “Web page” dialogs will be opened in new
   windows. If false, the dialogs will be opened in the current windows
   as usual.

**Returns**

   Nothing.


SELECTWINDOW
=========================================

Selects a window from the active windows in the current navigation
sequence.

**Parameters**

-  int numberWindow: position of the window in the active window vector
   of the current navigation sequence.

**Returns**

   (boolean): *true* if the operation was executed correctly, otherwise
   *false*.


SETWINDOWNAME
=========================================

Sets the name of the selected window.

**Parameters**

-  String name: the new window name.

**Returns**

   Nothing.


CLOSEINACTIVEWINDOWS
=========================================

Closes all windows but the active one, in sequences with more than one
window.

**Parameters**

   None.

**Returns**

   Nothing.

