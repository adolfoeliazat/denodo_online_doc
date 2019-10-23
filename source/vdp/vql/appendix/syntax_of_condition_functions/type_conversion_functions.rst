=========================
Type Conversion Functions
=========================

Type conversion functions transform the type of a value.


.. contents:: Type functions supported by Virtual DataPort
   :depth: 1
   :local:
   :backlinks: none
   :class: threecols

ARRAY\_TO\_STRING
=================================================================================

.. rubric:: Description

The ``ARRAY_TO_STRING`` function converts an array field to a string
that contains the elements of the array separated by a character.

This function has two signatures. With the first one, the array is
surrounded by braces (“{“ and “}”) and if the array contains other
arrays they will also be surrounded by braces. If the array contains
registers, they will be surrounded by parentheses (“(” and “)”).

With the second signature, the user can indicate the characters that
surround the array and its inner registers and arrays.

.. rubric:: Syntax

.. code-block:: bnf

   ARRAY_TO_STRING( <separator:text>, <array value:array> ):text

   ARRAY_TO_STRING( <separator:text>, <array begin delimiter:text>, 
       <array end delimiter:text>, <register begin delimiter:text>,
       <register end delimiter:text>, <array value:array> ):text

-  ``separator``. Character that separates the elements of the array.
-  ``array begin delimiter``. Character placed before the array and its
   inner arrays.
-  ``array end delimiter``. Character placed after the array and its
   inner arrays.
-  ``register begin delimiter``. Character placed before the inner
   register fields.
-  ``register end delimiter``. Character placed after the inner register
   fields.

.. rubric:: Examples

Consider the view ``V`` with two fields: ``name`` and ``info``.
``info`` is an array of registers whose subfields are ``message`` and
``register_sample``. ``register_sample`` is a field of type register
with two subfields: ``key`` and ``value``.

+------------------+-----------------------------------------------+
| name             | info                                          |
|                  +-----------------------------+-----------------+
|                  | message                     | register_sample |
|                  |                             +--------+--------+
|                  |                             | key    |  value |
+==================+=============================+========+========+
| Virtual DataPort | Virtual Data Access Layer   | 1      | one    |
|                  +-----------------------------+--------+--------+
|                  | Data Federation             | 2      | two    |
+------------------+-----------------------------+--------+--------+
| ITPilot          | Web Integration             | 3      |three   |
|                  +-----------------------------+--------+--------+
|                  | Web Automation              | 4      | four   |
+------------------+-----------------------------+--------+--------+
| Aracne           | Crawling                    | 5      | five   |
|                  +-----------------------------+--------+--------+
|                  | Quering non-structured data | 6      | six    |
+------------------+-----------------------------+--------+--------+
| Scheduler        | Job Scheduling              | 7      | seven  |
+------------------+-----------------------------+--------+--------+

.. rubric:: Example 1



.. code-block:: sql

   SELECT name, ARRAY_TO_STRING(' - ', info)
   FROM V


+--------------------------------------+--------------------------------------+
| name                                 | array\_to\_string                    |
+======================================+======================================+
| Virtual DataPort                     | {Virtual Data Access Layer,(1,one) - |
|                                      | Data Federation,(2,two)}             |
+--------------------------------------+--------------------------------------+
| ITPilot                              | {Web Integration,(3,three) - Web     |
|                                      | Automation,(4,four)}                 |
+--------------------------------------+--------------------------------------+
| Aracne                               | {Crawling,(5,five) - Quering         |
|                                      | non-structured data,(6,six)}         |
+--------------------------------------+--------------------------------------+
| Scheduler                            | {Job Scheduling,(7,seven)}           |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 2

.. code-block:: sql

   SELECT name, ARRAY_TO_STRING(', ', ' [ ', ' ] ', ' |- ', ' -|', info)
   FROM V

