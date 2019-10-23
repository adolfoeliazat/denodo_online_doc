=================
Delete Operations
=================

When an element under version control is deleted, it is still shown in
the Server Explorer, marked with a gray icon (see section :ref:`Integration
with the Server Explorer`), although it does not exist anymore in the
server, so its details cannot be accessed from the Administration Tool.
When deleting an element, it is possible that other elements are also
deleted (because they depended on the element). If those elements are
under version control, they will also be shown in the Server Explorer,
marked with a gray icon.

Checking in a deleted item will delete it from the version control
server. Checking in a folder or a whole database will remove all the
deleted elements that it may contain.

