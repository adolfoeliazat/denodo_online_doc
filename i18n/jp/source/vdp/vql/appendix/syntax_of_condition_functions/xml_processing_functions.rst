========================
XML Processing Functions
========================

XML processing functions create and transform values of type xml.


.. contents:: XML functions supported by Virtual DataPort
   :depth: 1
   :local:
   :backlinks: none
   :class: threecols

XMLQUERY
=================================================================================

.. rubric:: Description

The ``XMLQUERY`` function extracts information from an XML document
using the XQuery language (`XML Query (XQuery) <https://www.w3.org/XML/Query>`_).

.. rubric:: Syntax

.. code-block:: bnf

   XMLQUERY( <XQuery expression:text>, <is XQuery file:boolean> ):xml

   XMLQUERY( <XQuery expression:text>, <is XQuery file:boolean>, <xml value:xml> ):xml
   
   XMLQUERY( <XQuery expression:text>, <is XQuery file:boolean>, <xml value:text> , <is XML file:boolean> ):xml

-  ``XQuery expression``. XQuery expression used to query xml data.
-  ``is XQuery file``. True, if the parameter “XQuery expression” is a path
   to a file containing an XQuery expression. False, if
   “XQuery expression” is a literal or is the name of a field that
   contains an expression.
-  ``xml value``. The XML to manipulate.
-  ``is XML File``. True, if the parameter “xml value” is a path to a file
   containing an XML document. If “is XML File” is false or is missing,
   “xml value” is a literal or the name of an XML field.

.. rubric:: Examples

Consider the view ``V`` that only has one column of type “xml” and one
row:

+--------------------------------------------------------------------------+
| books_xml                                                                |
+==========================================================================+
| .. code-block:: xml                                                      |
|                                                                          |
|    <BOOKLIST xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">      |
|       <BOOKS>                                                            |
|          <ITEM CAT="MMP">                                                |
|             <TITLE>Pride and Prejudice</TITLE>                           |
|             <AUTHOR>Jane Austen</AUTHOR>                                 |            
|             <PUBLISHER>Modern Library</PUBLISHER>                        |
|          </ITEM>                                                         |
|          <ITEM CAT="P">                                                  |
|             <TITLE>Wuthering Heights</TITLE>                             |
|             <AUTHOR>Emily Brontë</AUTHOR>                                |
|             <PUBLISHER>Penguin Classics</PUBLISHER>                      |
|          </ITEM>                                                         |
|        </BOOKS>                                                          |
|        <CATEGORIES DESC="Miscellaneous categories">                      |
|            <CATEGORY CODE="P" DESC="Paperback"/>                         |
|            <CATEGORY CODE="MMP" DESC="Mass-market Paperback"/>           |
|            <CATEGORY CODE="H" DESC="Hard Cover"/>                        |
|       </CATEGORIES>                                                      |
|    </BOOKLIST>                                                           |
+--------------------------------------------------------------------------+


Consider the file ``C:/books_info.xml`` with the same content as
the view ``V``.

Consider the file ``C:/books.xq`` with the following XQuery
expression:



.. code-block:: xquery

   <ul>
      {
      for $b in //BOOKS/ITEM
      order by $b/TITLE return
      <li>
         <i> { string($b/TITLE) } </i> by { string($b/AUTHOR) }
      </li>
      }
   </ul>
   
And consider the file ``C:/books2.xq`` with an XQuery expression
that transform the XML document of the file ``C:/books_info.xml``:


.. code-block:: xquery

   <ul>
      {
      for $b in doc('c:/books_info.xml')//BOOKS/ITEM
      order by $b/TITLE return
      <li>
        <i> { string($b/TITLE) } </i> by { string($b/AUTHOR) }
      </li>
      }
   </ul>
   
.. rubric:: Example 1

The following queries have the same result:

**Query 1**

