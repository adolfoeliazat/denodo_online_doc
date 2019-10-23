====================================
Data Source Configuration Properties
====================================

The configuration properties of a JDBC or an ODBC data source, also
called *source configuration*, specify certain characteristics of the
underlying source such as the operations they support.

The Execution Engine uses this information to know which operations are
supported by the source, in order to push down to the data source as
much processing as possible to optimize response times and minimize
traffic through the network.

.. note:: Usually, users do not need to change these settings since
   Virtual DataPort automatically uses suitable configurations for most
   common data sources.

You can define them in the "Source configuration" tab of the data source or by adding them
to the statement ``CREATE DATASOURCE``.

The table below lists the properties you can configure in the
``SOURCECONFIGURATION`` clause of the data sources. On the left column, you have the name of each property;
on the right column, the label of this property in the administration tool, in the tab "Source configuration" of the data source.
To find the meaning of a property, go to the section :doc:`/vdp/administration/creating_data_sources_and_base_views/data_source_configuration_properties/data_source_configuration_properties` of the Administration Guide.

.. table:: Properties of the SOURCECONFIGURATION clause of data sources
   :name: Properties of the SOURCECONFIGURATION clause of data sources

   +----------------------------------------+--------------------------------------+
   | Property Name in SOURCECONFIGURATION   | Property Name in the Administration  |
   | Clause                                 | Tool (*Source configuration* tab)    |
   +========================================+======================================+
   | ALLOWFORUPDATE                         | Allow for UPDATE clause              |
   +----------------------------------------+--------------------------------------+
   | ALLOWLITERALASPARAMETER                | Allow literal as parameter           |
   +----------------------------------------+--------------------------------------+
   | DELEGATEAGGREGATEFUNCTIONS             | Delegate aggregate functions list    |
   +----------------------------------------+--------------------------------------+
   | DELEGATEALLOPERATORS                   | Delegate all operators               |
   +----------------------------------------+--------------------------------------+
   | DELEGATEARRAYLITERAL                   | Delegate array literal               |
   +----------------------------------------+--------------------------------------+
   | DELEGATE\_BINARY\_ORDERBY\_COLLATION   | Delegate binary ORDER BY collation   |
   +----------------------------------------+--------------------------------------+
   | DELEGATECOMPOUNDFIELDPROJECTION        | Delegate compound field projection   |
   +----------------------------------------+--------------------------------------+
   | DELEGATEFETCH                          | Delegate FETCH clause                |
   +----------------------------------------+--------------------------------------+
   | DELEGATEGROUPBY                        | Delegate GROUP BY clause             |
   +----------------------------------------+--------------------------------------+
   | DELEGATEHAVING                         | Delegate HAVING clause               |
   +----------------------------------------+--------------------------------------+
   | DELEGATEINNERJOIN                      | Delegate INNER JOIN                  |
   +----------------------------------------+--------------------------------------+
   | DELEGATEINTERSECTION                   | Delegate INTERSECT                   |
   +----------------------------------------+--------------------------------------+
   | DELEGATEINVALIDNUMBERLITERALSASNULL    | Delegate invalid number literals as  |
   |                                        | NULL                                 |
   +----------------------------------------+--------------------------------------+
   | DELEGATEJOIN                           | Delegate JOIN                        |
   +----------------------------------------+--------------------------------------+
   | DELEGATELEFTFUNCTION                   | Delegate left function               |
   +----------------------------------------+--------------------------------------+
   | DELEGATELEFTLITERAL                    | Delegate left literal                |
   +----------------------------------------+--------------------------------------+
   | DELEGATELITERALEXPRESSION              | Delegate literal expression          |
   +----------------------------------------+--------------------------------------+
   | DELEGATEMINUS                          | Delegate MINUS                       |
   +----------------------------------------+--------------------------------------+
   | DELEGATEMIXEDAGGREGATEEXPRESSION       | Delegate mixed literal expression    |
   +----------------------------------------+--------------------------------------+
   | DELEGATENATURALOUTERJOIN               | Delegate natural OUTER JOIN          |
   +----------------------------------------+--------------------------------------+
   | DELEGATENOTCONDITION                   | Delegate NOT condition               |
   +----------------------------------------+--------------------------------------+
   | DELEGATEOFFSET                         | Delegate OFFSET clause               |
   +----------------------------------------+--------------------------------------+
   | DELEGATE\_OFFSET\_RESTRICTION          | Delegate OFFSET clause restriction   |
   +----------------------------------------+--------------------------------------+
   | DELEGATEOPERATORSLIST                  | Delegate operators list              |
   +----------------------------------------+--------------------------------------+
   | DELEGATEORCONDITION                    | Delegate OR condition                |
   +----------------------------------------+--------------------------------------+
   | DELEGATEORDERBY                        | Delegate ORDER BY                    |
   +----------------------------------------+--------------------------------------+
   | DELEGATE\_ORDERBY\_COLLATION\_MODIFIER | Delegate ORDER BY collation modifier |
   +----------------------------------------+--------------------------------------+
   | DELEGATEPROJECTION                     | Delegate projection                  |
   +----------------------------------------+--------------------------------------+
   | DELEGATEREGISTERLITERAL                | Delegate register literal            |
   +----------------------------------------+--------------------------------------+
   | DELEGATERIGHTFIELD                     | Delegate right field                 |
   +----------------------------------------+--------------------------------------+
   | DELEGATERIGHTFUNCTION                  | Delegate right function              |
   +----------------------------------------+--------------------------------------+
   | DELEGATERIGHTLITERAL                   | Delegate right literal               |
   +----------------------------------------+--------------------------------------+
   | DELEGATESCALARFUNCTIONS                | Delegate scalar functions list       |
   +----------------------------------------+--------------------------------------+
   | DELEGATESELECTDISTINCT                 | Delegate SELECT DISTINCT             |
   +----------------------------------------+--------------------------------------+
   | DELEGATESELECTION                      | Delegate selection                   |
   +----------------------------------------+--------------------------------------+
   | DELEGATESUBQUERY                       | Delegate subquery                    |
   +----------------------------------------+--------------------------------------+
   | DELEGATEUNION                          | Delegate UNION                       |
   +----------------------------------------+--------------------------------------+
   | NESTEDJOINWITHBLOCKSSTRATEGY           | Nested join optimization syntax      |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSAGGREGATEFUNCTIONSOPTIONS      | Supports modifier in aggregate       |
   |                                        | function                             |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSBATCHINSERT                    | Supports batch inserts               |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSBRANCHOUTERJOIN                | Supports branch OUTER JOIN           |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSCLOBINBATCH                    | Supports CLOBs in batch inserts      |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSEQOUTERJOINOPERATOR            | Supports Eq OUTER JOIN               |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSEXPLICITCROSSJOIN              | Supports explicit CROSS JOIN         |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSFULLEQOUTERJOIN                | Supports full Eq OUTER JOIN          |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSFULLNOTEQOUTERJOIN             | Supports full NotEq OUTER JOIN       |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSFUSINGINUSINGANDNATURALJOIN    | Supports fusing in USING and natural |
   |                                        | JOIN                                 |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSGROUPBYLITERALASPARAMETER      | Supports GROUP BY literals as        |
   |                                        | parameters                           |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSJOINONCONDITION                | Supports JOIN ON Condition           |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSNATURALJOIN                    | Supports NATURAL JOIN                |
   +----------------------------------------+--------------------------------------+
   | SUPPORTS\_ORDERBY\_BINARY\_COLLATION   | Supports binary ORDER BY collation   |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSORDERBYEXPRESSION              | Supports ORDER BY expressions        |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSRIGHTDEEPJOIN                  | Supports right deep n-joins          |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSPREPAREDSTATEMENT              | Supports prepared statements         |
   +----------------------------------------+--------------------------------------+
   | SUPPORTSUSINGJOIN                      | Supports USING JOIN                  |
   +----------------------------------------+--------------------------------------+

