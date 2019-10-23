===================
Managing Extensions
===================


.. toctree::
   :hidden:

   adding_extensions.rst
   deleting_extensions.rst
   exporting_a_flow_as_an_extension.rst

The core functionality of ITPilot can be extended by the use of
extensions, that are JAR files containing Java classes that provide new
functionality not present in the original product, in the form of custom
functions. These classes must be created using the existing extension
points that ITPilot provide. The :doc:`/itpilot/developer/index` explains how to create the classes and
packaging them as extensions.



The extension management screen can be accessed by clicking the menu
option “Tools > Extensions…”. This screen provides means to add new
extensions and delete existing extensions from the environment.



The extensions window shows a list of all the already installed
extensions, under the Extensions header (the list will be empty in a
newly installed environment). When clicking any of the installed
extensions the functions that the extension provides will be shown in
the center list, under the Functions header. If any of the functions is
clicked, the Signatures list will show all the different signatures that
the function provides (as a function can have several signatures with
different number and/or types of parameters).
