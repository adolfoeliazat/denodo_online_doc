===========================================
Exporting and Importing the Server Metadata
===========================================

.. toctree::
   :hidden:
   
   exporting_the_server_metadata/exporting_the_server_metadata.rst
   exporting_and_importing_elements_across_different_environments/exporting_and_importing_elements_across_different_environments.rst
   importing_metadata_into_a_server/importing_metadata_into_a_server.rst
   

You can export the metadata of the Server for backup purposes or to
re-create the same metadata in another Virtual DataPort server. You can
also export the metadata of one Virtual DataPort database or even just a
single element.

The export process can store the metadata in:

-  A single file, which will contain the VQL statements to recreate the
   metadata.
-  Two files: one that contains the VQL statements to recreate the
   metadata and another one that contains the value of the parameters
   that depend on the environment where the Server is running.
-  Or, to a **Repository** of files where the VQL statements that
   create each element are stored in a different file. Only use this
   option if you want to store the metadata on a version control system
   that is not supported by Virtual DataPort. To store the metadata in
   one of a supported VCS, use the VCS integration of the administration
   tool (see section :ref:`Version Control Systems Integration`)

The following sections explain how to:

*  Export the metadata from a Server
   (section :ref:`Exporting the Server Metadata`) 

*  Import it (section
   :ref:`Exporting and Importing Elements across Different Environments`).

