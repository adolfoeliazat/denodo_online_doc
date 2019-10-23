====================
Extensions (Plugins)
====================

.. toctree::
   :hidden:
   
   filters.rst
   exporters.rst
   handlers.rst
   aracne_custom_crawlers.rst

Denodo Scheduler allows users to create their own filters (deprecated),
exporters, handlers or crawlers (deprecated).

.. important:: Filters and crawlers are deprecated and will be removed in a later version of the Denodo Platform.
   If you still want to implement a new one, you can see the documentation from 
   `previous version <https://community.denodo.com/docs/html/browse/6.0/scheduler/administration/developer_api/extensions_plugins/extensions_plugins#extensions-plugins>`_.

The following sections describe how to implement a new :ref:`exporter <exporters>` or :ref:`handler <handlers>`.

Denodo4E, an Eclipse plug-in which provides tools for creating,
debugging and deploying Denodo extensions, including exporters and
handlers, is included in the Denodo Platform. Read
the README in :file:`{<DENODO_HOME>}/tools/denodo4e` for more information.

Once the extension is implemented, it needs to be packed in a JAR file
along with an XML file that specifies its configuration metadata (the
extension type, its name, and its input parameters), and, optionally,
another XML file that specifies the default values of the input
parameters specified in the metadata. The metadata is used by the
administration tool to configure the extension through an auto-generated
configuration wizard (metadata specifies the value of the parameters the
extension receives in its ``init`` method), and the default values can
be used to initially set the values of the parameters in the
configuration wizard.

 

The name and location of the metadata file must be the same as that of
the class that implements the extension (i.e. in the same package as the
implementation class). It must follow the DTD ``custom-metaconfig.dtd``
which can be found in the JAR file
:file:`{<DENODO_HOME>}/lib/contrib/denodo-configuration.jar`. In this XML file
the ``element`` tag allows the type (``type``) and subtype (``subType``)
to be specified for the extension. Each configuration parameter is
specified using the ``param`` element, whose attributes are the
parameter name (``name``), its obligatoriness (``mandatory``), if it can
have multiple values (``multivalued``), its Java type (``javaType``), if
it contains sensitive data (``hidden``), the label to be displayed in
the configuration wizard (``displayName``), and a help text to be shown
as a tooltip in the configuration wizard (``helpText``). Hidden
parameters’ values will be shown as strings of dots in the configuration
wizard, and if no ``displayName`` is specified, then the parameter
``name`` will be used as the parameter label. If a parameter is
compound, the components of the parameter are listed within the
``components`` element.

The following metadata XML file for the XMLCustomHandler handler
included in the Scheduler samples is shown as an example. As can be seen, it is a
handler element called "xml-custom-handler", which has as input parameters the input and output file paths.

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   
   <!DOCTYPE metaconfig SYSTEM "custom-metaconfig.dtd">
   
   <metaconfig>
     <element type="handler" subType="xml-custom-handler"
   	  class="com.denodo.scheduler.demo.XMLCustomHandler" />
   	  
     <param javaType="java.lang.String" mandatory="false" multivalued="false"
   	  name="Input file absolute path" />
     <param javaType="java.lang.String" mandatory="true" multivalued="false"
   	  name="Output XML file absolute path" />
  </metaconfig>
 

.. important:: The export/import process (see section :ref:`Connection details`
   for details) attempts to convert from id to name and then from name to
   id the values of all parameters called “datasource” (because the id of a
   data source will be different for each installation). So, if you need to
   use a data source’s id as the value for an extension’s parameter, you
   should call it “datasource”. If you need to use a data source’s name as
   the value for an extension’s parameter, then you should use a parameter
   name different to “datasource” (otherwise, after exporting and then
   importing a job using this extension, the value of the “datasource”
   parameter would be replaced by an id).

 

The optional default values file must be placed in the same directory
than the metadata file and must have the same name prefixed by
“default\_”. It must follow the DTD ``default-custom-config.dtd`` which
can also be found in the JAR file
:file:`{<DENODO_HOME>}/lib/contrib/denodo-configuration.jar`. In this XML file
the ``simple-param`` and ``compound-param`` tags are used to set the
default value of simple and compound parameters, respectively. Both
accept an attribute (``name``) to specify the name of the parameter. The
value is specified using a nested ``value`` tag. In the case of the
simple parameters the ``value`` tag contains the parameter value itself,
and in the case of the compound parameters it contains nested
``simple-param`` or ``compound-param`` tags to specify the values of the
parameters composing it. Only one default value can be specified for
both simple and compound parameters, even though they are multivalued.
If the parameter is multivalued, the attribute ``instances`` can be used
in the parameter tag to specify the number of parameters of that type to
be initially created (its default value is 1, and it can be set to 0 if
no parameters of that type should be created). All the instances of the
multivalued parameters, including those added later by the user, will be
initially created with the default values specified for the parameters
composing it. If the parameter is not multivalued but it is optional,
the attribute ``instances`` can be used to specify if the parameter
should be initially created (``instances`` should be set to 1) or not
(``instances`` should be set to 0).

 

The following default values XML file could be used with the XMLCustomHandler
handler shown previously. As can be seen, it specifies “/tmp” as
the default value for the ``Output XML file absolute path`` parameter.


.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   
   <!DOCTYPE metaconfig SYSTEM "default-custom-config.dtd">
   
   <default-parameters>
   
       <simple-param name="Output XML file absolute path">
           <value>/tmp</value>
       </simple-param>
   
   </default-parameters>
 

Finally, in the ``META-INF/MANIFEST.MF`` JAR file the following metadata
needs to be specified:

-  **Name: CustomElement**
-  **PluginType**: exporter \| handler
-  **PluginName**: Name of the extension (it will be used as name for
   the extension in the administration tool).
-  **PluginClass**: Name of the extension implementation class.

 

The following MANIFEST.MF fragment for the XMLCustomHandler handler included
in the Scheduler samples is shown as an example.

.. code-block:: none

   Manifest-Version: 1.0
   ...
   
   Name: CustomElement
   PluginType: handler
   PluginName: XMLCustomHandler
   PluginClass: com.denodo.scheduler.demo.XMLCustomHandler
 

Section :ref:`Plugins` describes how to install a new extension in
Scheduler.
