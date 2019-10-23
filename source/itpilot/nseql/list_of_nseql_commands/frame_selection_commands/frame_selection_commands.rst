========================
Frame Selection Commands
========================

List of frame selection commands:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

FINDFRAMEBYNAME
=========================================

Searches for a frame by name in the contents of the current frame.

**Parameters**

-  String name: name of the frame.
-  int position: index in the list of all frames with the specified name
   (starting in 0).
-  boolean equals: specifies if the name received as a parameter should
   exactly match the name of a frame for the system to establish a
   coincidence (true) or if it is enough for it to be contained in same
   (false).

**Returns**

   (boolean): *true* if the frame was found.

.. note:: The *value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDFRAMEBYNAMEREC
=========================================

Recursively searches for a frame by name in all the frames (deep
search).

**Parameters**

-  String name: name of the frame.
-  int position: index in the list of all frames with the specified name
   (starting in 0).
-  boolean equals: specifies if the name received as a parameter should
   exactly match the name of a frame for the system to establish a
   coincidence (true) or if it is enough for it to be contained in same
   (false).

**Returns**

   (boolean): *true* if the frame was found.

.. note:: The *value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDFRAMEBYPOSITION
=========================================

Searches for a frame by position in the contents of the current frame.

**Parameters**

-  int position: frame position.

**Returns**

   (boolean): ``true`` if the frame was found.


FINDFRAMEBYSOURCE
=========================================

Searches for a frame by the *source* attribute in the contents of the
current frame.

**Parameters**

-  String source: source of the frame.
-  int position: index in the list of all frames with the specified
   source attribute (starting in 0).
-  boolean equals: specifies if the indicated text should exactly match
   the source attribute of the frame (true) or if it is enough that it
   is contained (false).

**Returns**

   (boolean): *true* if the frame was found.

.. note:: The *value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDFRAMEBYSOURCEREC
=========================================

Locates a frame by the ``source`` attribute recursively searching in all
the frames (deep search).

**Parameters**

-  String source: source of the frame.
-  int position: index in the list of all frames with the specified
   source attribute (starting in 0).
-  boolean equals: indicates if the indicated text should exactly match
   the source attribute of the frame (true) or if it is enough that it
   is contained (false).

**Returns**

   (boolean): *true* if the frame was found.

.. note:: The *value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


RESETFRAME
=========================================

Resets the selected frame, with “\_top” becoming the active frame.

**Parameters**

   None.

**Returns**

   Nothing.