+------------------+----------------------------------------------------------+
| name             | array\_to\_string                                        |
+==================+==========================================================+
| Virtual DataPort | [ Virtual Data Access Layer, \|- 1, one -\|, Data        |
|                  | Federation, \|- 2, two -\| ]                             |
+------------------+----------------------------------------------------------+
| ITPilot          | [ Web Integration, \|- 3, three -\|, Web Automation,     |
|                  | \|- 4, four -\| ]                                        |
+------------------+----------------------------------------------------------+
| Aracne           | [ Crawling, \|- 5, five -\|, Quering non-structured data,|
|                  | \|- 6, six -\| ]                                         |
+------------------+----------------------------------------------------------+
| Scheduler        | [ Job Scheduling, \|- 7, seven -\| ]                     |
+------------------+----------------------------------------------------------+

.. _vql_guide_type_conversion_functions_cast:

CAST
=================================================================================

.. rubric:: Description

The ``CAST`` function converts data from one data type to another.

.. rubric:: Syntax 1

.. code-block:: bnf

   CAST( <vdp data type:text>, <value:expression> )

-  ``vdp data type``. Required. Data type you want the value to be
   converted to. This value is the name of a Virtual DataPort type.
-  ``value``. Required. The value to convert.

The following table shows the possible type conversions. The column
Output type contains the possible values of the parameter
``vdp data type``.

.. table:: Type conversions permitted with the CAST function
   :name: Type conversions permitted with the CAST function
   
   +--------------------------------------+--------------------------------------+
   | Input Value Type (type of the        | Output Type                          |
   | parameter value)                     |                                      |
   +======================================+======================================+
   | array                                | array                                |
   +--------------------------------------+--------------------------------------+
   | boolean                              | int                                  |
   +--------------------------------------+--------------------------------------+
   | text, blob                           | blob                                 |
   +--------------------------------------+--------------------------------------+
   | text, int, long, float, double,      | boolean                              |
   | boolean                              |                                      |
   +--------------------------------------+--------------------------------------+
   | text, date (deprecated), localdate,  | date (deprecated)                    |
   | timestamp, timestamptz, time, long   |                                      |
   +--------------------------------------+--------------------------------------+
   | text, date (deprecated), localdate,  | localdate                            |
   | timestamp, timestamptz, long         |                                      |
   +--------------------------------------+--------------------------------------+
   | text, date (deprecated), localdate,  | timestamp                            |
   | timestamp, timestamptz, time, long   |                                      |
   +--------------------------------------+--------------------------------------+
   | text, date (deprecated), localdate,  | timestamptz                          |
   | timestamp, timestamptz, time, long   |                                      |
   +--------------------------------------+--------------------------------------+
   | text, date (deprecated),             | time                                 |
   | timestamp, timestamptz, time, long   |                                      |
   +--------------------------------------+--------------------------------------+
   | text, int, long, float, double       | double                               |
   +--------------------------------------+--------------------------------------+
   | text, int, long, float, double       | float                                |
   +--------------------------------------+--------------------------------------+
   | text, int, long, float, double       | decimal                              |
   +--------------------------------------+--------------------------------------+
   | text, int, long, float, double       | int                                  |
   +--------------------------------------+--------------------------------------+
   | text, int, long, float, double       | long                                 |
   +--------------------------------------+--------------------------------------+
   | xml, register                        | register                             |
   +--------------------------------------+--------------------------------------+
   | text, int, long, float, double,      | text                                 |
   | boolean, date (deprecated),          |                                      |
   | localdate, timestamp, timestamptz,   |                                      |
   | time , xml, blob, register, array    |                                      |
   +--------------------------------------+--------------------------------------+
   | text, blob, xml, register, array     | xml                                  |
   +--------------------------------------+--------------------------------------+


.. rubric:: Syntax 2

.. code-block:: bnf

   CAST( <value:expression> AS <SQL type:text> )

-  ``value``. Required. The value to convert.
-  ``SQL type``. Required. Name of an ANSI SQL type you want the value
   to be converted to.

