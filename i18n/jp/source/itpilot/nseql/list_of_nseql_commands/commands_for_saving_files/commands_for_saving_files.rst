=========================
Commands for Saving Files
=========================

List of commands for saving files:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

SAVEFILE
=========================================

Downloads the contents of the previously selected link
(*SelectAnchorByXXX*), established target URL (*SetTargetURL*) or
canceled navigation (*CancelNextNavigation*) to the local file system.

**Parameters**

-  String filename: name of the file where the content is going to be
   saved.
-  String folder: name of the folder where the content is going to be
   saved.

**Returns**

   (String): Information about the downloaded file (size, content-type…)

