====================
Comparison Operators
====================


.. toctree::
   :hidden:

   comparing_literals.rst
   three-valued_logic.rst

Comparison operators are used in conditions to compare one expression with another. The result of the comparison can be *true* or *false* or *unknown*.

.. important::
  
   Any comparison with a ``null`` value returns *unknown*. If a query has a WHERE and/or HAVING clause, only the rows for which the condition returns *true* are added to the result.
   
   This behavior is defined by the SQL standard. The section :doc:`/vdp/vql/language_for_defining_and_processing_data_vql/comparison_operators/three-valued_logic` explains this in more detail.


The comparison operators are the following:

-  ``a < b``: *true* if ``a`` is less than ``b``.

-  ``a <= b``: *true* if ``a`` is less than or equal to ``b``.

-  ``a > b``: *true* if ``a`` is greater than ``b``.

-  ``a >= b``: *true* if ``a`` is greater than or equal to ``b``.

-  ``a = b``: *true* if ``a`` and ``b`` are equal.

-  ``a <> b``: *true* if ``a`` is different than ``b``.

-  ``a LIKE b``: *true* if ``a`` matches the pattern ``b``. ``b`` is an expression of type text and can include these wildcard characters:

   -  ``%`` (percentage): represents a segment of text of any length,
      including an empty text.
   -  ``_`` (underscore): represents any character (only one character).

   Optionally, you can indicate the ``ESCAPE`` clause followed by any
   character. This character is called “escape character”. If the escape
   character is placed in front of a wildcard character (``_`` or ``%``),
   it indicates that the wildcard has to be interpreted as a regular
   character and not as a wildcard.
   
   The default value for the escape character is the ``$`` (dollar).
   
   If the pattern includes the characters ``%`` or ``_`` and
   you want to consider them as literals and not wildcards, escape them by
   prefixing them with the escape character. That is, the character
   indicated in the ``ESCAPE`` parameter or if you do not indicate that,
   with dollar. For example, "$$".
   
   Example 1) the pattern ``'%commerce_'`` matches any string ending with
   the substring ``'commerce'`` followed by any character. For example,
   these values match this pattern: "commerce1", "New commerce2".
   
   Example 2) the following query returns the rows of the view
   ``internet_inc`` whose ``summary`` contain the text ``adsl``:
   ``SELECT * FROM internet_inc WHERE summary like '%adsl%'``
   
   Example 3) to obtain all the rows whose value for the column
   ``discount`` is “30%” use this condition:
   ``SELECT ... FROM ... WHERE discount LIKE '30~%' ESCAPE '~'``
   
   The escape character ~ indicates that the percentage character after
   “30” does not have to be treated as a wildcard.

-  ``a regexp_like b``: *true* if ``a`` matches the pattern ``b``. ``b`` is a 
   `Java regular expression <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.
   
   If you want to do a case-insensitive comparison, use the
   operator ``regexp_ilike`` because the performance will be better than
   if you use a regular expression that ignores the case (i.e. a regular expression that starts with ``?i``).

   Examples: Consider the following view ``PRODUCTS``:

   +--------------------------------------+--------------------------------------+
   | IDENTIFIER                           | NAME                                 |
   +======================================+======================================+
   | AJ00                                 | Product A                            |
   +--------------------------------------+--------------------------------------+
   | AJ17                                 | Product B                            |
   +--------------------------------------+--------------------------------------+
   | AJ1A8                                | Product C                            |
   +--------------------------------------+--------------------------------------+
   | PQ983                                | Product D                            |
   +--------------------------------------+--------------------------------------+
   | PQ00                                 | Product E                            |
   +--------------------------------------+--------------------------------------+

   The query
   ``SELECT * FROM products WHERE identifier regexp_like 'AJ\d+'`` returns
   the rows:

   +--------------------------------------+--------------------------------------+
   | IDENTIFIER                           | NAME                                 |
   +======================================+======================================+
   | AJ00                                 | Product A                            |
   +--------------------------------------+--------------------------------------+
   | AJ17                                 | Product B                            |
   +--------------------------------------+--------------------------------------+

-  ``a regexp_ilike b``: *true* if ``a`` matches the pattern ``b``, ignoring case differences. ``b`` is a 
   `Java regular expression <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_ .