.. csv-table:: Type conversion from ANSI SQL types and Virtual DataPort types
   :header: "SQL Type", "Virtual DataPort Type"
   :name: Type conversion from ANSI SQL types and Virtual DataPort types
   
   "BIT (n)", "blob"
   "BIT VARYING (n)", "blob"
   "BOOL", "boolean"
   "BYTEA", "blob"
   "CHAR (n)", "text"
   "CHARACTER (n)", "text"
   "CHARACTER VARYING (n)", "text"
   "DATE", "localdate"
   "DECIMAL", "double"
   "DECIMAL (n)", "double"
   "DECIMAL (n, m)", "double"
   "DOUBLE PRECISION", "double"
   "FLOAT", "float"
   "FLOAT4", "float"
   "FLOAT8", "double"
   "INT2", "int"
   "INT4", "int"
   "INT8", "long"
   "INTEGER", "int"
   "NCHAR (n)", "text"
   "NUMERIC", "double"
   "NUMERIC (n)", "double"
   "NUMERIC (n, m)", "double"
   "NVARCHAR (n)", "text"
   "REAL", "float"
   "SMALLINT", "int"
   "TEXT", "text"
   "TIMESTAMP", "timestamp"
   "TIMESTAMP WITH TIME ZONE", "timestamptz"
   "TIMESTAMPTZ", "timestamptz"
   "TIME", "time"
   "TIMETZ", "time"
   "VARBIT", "blob"
   "VARCHAR", "text"
   "VARCHAR ( MAX )", "text"
   "VARCHAR (n)", "text"

.. rubric:: Remarks

.. rubric:: Remark 1

The function ``CAST`` truncates the output when converting a value to a
text, when these two conditions are met:

#. You specify a SQL type with length for the target data type. E.g.
   ``VARCHAR(20)``.
#. And, this length is lower than the length of the input value.

For example, ``CAST ('Denodo' AS VARCHAR(2))`` returns “De” because the
target type specifies a length lower than the length of the input value.

.. rubric:: Remark 2

When casting a ``boolean`` to an ``integer``, ``true`` is mapped to
``1`` and ``false`` to ``0``.

.. rubric:: Examples

.. rubric:: Example 1



.. code-block:: sql

   SELECT CAST('blob', 'hello') AS text_to_blob_cast
       , CAST('boolean', 'true') AS text_to_boolean_cast
       , CAST('boolean', 500000) AS long_to_boolean_cast
       , CAST('boolean', 0) AS long_to_boolean_cast_Zero
       , CAST('double', 5 + 5) AS int_to_double_cast
   FROM Dual();

+----------------+-----------------+-----------------+-----------------+----------------+
| text\_to\_     | text\_to\_      | long\_to\_      | long\_to\_      | int\_to\_      |
| blob\_cast     | boolean\_cast   | boolean\_cast   | boolean\_cast   | double\_cast   |
+================+=================+=================+=================+================+
| [BINARY DATA]  | true            | true            | false           | 10.0           |
| - 5 bytes      |                 |                 |                 |                |
+----------------+-----------------+-----------------+-----------------+----------------+

.. rubric:: Example 2

Consider the view ``V`` with a column ``register_sample`` of type
``register``. This register has a field ``STR`` of type ``array``.

+----------------------------------+
| register\_sample                 |
+===============+========+=========+
| **str**       | **r1** | **r2**  |
+---------------+--------+---------+
| hello | world | 3      | 3.70    |
+---------------+--------+---------+

.. code-block:: sql

   SELECT CAST('xml', register_sample)
   FROM V

.. code-block:: xml
   
   <?xml version="1.0" encoding="UTF-8"?>
   <register>
       <R1>9</R1>
       <R2>1.1</R2>
       <STR>another string</STR>
       <STR>last string here</STR>
   </register>

.. rubric:: Example 3   

Consider the view ``V`` with a column ``array_sample`` of type ``array``.
The array ``array_sample`` has another array into it.

.. table::

   +------------------------------------------------------------+
   | array\_sample                                              |
   +========================================+========+==========+
   | **str**                                | **r1** | **r2**   |
   +------------+---------------------------+--------+----------+
   | denodo     | platform                  | 40     | 52.0     |
   +------------+---------------------------+--------+----------+
   | enterprise | data     | virtualization | 60     | 72.0     |
   +------------+---------------------------+--------+----------+

.. code-block:: sql

   SELECT CAST('xml', array_sample)
   FROM V

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <array>
       <item>
           <R1>40</R1>
           <R2>52.0</R2>
           <STR>denodo</STR>
           <STR>platform</STR>
       </item>
       <item>
           <R1>60</R1>
           <R2>72.0</R2>
           <STR>enterprise</STR>
           <STR>data</STR>
           <STR>virtualization</STR>
       </item>
   </array>
   
