==================================================
Native-type Conversions of a Wrapper to Java Types
==================================================

Each wrapper type has its own associations between native types of the
sources modeled and java types. The following sections show the
conversions applied to the different wrapper types supported by Virtual
DataPort.

In general, for those wrappers that access sources that may return
objects or arrays of objects the wrapper is responsible for representing
these structures using Virtual DataPort registers and arrays
respectively.

Type Conversion Tables for JDBC Wrappers
=================================================================================

.. table:: Type Conversion Tables for JDBC Wrappers
   :name: Type Conversion Tables for JDBC Wrappers

   +--------------------------------------+--------------------------------------+
   | JDBC types                           | Java types                           |
   +======================================+======================================+
   | ARRAY                                | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | BIGINT                               | java.lang.Long                       |
   +--------------------------------------+--------------------------------------+
   | BINARY                               | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | BIT                                  | java.lang.Boolean                    |
   +--------------------------------------+--------------------------------------+
   | BLOB                                 | byte[]                               |
   +--------------------------------------+--------------------------------------+
   | BOOLEAN                              | java.lang.Boolean                    |
   +--------------------------------------+--------------------------------------+
   | CHAR                                 | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | CLOB                                 | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | DATALINK                             | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | DATE                                 | java.sql.Date                        |
   +--------------------------------------+--------------------------------------+
   | DECIMAL                              | java.lang.Double                     |
   +--------------------------------------+--------------------------------------+
   | DISTINCT                             | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | DOUBLE                               | java.lang.Double                     |
   +--------------------------------------+--------------------------------------+
   | FLOAT                                | java.lang.Float                      |
   +--------------------------------------+--------------------------------------+
   | INTEGER                              | java.lang.Integer                    |
   +--------------------------------------+--------------------------------------+
   | Java\_OBJECT                         | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | LONGVARBINARY                        | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | LONGVARCHAR                          | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | NULL                                 | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | NUMERIC                              | java.lang.Double                     |
   +--------------------------------------+--------------------------------------+
   | OTHER                                | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | REAL                                 | java.lang.Float                      |
   +--------------------------------------+--------------------------------------+
   | REF                                  | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | SMALLINT                             | java.lang.Short                      |
   +--------------------------------------+--------------------------------------+
   | STRUCT                               | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | TIME                                 | java.sql.Time                        |
   +--------------------------------------+--------------------------------------+
   | TIMESTAMP                            | java.sql.Timestamp                   |
   +--------------------------------------+--------------------------------------+
   | TINYINT                              | java.lang.Byte                       |
   +--------------------------------------+--------------------------------------+
   | VARBINARY                            | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | VARCHAR                              | java.lang.String                     |
   +--------------------------------------+--------------------------------------+

Other types are converted to ``java.lang.String``.

.. note:: The table shows the generic conversions associated to JDBC
   sources. Depending on the vendor and the version of the database which
   is being accessed, these conversions may vary slightly.


Type Conversion Table for ODBC Wrappers
=================================================================================

For ODBC wrappers the same conversions are applied as for the JDBC
wrappers.


Type Conversion Table for Web Source Wrappers
=================================================================================

Web wrappers use the following type conversion table:

.. table:: ITPilot-type conversions
   :name: ITPilot-type conversions

   +--------------------------------------+--------------------------------------+
   | ITPilot Types                        | Java Types                           |
   +======================================+======================================+
   | boolean                              | boolean                              |
   +--------------------------------------+--------------------------------------+
   | date                                 | java.util.Calendar                   |
   +--------------------------------------+--------------------------------------+
   | double                               | double                               |
   +--------------------------------------+--------------------------------------+
   | float                                | float                                |
   +--------------------------------------+--------------------------------------+
   | int                                  | int                                  |
   +--------------------------------------+--------------------------------------+
   | string                               | java.lang.String                     |
   +--------------------------------------+--------------------------------------+
   | url                                  | java.lang.String                     |
   +--------------------------------------+--------------------------------------+




Type Conversion Table for Web Services Wrappers
=================================================================================


