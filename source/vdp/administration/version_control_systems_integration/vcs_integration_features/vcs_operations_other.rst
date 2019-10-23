================================
Other Version Control Operations
================================

This section describes commands that are common to all the supported Version
Control Systems.

.. contents::
   :depth: 1
   :local:
   :backlinks: none

Discard
================================================================================

The **Discard** operation discards local modifications and 
eliminations (i.e. non-committed changes):

* On single elements.

* On a folder and its contents.

* On a database and its contents.

The scope of a discard operation is restricted to the scope of the 
selected element: no dependencies outside of the scope will be considered 
for the discard operation.

.. note::
  
   The only exception to this behavior will happen when including 
   **global elements** in the discard operation. This will be the case if a
   database-level discard operation detects that there are elements depending on
   global elements with local changes: the operation will be aborted and you will 
   be asked to confirm if you wish to discard changes on the affected global 
   elements.  


This means that if you are, for instance, discarding local changes on 
a derived view that depends on a modified base view, only the derived view will 
be restored. This operation could fail (for example, the restored version of the 
derived view could be incompatible with the base view's current schema). In 
cases like this, you will be informed about the errors and about which are the
modified dependencies of each element which failed to be restored.

Regarding unversioned and synchronized elements:

* They are not affected by discard operations.

* They cannot be the target of a discard operation.
