======================
Publication of Widgets
======================


.. toctree::
   :hidden:

   create_new_widgets/create_new_widgets.rst
   export_a_widget/export_a_widget.rst
   deployment_and_export_of_auxiliary_web_services/deployment_and_export_of_auxiliary_web_services.rst

.. note:: Publishing views as widgets is a deprecated feature and it may be removed in future
   major versions of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

Virtual DataPort can publish a view or a stored procedure as a widget.

Once a widget is deployed, it allows querying the content of the desired
element. It contains both a query form and a table to visualize the
obtained results. The widgets are compliant with the most common widget
inter-operability standards; therefore, they can collaborate with other
independently developed widgets (see section :ref:`Publication of Views as
Widgets` of the Administration Guide).

The widgets can be exported to different widget technologies:

-  Java Portal Servers (Portlets `JSR-168 <https://jcp.org/en/jsr/detail?id=168>`_ and `JSR-286 <https://jcp.org/en/jsr/detail?id=286>`_)
-  Microsoft SharePoint

.. note:: We strongly recommend publishing widgets using the
   Administration Tool (see the section :ref:`Publication of Views as Widgets`
   of the Administration Guide). That way, all the necessary VQL
   statements are generated automatically.
