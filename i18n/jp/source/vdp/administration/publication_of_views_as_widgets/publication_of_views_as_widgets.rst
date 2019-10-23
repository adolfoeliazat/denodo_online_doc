===============================
Publication of Views as Widgets
===============================

.. note:: Publishing views as widgets is a deprecated feature and it may be removed in future
   major versions of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

Virtual DataPort allows a view or stored procedure to be published as a
visual widget. Then, these widgets can be exported to different widget
technologies:

-  Java Portal Servers (`Portlets JSR-168 <https://jcp.org/en/jsr/detail?id=168>`_ and
   `JSR-286 <https://jcp.org/en/jsr/detail?id=286>`_)
-  `Microsoft SharePoint <http://sharepoint.microsoft.com/>`_ Web Parts

These widgets allow querying and displaying the data of the selected
view and can interact with other widgets sending them the data obtained
from the view or receiving it to filter the contents of the view that
are shown.

.. toctree::
   :maxdepth: 1
   
   publish_a_view_as_a_widget/publish_a_view_as_a_widget.rst
   auxiliary_web_services/auxiliary_web_services.rst
   export_to_jsr-168_or_jsr-286_portlet/export_to_jsr-168_or_jsr-286_portlet.rst
   export_as_microsoft_web_part/export_as_microsoft_web_part.rst
   deployment_of_a_microsoft_web_part/deployment_of_a_microsoft_web_part.rst  
