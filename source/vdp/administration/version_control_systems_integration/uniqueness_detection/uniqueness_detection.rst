====================
Uniqueness Detection
====================

.. toctree::
   :hidden:
   
   enabling_uniqueness_detection.rst
   examples.rst

Ignore this section if you are using Git. It is only relevant for Microsoft TFS or Subversion.

File systems and version control systems allow the coexistence of files
with the same name as long as they are located in different folders.
Virtual DataPort is stricter regarding how elements can be named in the
context of a database:

-  Different folders can have the same name as long as their parent
   folders are different.
-  The names of views, data sources of the same type, web services, etc.
   cannot be duplicated in the same database in any case (e.g. it is
   forbidden to create a view named ``My_View`` in the folder
   ``Folder_1`` and another view ``My_View`` in ``Folder_2``).

This difference between file systems and Virtual DataPort makes
necessary to check if the elements included in a check-in exist in other
locations of the remote repository in order to avoid having duplicate
Virtual DataPort elements in the repository.

Virtual DataPort also has to check if the elements that are going to be
affected by a check-out that are either new (from a local point of view)
or locally modified, do exist in other locations of the remote
repository. It has to do this in order to avoid unexpected behaviors
(e.g. experiencing unexpected movements of elements between folders).

The uniqueness detection feature detects these scenarios and lets the
user choose what to do:

-  Cancel the check-in / check-out operation.
-  In check-ins, move the conflicting elements to the same locations as
   in the local database, and override them with the current changes.
-  In check-outs, move the conflicting elements to the same locations as
   in the remote repository, and override them with the remote changes.

By enabling the uniqueness detection, renaming folders or moving
elements between folders, no longer leads to unexpected behaviors (e.g.
unexpected movements of elements between folders after a check-out). As
explained above, uniqueness detection gives users control on how to deal
with this kind of scenarios.
