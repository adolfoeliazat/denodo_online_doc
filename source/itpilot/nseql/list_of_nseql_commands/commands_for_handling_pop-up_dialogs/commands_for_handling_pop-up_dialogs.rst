====================================
Commands for Handling Pop-Up Dialogs
====================================

List of commands for handling pop-up dialogs:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

ONDIALOG
=========================================

Looks for a dialog with an active MSIE browser child as its associated
window, and which belongs to the “#32770” class (“Dialog Box”). In case
the dialog is found and a value is provided, the command sets said value
in one edition field, and a button (that can be specified) is pressed.

**Parameters**

-  int buttonNumber: position of the dialog button to be pressed
   (starting by 0).
-  String content: content to place in the edition field. In case of not
   wanting to fill the edition field, or if the dialog does not include
   an edition control, this parameter must be assigned the reserved
   value “\_NULL\_”.

**Returns**

   Nothing.

.. note:: Not implemented for the Denodo browser.


ONPRINTDIALOG
=========================================

It finds a dialog of type IE Print Dialog (class “#32770”) child of the
active MSIE browser and simulates a command sequence on the window.

**Parameters**

-  int buttonPosition: position of the dialog button that must be
   pressed, starting by 0.
-  String printerName: optional attribute, refers to the name of the
   printer to be used (it might be \_NULL\_).
-  int printerPosition: indicates the position of the printer to use,
   starting by 0. If the name of the printer (parameter printername) is
   \_NULL\_, it considers all printers. Else, it only considers the
   printers with the specified name. If this parameter has a value -1,
   it uses the default printer.

**Returns**

   Nothing.

.. note:: This command is not implemented in the Denodo browser.


ONWEBPAGEDIALOG
=========================================

Finds a dialog with an active MSIE browser child as its associated
window, and belonging to the "Internet Explorer\_TridentDlgFrame“ class,
and simulates a command sequence on the dialog window.

Parameters:

-  String strCommands: string which contains a list of actions to be
   executed on the dialog. The possible actions are the following:

   1. Type a text character.

   #. Perform a specific action through a command. Each of the commands
      must be delimited by brackets. The commands available are:

      -  ENTER: simulates the pressing of the “ENTER” key.
      -  TAB: simulates the pressing of the tabulator key.
      -  UP: simulates the pressing of the ARROW UP key.
      -  DOWN: simulate the pressing of the ARROW DOWN key.
      -  WAIT: waits for a second before executing the following command.
      -  CLOSE: closes the dialog window.

   For instance, a possible action sequence is:
   “{TAB}text{TAB}{DOWN}{TAB}{UP}{CLOSE}”.

   Optionally, if a specific command is required to be executed a number of
   times, it can be specified by pointing out that number between <> (after
   the name of the command and inside of the brackets). For example,
   “{TAB<3>}” means that the TAB key must be pressed three consecutive
   times, just like “{TAB}{TAB}{TAB}”.

**Returns**

   Nothing.

.. note:: This command is not implemented in the Denodo browser.

.. important:: The symbols ``{`` and ``}`` must be
   escaped with ``\\\\``.


ONCERTIFICATEDIALOGDENODOBROWSER
=========================================

This command allows selecting one of the certificates contained in a
keystore to be used by the Denodo Browser when client authentication
using certificates is required.

**Parameters**

-  String type: type of the keystore to be used (supported types are
   “pkcs12” and “jks”).
-  String file: complete path to the file where the keystore is stored.
-  String password: password which protects the keystore.
-  String certName: name of the certificate to be used (OPTIONAL, it can
   be omitted if the keystore only contains one certificate).

**Returns**

   Nothing.

.. note:: This command is not implemented in MSIE.


SETDIALOGPROPERTIES
=========================================

Allows the modification of the configuration parameters for the dialog
manager.

**Parameters**

-  boolean synchronized: specifies whether the dialogs are managed
   sequentially or they are managed concurrently in case there are
   several active at the same time. The default value is false.
-  int timeout: maximum time it can take for a dialog to be filled. The
   default value is 15s.
-  boolean closeProxy: true if the proxy dialog should be automatically
   closed when it pops again after an authentication failure. (OPTIONAL,
   it can be omitted and its default value is true).

**Returns**

   (boolean): *true* if the command is successful, otherwise *false*.

.. note:: This command is not implemented in the Denodo browser.


SETSILENT
=========================================

Allows blocking the emergence of JavaScript dialogs, automatically
closing them when they appear.

**Parameters**

-  int block: this parameter accepts values 0, 1, 2 or 3. If it is 0,
   all dialogs will be shown. If the value is 1, all dialogs will be
   closed when they appear. If value is 2, JavaScript dialogs, like
   alert or confirm, will be closed when they appear, but control
   dialogs opened by the browser, like authentication dialogs or
   certificate related dialogs, will be shown. Finally, if value is 3,
   JavaScript dialogs, like alert or confirm, will be shown but control
   dialogs opened by the browser, like authentication dialogs or
   certificate related dialogs, will be closed as they appear.

**Returns**

   Nothing.

.. note:: This command is not implemented in the Denodo browser.

