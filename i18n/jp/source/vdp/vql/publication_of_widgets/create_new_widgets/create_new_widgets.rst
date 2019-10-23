==================
Create New Widgets
==================

.. note:: Publishing views as widgets is a deprecated feature and it may be removed in future
   major versions of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

Use the statement ``CREATE WIDGET`` to create a new widget.

.. code-block:: bnf
   :caption: Syntax of the CREATE WIDGET statement
   :name: Syntax of the CREATE WIDGET statement

   CREATE [ OR REPLACE ] WIDGET <name:identifier>
       [ DISPLAYNAME = <literal> ]
       [ DESCRIPTION = <literal> ]
       ELEMENTTOPUBLISH = [ <database:identifer>.]<element name:identifier>
       ELEMENTTOPUBLISHTYPE = { VIEW | STOREDPROCEDURE }
       CHUNKSIZE = <integer>
       CHUNKTIMEOUT = <integer>
       QUERYTIMEOUT = <integer>
       POOLENABLED = <boolean>
       POOLINITSIZE = <integer>
       POOLMAXACTIVE = <integer>
       I18N = <identifier>
       HELPMODEENABLED = <boolean>
       [ CUSTOMIZEDHELPMODECONTENTS = <html_fragment:literal> ]
       [ OPTIONS
         ( PORTLETJSR286 ( PUBLISHCUSTOMTABLEEVENT = <boolean> )
         )
       ]

Below is a brief description of some of the parameters of this
statement:

-  ``ELEMENTTOPUBLISH``: Name of the view (optionally prefixed with the
   identifier of a database), base view or stored procedure to publish.
-  ``HELPMODEENABLED``: Enables the Help Mode for the widget. In the
   three widget platforms, a widget might have a help mode used to
   display information about the widget. If this parameter is ``true``,
   but the ``CUSTOMIZEDHELPMODECONTENTS`` parameter is not present, the
   widget’s help mode will display a text with instructions on how to
   use the widget.
-  ``CUSTOMIZEDHELPMODECONTENTS``: HTML fragment that will be shown when
   the user opens the widget’s Help Mode. This parameter is only useful
   if ``HELPMODEENABLED`` is ``true``.
-  ``CHUNKSIZE``, ``CHUNKTIMEOUT``, ``QUERYTIMEOUT``: Their
   interpretation is the same as in any other Virtual DataPort client.
-  ``POOLENABLED``: If ``true``, the connection pool will be enabled
   (highly recommended).
-  ``POOLINITSIZE``: Initial number of connections to be opened in the
   pool.
-  ``POOLMAXACTIVE``: Maximum number of connections in the pool. If this
   is a negative value, then the number is not limited.
-  ``PUBLISHCUSTOMTABLEEVENT``: Enables the option that the widget
   exported to a JSR-286 Portlet can send complex objects to other
   portlets. These complex objects contain the whole result of the
   executed query view/stored procedure obtained from Virtual DataPort
   (see section 
   :doc:`Export to JSR-168 or JSR-286 Portlet <../../../administration/publication_of_views_as_widgets/export_to_jsr-168_or_jsr-286_portlet/export_to_jsr-168_or_jsr-286_portlet>`
   of the Administration Guide).