-  ``a contains b``: ``a`` is an expression of type ``text`` of a view created from a
   searchable external index on an Aracne or Google Search data source. See sections
   :ref:`Denodo Aracne Data Sources` and :ref:`Google Search Data Sources`.
   
   ``b`` is a search expression written in the search
   language on non-structured data supported by Virtual DataPort (see
   section :ref:`Syntax of Search Expressions for the Contains Operator`).

   .. note:: This operator is deprecated and may be removed in the next major version of the Denodo Platform.

   The syntax of the search language on non-structured data is described in
   the section :ref:`Syntax of Search Expressions for the Contains Operator`.
   However, bear in mind that the search options available depend on the
   capacities provided by the data source. For example, Google Search does
   not support different characteristics of the search language such as
   proximity searches. Therefore, when the ``contains`` operator is used
   with attributes from Google Search sources, these capacities will not be
   available. The section :ref:`Support for the Contains Operator of Each Source
   Type` lists the search capacities supported by Google Search sources
   and Aracne sources. Custom wrappers that access other data sources, can
   specify the search language capacities for ``contains`` that are
   supported through Configuration Properties (see section :ref:`CONTAINS
   Operator Configuration Properties`).
   
   In the case of derived views, Virtual DataPort calculates the search
   capacities supported for an attribute depending on the capacities of
   their base view attributes. It is possible to view the capacities of
   each attribute by using the ``DESC VIEW`` statement to query the value
   of its Configuration Properties (see the sections :ref:`Denodo Aracne Data
   Sources` and :ref:`CONTAINS
   Operator Configuration Properties`).

   Examples: The following query returns the tuples from the
   ``aracneview`` view, where the ``searchablecontent`` attribute
   contains the words “acme” and “incorporated”:
   
   .. code-block:: sql
   
      SELECT * FROM aracneview 
      WHERE searchablecontent CONTAINS 'acme AND incorporated'
   
   The following query returns the tuples from the ``aracneview`` view
   where the ``searchablecontent`` attribute contains the exact words
   ``acme incorporated`` and some other word starting with
   ``product``: 
   
   .. code-block:: sql
      
      SELECT * from aracneview 
      WHERE searchablecontent contains '"acme incorporated" AND product*'

-  ``a containsor (b [, c]* )``: *true* if at least one of the expressions on the right-side (``b``, ``c``...) is a substring of ``a``.

   The right side of the operator can be one expression or a comma-separated list of expressions.
   
   This operator is case-insensitive.
   
   Example:
   
   .. code-block:: vql
   
      'California' containsor ('AZ', 'CA')
      
   This condition evaluates to *true* because one of the values on the right-side ("CA") is a substring of the expression on the left ("California").

-  ``a containsand (b [, c]* )``: *true* if all the expressions on the right-side (``b``, ``c``...) are a substring of ``a``.

   The right side of the operator can be one expression or a comma-separated list of expressions.
   
   This operator is case-insensitive.
   
   Example #1:
   
   .. code-block:: vql
   
      'California' containsand ('AZ', 'CA')
      
   This condition evaluates to *false* because one of the values on the right-side ("AZ") is not a substring of the expression on the left ("California").
   
   Example #2:
   
   .. code-block:: vql
   
      'California' containsand ('CA', 'Cali')
      
   This condition evaluates to *true* because all the values on the right-side are a substring of the expression on the left ("California").
      

