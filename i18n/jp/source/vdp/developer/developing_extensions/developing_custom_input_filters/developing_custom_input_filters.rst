===============================
Developing Custom Input Filters
===============================

.. toctree::
   :hidden:

   required_libraries_to_develop_custom_filters.rst
   developing_custom_filters.rst

When creating a DF, JSON or XML data source, you can select an input
filter that preprocesses the data retrieved from the source, before the
Execution Engine processes it. Besides providing several out of the box
input filters, Virtual DataPort provides a Java API that allows you to
develop filters that preprocess the data in any way you need.

We strongly recommend using the Denodo4E plugin for Eclipse to develop custom input filters (see the file README in <DENODO_HOME>/tools/denodo4e).

Virtual DataPort includes a sample custom filter that reads the data
from the source and replaces one character with another one. This
example is at the folder
:file:`{<DENODO_HOME>}/samples/vdp/customConnectionFilter`. The ``README``
file in this directory explains how to compile, install and use this
custom filter.
