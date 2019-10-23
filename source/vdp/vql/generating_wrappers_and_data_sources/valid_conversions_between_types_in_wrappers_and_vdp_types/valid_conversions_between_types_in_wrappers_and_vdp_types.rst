=========================================================
Valid Conversions Between Types in Wrappers and VDP Types
=========================================================


.. toctree::
   :hidden:

   native-type_conversions_of_a_wrapper_to_java_types.rst
   filters.rst

This section describes compatibility mappings between the Java types
exported by the wrappers and the data types used by Virtual DataPort in
the base relations and views (see section :ref:`Data Types`). When assigning
wrappers to base relations it is important to bear these compatibility
rules in mind to ensure that the defined schemas for the wrappers and
base relations are compatible.

The following table shows mappings of the more common types. These are
also the mappings applied automatically by the Virtual DataPort
Administration Tool (see Administration Guide).


.. table:: Automatic conversions between Java types and Virtual DataPort types
   :name: Automatic conversions between Java types and Virtual DataPort types
   
   +--------------------------------------+--------------------------------------+
   | Java Types                           | Virtual DataPort Types               |
   +======================================+======================================+
   | int, java.lang.Short,                | int                                  |
   | java.lang.Integer                    |                                      |
   +--------------------------------------+--------------------------------------+
   | long, java.lang.Long                 | long                                 |
   +--------------------------------------+--------------------------------------+
   | float, java.lang.Float               | float                                |
   +--------------------------------------+--------------------------------------+
   | double, java.lang.Double             | double                               |
   +--------------------------------------+--------------------------------------+
   | boolean, java.lang.Boolean           | boolean                              |
   +--------------------------------------+--------------------------------------+
   | java.math.BigDecimal                 | decimal                              |
   +--------------------------------------+--------------------------------------+
   | java.lang.String                     | text                                 |
   +--------------------------------------+--------------------------------------+
   | java.util.Date, java.util.Calendar,  | date                                 |
   | java.sql.Date, java.sql.Timestamp,   |                                      |
   | java.sql.Time                        |                                      |
   +--------------------------------------+--------------------------------------+
   | byte[], java.sql.Blob                | blob                                 |
   +--------------------------------------+--------------------------------------+

Any other java data type not specified in this table will be associated
by default to the VDP data type ``text``.

Other possible mappings exist between Java types and Virtual DataPort
types that can be specified but that are not applied automatically.
These can be seen in the following table.

.. table:: Other valid conversions between Java types and Virtual DataPort types
   :name: Other valid conversions between Java types and Virtual DataPort types

   +--------------------------------------+--------------------------------------+
   | Java Types                           | Virtual DataPort Types               |
   +======================================+======================================+
   | java.lang.String                     | xml                                  |
   +--------------------------------------+--------------------------------------+



Likewise, wrappers can provide compound elements such as arrays and
registers that are directly associated with VDP arrays and registers.

