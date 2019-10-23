===================================
Generic Support for Other Databases
===================================

Virtual DataPort can store the cached data in several database
management systems (DBMS) out of the box. In addition, you can configure
Virtual DataPort to store the cached data in any other DBMS.

To store the cache in another DBMS, configure the cache of the
Server/database to use the **Generic DB Adapter**. When using this
adapter, the Server uses the configuration set in the following files:

-  :file:`{<DENODO_HOME>}/conf/vdp/cacheConfig-generic.xml`
-  Or :file:`{<DENODO_HOME>}/conf/vdp/cacheConfig-generic-unicode.xml`

The structure of both configuration files is the same. The Server reads
the configuration from the first file when the **UTF-8 data types**
check box is cleared and from the second one when this check box is
selected. The Server has two different files because usually, databases
use different data types for columns configured to store UTF-8 strings.

When you enable the cache engine, the Server creates several tables in
the external DBMS that will be used to keep track of the name of the
table where the cached data of each view is stored. In addition, when
you enable the cache for a view, the Server creates a table with the
same schema as the view.

These configuration files define the type of column in the DBMS used to
store each type of field: ``text``, ``int``, ``float``, ``double``, etc.

In these configuration files, you usually have to customize the value of
the elements ``type``, of the element ``datatypes``. For example, the
following element defines that the columns that store fields of type
``text`` will be created with the type ``VARCHAR(4000)``.

.. code-block:: xml

   <datatypes>
   ...
   <type>
       <vdb>MetaType.TEXT</vdb>
       <sql>VARCHAR(4000)</sql>
   </type>
