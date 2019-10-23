============================================
Configuring the Default Internationalization
============================================

The Virtual DataPort server internationalization configuration specifies
aspects such as time zones.

Virtual DataPort includes typical internationalization configurations
and you can also create new *ad hoc* configurations (see section :ref:`Managing Internationalization Configurations` of the VQL Guide).

A specific internationalization configuration can also be specified for
each Virtual DataPort database (see section :ref:`Configuring and Deleting
Databases`)

.. note:: It is also possible to specify a different
   internationalization configuration for each base view (see section
   :ref:`Internationalization Configuration`).

The Administration Tool also has its own internationalization
configuration that can be changed on the tab **Locale** of the dialog
**Tools** > **Admin Tool Preferences** (see section :doc:`Locale </vdp/administration/installation_and_execution/launching_the_virtual_dataport_administration_tool/tool_preferences>`)

.. important:: When you change the i18n of the Denodo server, Denodo invalidates the cache of the views that have one or more fields of type timestamptz and/or date (deprecated type).

   This occurs when you change this from the administration tool or when you execute the following statement and the existing value is different than the new one:

   .. code-block:: vql
   
      SET 'com.denodo.vdb.misc.I18nParametersManager.i18nDefault'='<new i18n map>';
