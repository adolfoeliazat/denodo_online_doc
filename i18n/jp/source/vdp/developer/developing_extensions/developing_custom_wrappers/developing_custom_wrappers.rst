==========================
Developing Custom Wrappers
==========================

.. toctree::
   :hidden:

   required_libraries_to_develop_custom_wrappers.rst
   extending_abstractcustomwrapper.rst
   overriding_abstractcustomwrapper.rst
   dealing_with_conditions.rst
   dealing_with_the_order_by_clause
   configuring_a_custom_wrapper.rst
   updating_the_custom_wrapper_plan.rst
   dealing_with_datetime_and_interval_types.rst


Virtual DataPort provides an API to develop custom wrappers to retrieve
data from sources that are not supported.

We strongly recommend using the Denodo4E plugin for Eclipse to develop
custom wrappers (see the file ``README`` in
:file:`{<DENODO_HOME>}/tools/denodo4e`).

To create a new Custom wrapper, also called Custom data source, extend the Java abstract class ``AbstractCustomWrapper``
(``com.denodo.vdb.engine.customwrapper``). This abstract class provides
a default implementation of the interface ``CustomWrapper``
(``com.denodo.vdb.engine.customwrapper``). Do *not* implement
the interface ``CustomWrapper``.

The following sections explain how to extend the
``AbstractCustomWrapper`` class.

Virtual DataPort includes a sample custom wrapper that retrieves data
from a Salesforce account. This example is at
:file:`{<DENODO_HOME>}/samples/vdp/customWrappers`. There is a ``README`` file
in this directory that explains how to compile, install and use this
Custom wrapper.