.. code-block:: sql

   SELECT XMLQUERY('
   <ul>
      {
      for $b in doc(''C:/books_info.xml'')//BOOKS/ITEM
      order by $b/TITLE return
       <li>
           <i> { string($b/TITLE) } </i> by { string($b/AUTHOR) }
       </li>
      }
   </ul>', false)
   FROM Dual();

**Query 2**


.. code-block:: sql

   SELECT XMLQUERY ('C:/books2.xq', true)
   FROM Dual();

**Query 3**


.. code-block:: sql

   SELECT XMLQUERY('<ul>
   {
   for $b in //BOOKS/ITEM
   order by $b/TITLE return
   <li>
   <i> { string($b/TITLE) } </i> by { string($b/AUTHOR) }
   </li>
   }
   </ul>', false, booksxml) 
   FROM xQuery_sample_view;

**Query 4**

.. code-block:: sql

   SELECT XMLQUERY('C:/books.xq', true, booksxml, false)
   FROM xQuery_sample_view


+--------------------------------------------------------------------------+
| xmlquery                                                                 |
+==========================================================================+
| .. code-block:: html                                                     |
|                                                                          |
|    <ul>                                                                  |
|      <li>                                                                |
|        <i>Pride and Prejudice</i> by Jane Austen                         |
|      </li>                                                               |
|      <li>                                                                |
|        <i>Wuthering Heights</i> by Emily Brontë                          |
|      </li>                                                               |
|    </ul>                                                                 |
+--------------------------------------------------------------------------+

In "Query 1" the XQuery expression is passed as a parameter and in
"Query 2" the parameter is the path to a file containing the same
expression. That is why in the "Query 2", the second parameter is
``true``. This expression reads the content of the file
"C:/books\_info.xml".

In "Query 3" and "Query 4" the XML document is obtained from the field
``booksxml`` of the view ``V``.


XPATH
=================================================================================

.. rubric:: Description

The ``XPATH`` function returns the nodes from an XML document selected
by an XPath expression (`XPath Language <https://www.w3.org/TR/xpath>`_).

This function supports XPath 2.0 at the basic level of conformance
defined by W3C. This means that the function provides all XPath features
that do not involve schema-awareness.

.. rubric:: Syntax

.. code-block:: bnf

   XPATH( <xml value:xml>, <XPath expression:text> [, <xml header:boolean> ]):xml

-  ``xml value``. Required. XML data-type which you want to apply the
   XPath expression on.
-  ``XPath expression``. Required. XPath expression.
-  ``xml header``. Optional. If ``true`` and the selected value is an XML
   node, the result includes the XML declaration
   (``<?xml version="1.0"``...). If ``false`` or the selected value is
   not an XML node, the result will not include this declaration.

If the function is delegated to the Oracle database, the parameter
``xml header`` is ignored and the function only returns the result of
applying the XPath expression.

.. rubric:: Examples

.. rubric:: Example 1



.. code-block:: sql

   SELECT XPATH ( CAST ('xml' ,
   '<?xml version="1.0" encoding="ISO-8859-1"?>
   <a>
   <b>Hello</b>
   <b>World</b>
   </a>' ) , '/a/b', true) as xpath_results
   FROM Dual();


+--------------------------------------------------------------------------+
| xpath\_results                                                           |
+==========================================================================+
| <?xml version="1.0" encoding="UTF-8"?><b>Hello</b><b>World</b>           |
+--------------------------------------------------------------------------+

.. rubric:: Example 2



.. code-block:: sql

   SELECT xpath ( cast ('xml' ,
   '<?xml version="1.0" encoding="ISO-8859-1"?>
   <a>
   <b>Hello</b>
   <b>World</b>
   </a>' ) , '/a/b/text()', true) as xpath_results
   FROM Dual();

+--------------------------------------------------------------------------+
| xpath\_results                                                           |
+==========================================================================+
| HelloWorld                                                               |
+--------------------------------------------------------------------------+

.. rubric:: Example 3



.. code-block:: sql

   SELECT xpath ( cast ('xml',
   '<?xml version="1.0" encoding="ISO-8859-1"?>
   <a>
   <b>Hello</b>
   <b>World</b>
   </a>' ) , '/a/b/text()', false) as xpath_results
   FROM Dual();


+--------------------------------------------------------------------------+
| xpath\_results                                                           |
+==========================================================================+
| HelloWorld                                                               |
+--------------------------------------------------------------------------+

The result of “Example 2” and “Example 3” is the same even though in
“Example 2” the value of the parameter ``xml header`` is ``true`` and in
“Example 3” is ``false``. The reason is that the result of the XPath
function is not an XML document.


XSLT
=================================================================================

.. rubric:: Description

The ``XSLT`` function returns the result of applying an XSL
transformation to an XML.

.. rubric:: Syntax

.. code-block:: bnf

   XSLT( <XML value:xml>, <XSL value:xml> ):xml

   XSLT( <XML value:{xml\|text}>, <xslValue:{xml\|text}>, [, <is path to XML:boolean> ] [, <is path to XSLT:boolean> ]:xml

-  ``XML value``. Required. XML literal, XML field or file to transform.
-  ``XSL value``. Required. XSL literal, field containing an XSL or file
   containing an XSL.
-  ``is path to XML``. Required only if the type of ``xml value`` is text.
   ``true`` if ``xml value`` is a path to the xml file. ``false`` otherwise.
-  ``is path to XSLT``. Required only if the type of ``XSL value`` is text.
   ``true`` if is a path to the XSL file. ``false`` otherwise.

.. rubric:: Examples

Consider the view ``V``:

+--------------------------------------+------------------------------------------+
| xml\_sample                          | xsl\_sample                              |
+======================================+==========================================+
| .. code-block:: xml                  | .. code-block:: xml                      |
|                                      |                                          |
|    <?xml version='1.0'               |    <?xml version='1.0'                   |
|    encoding='UTF-8'?>                |    encoding='UTF-8'?>                    | 
|    <shop>                            |    <xsl:transform version='1.0'          |
|    <products>                        |    xmlns:xsl='http://www.w3.org/1999/    |
|      <product>                       |    XSL/Transform'>                       |
|        <id>1</id>                    |                                          |
|        <name>Virtual DataPort</name> |    <xsl:template match='/shop/products'> |
|      </product>                      |      <shop>                              |
|      <product>                       |        <xsl:for-each select='product'>   |
|        <id>2</id>                    |          <product>                       |
|        <name>ITPilot</name>          |            <xsl:value-of select='name'/> |
|      </product>                      |          </product>                      |
|      <product>                       |        </xsl:for-each>                   |
|        <id>3</id>                    |      </shop>                             |
|        <name>Scheduler</name>        |    </xsl:template>                       |
|      </product>                      |    </xsl:transform >                     |
|      <product>                       |                                          |
|        <id>4</id>                    |                                          |
|        <name>Aracne</name>           |                                          |
|      </product>                      |                                          |
|    </products>                       |                                          |
|  </shop>                             |                                          |
|                                      |                                          |
+--------------------------------------+------------------------------------------+

.. rubric:: Example 1

.. code-block:: sql

   SELECT XSLT(xml_sample, xsl_sample, false, false)
   FROM V

+--------------------------------------------------------------------------+
| xslt                                                                     |
+==========================================================================+
| .. code-block:: xml                                                      |
|                                                                          |
|    <?xml version="1.0" encoding="UTF-8"?>                                |
|    <shop>                                                                |
|      <products>                                                          |
|        <product>Virtual DataPort</product>                               |
|        <product>ITPilot</product>                                        |
|        <product>Scheduler</product>                                      |
|        <product>Aracne</product>                                         |
|      </products>                                                         |
|    </shop>                                                               |
+--------------------------------------------------------------------------+



The same result could be obtained with the following queries:

.. code-block:: sql

   SELECT XSLT(xml_sample, CAST('xml', xsl_sample), false)
   FROM V

.. code-block:: sql

   SELECT XSLT(CAST('xml', xml_sample), xsl_sample, false)
   FROM V

.. code-block:: sql

   SELECT XSLT(CAST('xml', xml_sample), CAST('xml', xsl_sample))
   FROM V

.. rubric:: Example 2

Convert the file ``books.xml`` using the XSL file ``books.xsl``. Note
the two last parameters indicating that the first and the second
parameters are paths to files.

.. code-block:: sql

   SELECT XSLT ('../test/xml/books.xml', '../test/xml/books.xsl', true, true)
   FROM V

.. rubric:: Example 3

Convert the cells of the columns ``xml_sample`` using the XSL file
``books.xsl``.

.. code-block:: sql

   SELECT XSLT (CAST ('xml', xml_sample), '../test/xml/books.xsl', true)
   FROM V

.. rubric:: Example 4

Convert the file ``books.xml`` using the XSL of the column
``xsl_sample``.

.. code-block:: sql

   SELECT XSLT ('../test/xml/books.xml', CAST ('xml', xsl_sample), true)
   FROM V


Converting XML Data into Virtual DataPort Compound Types
=================================================================================

By combining the ``CAST`` and ``CREATETYPEFROMXML`` functions you can
create fields with compound types (``register`` or ``array``) in a
view from XML data.

.. note:: The function ``CREATETYPEFROMXML`` is deprecated and may be removed in future versions of the Denodo Platform. Instead, create an XML data source with a route of type *from variable* and pass the XML document to this data source.

For example: suppose we have a view ``V`` with a field of type xml 
called ``PERSONAL_DATA_XML``. The data contained in this field has
the following structure:

.. code-block:: xml

   <person>
       <name> </name>
       <age> </age>
   </person>

Now consider the following expression:

.. code-block:: vql

   CREATE VIEW PERSON AS
       SELECT CAST(
           CREATETYPEFROMXML(
                 'personaldata_type'
               , '<person><name> John Smith </name><age>25</age></person>'
           ), PERSONAL_DATA_XML
       ) PERSONALDATA
   FROM V
   
The type of the derived field ``PERSONALDATA`` of the new view
``PERSON`` is ``personaldata_type``. This type is a ``register`` type 
made up of the fields ``name`` (``text`` type) and ``age`` (``long``
type).

The second parameter of the ``CREATETYPEFROMXML`` function must be an
example of the values contained in the ``PERSONAL_DATA_XML`` field of
the view ``V``.

``CREATETYPEFROMXML`` can also create array types. This will happen when
the XML data passed to the first parameter and second parameters has
repeated elements. E.g.:

.. code-block:: xml

   <titles>
      <title lang="en"> </title>
      <title lang="en"> </title>
   </titles>

In this case, the type created by ``CREATETYPEFROMXML`` is a register of
arrays. Each component of the array is a register with two components:
``title`` and ``lang``.

Converting XML-type data into Virtual DataPort compound-type data allows the
data in XML code to be combined with data from other relations. For
example, suppose you have a view ``RISK_LEVEL`` with two fields
called ``age`` (``long`` type) and ``risk`` (``double`` type), which
includes some type of risk index calculated according to the age of an
individual. It would be possible to run a join operation between the
``PERSON`` view and the ``RISK_LEVEL`` view using the ``age`` field
of ``RISK_LEVEL`` and the ``age`` field of the ``PERSONALDATA``
field in the ``PERSON`` view.


