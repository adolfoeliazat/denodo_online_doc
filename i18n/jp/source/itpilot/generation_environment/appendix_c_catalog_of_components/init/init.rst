====
INIT
====

.. rubric:: Description

The Init component marks the beginning of the process. It defines the
input parameters for the wrapper.

.. rubric:: Input Parameters

The component has no inputs itself.

.. rubric:: Output Values

It outputs a record containing a field for each of the wrapper input
parameters which have been defined in the component’s wizard.

.. rubric:: Component Details

The Init component wizard allows to define the input parameters for the
wrapper. See section :ref:`Process initialization` for an example of how to
do this.

When the wrapper is executed, it will require values for the input
parameters defined in the Init component. See section :doc:`../../generation_environment_tools_-_part_i/wrapper_generation_tests_and_exporting/wrapper_execution` for an example.

The locale used to type input data is shown on the upper right corner of
the wizard. It can be changed in the Wrapper Options wizard (see section :doc:`Locale <../../generation_envirtonment_tools_-_part_ii/wrapper_advanced_options_specific_browser_pool_and_locale/locale>`).