.. table:: Type Conversion Table for Web Services Wrappers
   :name: Type Conversion Table for Web Services Wrappers
   
   +--------------------------------------+--------------------------------------+
   | SOAP Types                           | Java Types                           |
   +======================================+======================================+
   | xsd:base64Binary                     | byte[]                               |
   +--------------------------------------+--------------------------------------+
   | xsd:boolean                          | boolean                              |
   +--------------------------------------+--------------------------------------+
   | xsd:byte                             | byte                                 |
   +--------------------------------------+--------------------------------------+
   | xsd:dateTime                         | java.util.Calendar                   |
   +--------------------------------------+--------------------------------------+
   | xsd:decimal                          | java.math.BigDecimal                 |
   +--------------------------------------+--------------------------------------+
   | xsd:double                           | double                               |
   +--------------------------------------+--------------------------------------+
   | xsd:float                            | float                                |
   +--------------------------------------+--------------------------------------+
   | xsd:hexBinary                        | byte[]                               |
   +--------------------------------------+--------------------------------------+
   | xsd:int                              | int                                  |
   +--------------------------------------+--------------------------------------+
   | xsd:integer                          | java.math.BigInteger                 |
   +--------------------------------------+--------------------------------------+
   | xsd:long                             | long                                 |
   +--------------------------------------+--------------------------------------+
   | xsd:QName                            | java.lang.String with format         |
   |                                      | “{namespace}localPart”               |
   +--------------------------------------+--------------------------------------+
   | xsd:short                            | short                                |
   +--------------------------------------+--------------------------------------+
   | xsd:string                           | java.lang.String                     |
   +--------------------------------------+--------------------------------------+



Compound elements are converted to Java objects by following the
standard *mapping* defined by the `JAX-RPC standard <https://docs.oracle.com/cd/E17802_01/webservices/webservices/docs/1.6/jaxrpc/>`_.


Type Conversion Table for XML Wrappers
=================================================================================


.. table:: Type Conversion Table for XML Wrappers
   :name: Type Conversion Table for XML Wrappers
   
   +--------------------------------------+--------------------------------------+
   | XML/Schema Types                     | Java Types                           |
   +======================================+======================================+
   | positiveinteger                      | java.lang.Integer                    |
   |                                      |                                      |
   | negativeinteger                      |                                      |
   |                                      |                                      |
   | nonpositiveinteger                   |                                      |
   |                                      |                                      |
   | nonnegativeinteger                   |                                      |
   |                                      |                                      |
   | int                                  |                                      |
   |                                      |                                      |
   | unsignedint                          |                                      |
   |                                      |                                      |
   | gYear                                |                                      |
   |                                      |                                      |
   | gMonth                               |                                      |
   |                                      |                                      |
   | gDay                                 |                                      |
   +--------------------------------------+--------------------------------------+
   | long                                 | java.lang.Long                       |
   |                                      |                                      |
   | unsignedlong                         |                                      |
   +--------------------------------------+--------------------------------------+
   | byte                                 | java.lang.Byte                       |
   |                                      |                                      |
   | unsignedbyte                         |                                      |
   +--------------------------------------+--------------------------------------+
   | double                               | java.lang.Double                     |
   +--------------------------------------+--------------------------------------+
   | float                                | java.lang.Float                      |
   +--------------------------------------+--------------------------------------+
   | short                                | java.lang.Short                      |
   |                                      |                                      |
   | unsignedshort                        |                                      |
   +--------------------------------------+--------------------------------------+
   | boolean                              | java.lang.Boolean                    |
   +--------------------------------------+--------------------------------------+
   | string                               | java.lang.String                     |
   |                                      |                                      |
   | normalizedString                     |                                      |
   |                                      |                                      |
   | token                                |                                      |
   |                                      |                                      |
   | base64Binary                         |                                      |
   |                                      |                                      |
   | hexBinary                            |                                      |
   |                                      |                                      |
   | duration                             |                                      |
   |                                      |                                      |
   | dateTime                             |                                      |
   |                                      |                                      |
   | date                                 |                                      |
   |                                      |                                      |
   | time                                 |                                      |
   |                                      |                                      |
   | gYearMonth                           |                                      |
   |                                      |                                      |
   | gMonthDay                            |                                      |
   +--------------------------------------+--------------------------------------+



Type Conversion Table for Delimited File Wrappers
=================================================================================

DF wrappers always consider the extracted data as ``java.lang.String``.


Type Conversion Table for CUSTOM Wrappers
=================================================================================

A CUSTOM wrapper indicates the types of its fields with Java classes
and, therefore, requires no conversion.


Type Conversion Table for Aracne Wrappers
=================================================================================

.. important:: Aracne wrappers are deprecated and will be removed in a later version of the Denodo Platform. 
   No new Aracne wrappers can be created in version 7.0, but you could edit and use the existing ones. 
   You can see the documentation from `previous version <https://community.denodo.com/docs/html/browse/6.0/vdp/vql/generating_wrappers_and_data_sources/valid_conversions_between_types_in_wrappers_and_vdp_types/native-type_conversions_of_a_wrapper_to_java_types>`_.


Type Conversion Table for Google Search Wrappers
=================================================================================

All the fields in Google Search indexes are translated to attributes of
type ``text`` in Virtual DataPort, except for the field ``RATING`` which
is of ``int`` type.

Wrappers created from Google Search indexes include some additional
attributes besides the ones contained in the original index. These
fields may be of other types. See section :ref:`Google Search Data Sources`.