CONTAINS Operator Configuration Properties
=================================================================================

The ``CONTAINS`` operator allows executing complex Boolean keyword
searches on text-type attributes from an external index of unstructured
data (e.g. Google Search data sources).

The syntax of the search language on unstructured data is described in the
section :ref:`Syntax of Search Expressions for the Contains Operator`.

Custom-type wrappers can also specify the search language capacities
that are supported through Operator Configuration Properties. This way,
other external indexes besides Google Search ones can be
imported in Virtual DataPort. This section describes these properties.

-  *Supports And*. This takes the value ``true`` if searches with the
   logic operator AND are supported, and the value false, if they are
   not.
-  *Supports OR*. This takes the value ``true`` if searches with the
   logic operator OR are supported, and the value false, if they are
   not.
-  *Supports Not*. This takes the value ``true`` if searches with the
   logic operator NOT are supported and the value false, if they are
   not.
-  *Supports Exact Search*. This takes the value ``true`` if searches by
   exact phrase are supported and the value false, if they are not.
-  *Supports One Wildcards First Position*. This takes the value
   ``true``, if the wildcard matches with just one character (i.e. the
   wildcard ``?``) in the first position of a term are supported.
-  *Supports One Wildcards Rest Position*. This takes the value true if
   the wildcard matches with just one character (i.e. the wildcard "?")
   in the remaining positions of a term other than the first are
   supported.
-  *Supports Multi Wildcards First Position*. This takes the value
   ``true`` if wildcards that match with multiple characters (i.e. the
   wildcard ``*``) are supported in the first position of a term.
-  *Supports Multi Wildcards Rest Position*. This takes the value
   ``true``, if wildcards that match with multiple characters (i.e. the
   wildcard ``*``) are supported in the remaining positions of a term
   other than the first.
-  *Supports Fuzzy Terms without Minimum Relevance*. This takes the
   value ``true`` if fuzzy searches without specifying a minimum
   similarity threshold are supported.
-  *Supports Fuzzy Terms with Minimum Relevance*. This takes the value
   ``true``, if fuzzy searches specifying a minimum similarity threshold
   are supported.
-  *Supports Proximity Terms Without Maximum Distance*. This takes the
   value ``true``, if searches by proximity without specifying a maximum
   distance among the terms are supported.
-  *Supports Proximity Terms With Maximum Distance*. This takes the
   value ``true``, if searches by proximity specifying a maximum
   distance among the terms are supported.
-  *Supports Boosting Terms without Boosting Factor*. This takes the
   value ``true``, if the relevance boosting specification is supported
   for a term without specifying a specific boosting factor.
-  *Supports Boosting Terms with Boosting Factor*. This takes the value
   ``true``, if the relevance boosting specification is supported for a
   term specifying a specific boosting factor.
-  *Supports Inclusive Range Search*. This takes the value ``true``, if
   range searches are supported (inclusive).
-  *Supports Exclusive Range Search*. This takes the value ``true``, if
   range searches are supported (exclusive).
-  *Supports Field Grouping*. This takes the value ``true``, if the
   combination of logic operators AND and OR is supported using
   brackets. For example:
   ``title contains '(term1 AND term2) OR (term3)'``
-  *Supports Grouping*. This takes the value true, if the combination of
   logic operators AND and OR in different query conditions is
   supported. For example:
   ``title contains 'term1' AND (content contains 'term2' OR summary contains 'term3')``
