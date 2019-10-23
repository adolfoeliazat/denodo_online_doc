============================
Commands for Selecting Forms
============================

List of commands for selecting forms:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

FINDFORMBYACTION
=========================================

Searches for a form by its ``action`` attribute in the current frame.

**Parameters**

-  String action: action of the search form.
-  int position: index of the element in the list of all forms with the
   specified action (starting in 0).
-  boolean equals: specifies if the indicated text should exactly match
   or only be contained in the form action.

**Returns**

   (boolean): *true* if the form was found, *false* otherwise.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDFORMBYACTIONREC
=========================================

Recursively searches for a form by its ``action`` attribute from the
current frame.

**Parameters**

-  String action: action of the search form.
-  int position: index of the element in the list of all forms with the
   specified action (starting in 0).
-  boolean equals: indicates whether the indicated text should exactly
   match or just be contained in the form action.

**Returns**

   (boolean): *true* if the form was found, *false* otherwise.

.. note:: The ``href`` parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDFORMBYNAME
=========================================

Searches for a form by name in the current frame.

**Parameters**

-  String name: name of the form.
-  int position: index of the element in the list of all forms with the
   specified name (starting in 0).

**Returns**

   (boolean): ``true`` if the form was found, ``false`` otherwise.


FINDFORMBYNAMEREC
=========================================

Recursively searches for a form by name starting in the current frame.

**Parameters**

-  String name: name of the form.
-  int position: index of the element in the list of all forms with the
   specified name (starting in 0).

**Returns**

   (boolean): *true* if the form was found, *false* otherwise.


FINDFORMBYPOSITION
=========================================

Searches for a form by position in the current frame.

**Parameters**

-  int position: position of the form within the current frame.

**Returns**

   (boolean): *true* if the form was found, *false* otherwise.


RESETFORM
=========================================

Resets the selected form.

**Parameters**

   None.

**Returns**

   Nothing.

.. note:: It is also possible to select a form by making use of the
   *FINDELEMENTBYXXX* commands (see section :ref:`COMMANDS FOR SELECTING
   ELEMENTS`).