-  ``a iscontained (b [, c]* )``: *true* if ``a`` is a substring of all the expressions on the right-side (``b``, ``c``...).

   The right side of the operator can be one expression or a comma-separated list of expressions.
   
   This operator is case-insensitive.
   
   Example #1:
   
   .. code-block:: vql
   
      'CA' iscontained ('AZ', 'California', 'CA')
      
   This condition evaluates to *false* because the expression on the left ("CA") is not a substring of one of the values on the right-side ("AZ").
   
   Example #2:
   
   .. code-block:: vql
   
      'CA' iscontained ('Cali', 'California');
      
   This condition evaluates to *true* because the expression on the left ("CA") is a substring of all the values on the right-side.

-  ``a is not NULL``: *true* if ``a`` is not null.

-  ``a is NULL``: *true* if ``a`` is null.

-  ``a is TRUE``: *true* if ``a`` is a boolean expression and is *true*.

-  ``a is FALSE``: *true* if ``a`` is a boolean expression and is *false*.

-  ``a in (b [, c]* )``: *true* if ``a`` is equal to one or more expressions on the right-side (``b``, ``c``...).

   Example: The following statement selects the tuples from the view ``internet_inc`` for which their value for
   the ``taxid`` attribute is ``B78596011`` or
   ``B78596012``:
   
   .. code-block:: sql
      
      SELECT * 
      FROM internet_inc 
      WHERE taxid in ('B78596011', 'B78596012')
      
-  ``a between b and c``: *true* if ``a`` is greater than or equal to ``b`` and less than or equal to ``c``. 
   
   Example: The following two statements produce the same result: They
   select tuples from the view ``internet_inc`` for which their value for
   the ``iinc_id`` attribute is within the range of 2 and 4 (inclusive):

   .. code-block:: sql 

      SELECT * 
      FROM internet_inc 
      WHERE iinc_id between 2 AND 4 
   
-  ``~``: The evaluation of this operator returns a value between 0 and 1
   that estimates the similarity between the two text-type operands using a
   variety of similarity algorithms. In addition to the operands to
   compare, the similarity operator receives the similarity algorithm to
   use and a *minimum* *similarity threshold* as parameters.

   Where the similarity between character strings reaches or exceeds the
   threshold, the condition is assessed as true. Where this is not the
   case, it is assessed as false.
   
   The left-hand (text-type) operand is one of the character strings to
   compare. The right-hand operand is a list of text-type elements. The
   first element in this list is the second character string to compare.
   The second specifies the minimum similarity threshold (a value of
   between 0 and 1) and the third (optional) specifies the similarity
   algorithm to be used.
   
   The algorithms available are the same as for the similarity function
   (see appendix :ref:`SIMILARITY`).
   
   Example: The following query returns tuples for which their
   ``customername`` field has a similarity of over 0.7 with the "General
   Motors Inc" string, using the Jaro Winkler editing distance algorithm
   between strings:

   .. code-block:: sql
   
      SELECT * 
      FROM internet_inc_cname 
      WHERE customer_name ~ ('General Motors Inc','0.7','JaroWinkler')

-  ``XMLExists``: This operator executes an XQuery expression 
   (`XML Query <https://www.w3.org/XML/Query/>`_)
   over an ``xml`` value. It returns *true* if it
   finds a match.
   This operator has three signatures:

   -  .. code-block:: bnf
   
         XMLExists(XQueryExpression : text, value : xml)   
         
      Returns ``true`` if there is a match of ``XQueryExpression`` in ``value``.
                  
   -  .. code-block:: bnf
   
         XMLExists(XQueryExpression : text, ReadXQueryExpressionFromFile : boolean, value : xml)
         
      If ``ReadXQueryExpressionFromFile`` is ``true``, ``XQueryExpression``
      is a path to a file that contains the XQuery expression.

   -  .. code-block:: bnf
   
         XMLExists(XQueryExpression : text, ReadXQueryExpressionFromFile : boolean, value : xml, ReadXMLValueFromFile) 
   
      If ``ReadXQueryExpressionFromFile`` is ``true``, ``XQueryExpression``
      has to be a path to a file that contains the XQuery expression.
      
      If ``ReadXMLValueFromFile`` is ``true``, ``ReadXMLValueFromFile``
      has to be path to a file that contains the input XML.