.. rubric:: Example 4

.. code-block:: sql

   SELECT
   CAST('hello' AS BIT VARYING(20)) AS text_to_blob_cast
   , CAST(5+5 AS VARCHAR(1)) AS int_to_text_cast
   , CAST('10' AS numeric) AS text_to_int_cast
   FROM Dual();


+-------------------------+-------------------------+-------------------------+
| text\_to\_blob\_        | int\_to\_text\_         | text\_to\_int\_         |
| cast                    | cast                    | cast                    |
+=========================+=========================+=========================+
| [BINARY DATA] - 5 bytes | 1                       | 10.0                    |
+-------------------------+-------------------------+-------------------------+

Note that the value of the second column is truncated from “10” (5+5) to
“1”. The reason is that the SQL type indicated in the ``CAST`` function
(``VARCHAR(1)``) has a maximum length of 1. If it was
``CAST(5+5 AS VARCHAR(2))``, the value of the second column would be
“10”.


CREATETYPEFROMXML
=================================================================================

.. rubric:: Description

The ``CREATETYPEFROMXML`` function creates a register or an array type
from XML data. If the type is created correctly, it returns the name of the new type.

This function is usually used along with ``CAST``. The section :ref:`Converting XML Data into Virtual
DataPort Compound Types` explains how to do it.

.. note:: This function is deprecated and may be removed in future versions of the Denodo Platform. Instead, create an XML data source with a route of type *from variable* and pass the XML document to this data source.

.. rubric:: Syntax

.. code-block:: bnf

   CREATETYPEFROMXML( <new type name:text>, <xml value:{xml|text}> ):text

-  ``new type name``. Required. Name of the new type.
-  ``xml value``. Required. Sample XML used as a template to create the
   new type. The type of the value can be xml or text.

.. rubric:: Examples

.. rubric:: Example 1

Creating a new register type:

.. code-block:: xml

   SELECT CREATETYPEFROMXML('bookstore_xml_type',
   '<bookstore>
       <book category="COOKING">
           <title lang="en">Everyday Italian</title>
           <author>Giada De Laurentiis</author>
           <year>2005</year>
           <price>30.00</price>
       </book>
       <book category="CHILDREN">
           <title lang="en">Harry Potter</title>
           <author>J K. Rowling</author>
           <year>2005</year>
           <price>29.99</price>
       </book>
   </bookstore>') FROM Dual();


.. rubric:: Example 2

Creating a new array type:

.. code-block:: sql

   SELECT CREATETYPEFROMXML('title_type',
   '<titles>
       <title lang="en">XQuery Kick Start</title>
       <title lang="en">Learning XML</title>
   </titles>') FROM Dual();


REGISTER
=================================================================================

.. rubric:: Description

The ``REGISTER`` function creates a register with the values of the
fields of a view.

.. rubric:: Syntax

.. code-block:: bnf

   REGISTER( <field name:any type> [, <field name:any type> ]*):register

-  ``field name``. The name of a field.

.. rubric:: Example

Consider the view ``V``:

+-------------------------+-------------------------+-------------------------+
| int\_sample             | text\_sample            | register\_sample        |
+=========================+=========================+=========================+
| 1                       | A                       | Register { hello ,      |
|                         |                         | how're you }            |
+-------------------------+-------------------------+-------------------------+
| 1                       | B                       | Register { hello, good  |
|                         |                         | bye }                   |
+-------------------------+-------------------------+-------------------------+
| 2                       | C                       | Register { another      |
|                         |                         | string, last string }   |
+-------------------------+-------------------------+-------------------------+


.. code-block:: sql

   SELECT REGISTER(int_sample, text_sample, register_sample) AS register_sample
   FROM V;


+--------------------------------------------------------------------------+
| register\_sample                                                         |
+==========================================================================+
| Register { 1, A, Register { hello , how're you } }                       |
+--------------------------------------------------------------------------+
| Register { 1, B, Register { hello, good bye } }                          |
+--------------------------------------------------------------------------+
| Register { 2, C, Register { another string, last string } }              |
+--------------------------------------------------------------------------+
