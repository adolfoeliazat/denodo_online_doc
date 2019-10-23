============
WWW Wrappers
============

The statement ``CREATE WRAPPER ITP`` creates WWW wrappers. These 
wrappers import semi-structured data sources (typically
semi-structured web sources). These sources may be accessible in the
web, through the local file system or through an FTP service. You need
Denodo ITPilot to execute this type of wrappers. ITPilot provides the
Wrapper Generation Tool to create these wrappers graphically.

.. note:: The user does not have to create VQL statements to import
   these wrappers manually. ITPilot includes options to automatically
   generate the necessary VQL for these tasks. The use of statements
   generated automatically by ITPilot is strongly recommended.


.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER ITP statement
   :name: Syntax of the CREATE WRAPPER ITP statement

   CREATE [ OR REPLACE ] WRAPPER ITP <name:identifier>
       [ FOLDER = <literal> ]
       [ VERIFICATION { TRUE | FALSE } ]
       [ DENODOBROWSEROPTIMIZATION { TRUE | FALSE } ]
       [ PASSTHROUGH { TRUE | FALSE } ]
       [ [ JSCRIPT ] <script code:literal> ]
       [ [ MODEL ] <model xml:literal> ]
       [ SCANNERS ( <scanner name:literal> [, <scanner name:literal> ]* ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

   <source configuration property> ::=
       DATAINORDERFIELDSLIST = { DEFAULT | ( <name:identifier> { ASC | DESC }
       [, <name:identifier> { ASC | DESC } ]* ) }


The command for modifying WWW wrappers is similar.

.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER ITP statement
   :name: Syntax of the ALTER WRAPPER ITP statement

   ALTER WRAPPER ITP <name:identifier>
       [ VERIFICATION { TRUE | FALSE } ]
       [ DENODOBROWSEROPTIMIZATION { TRUE | FALSE } ]
       [ PASSTHROUGH { TRUE | FALSE } ]
       [ [ JSCRIPT ] <script code:literal> ]
       [ [ MODEL ] <model xml:literal> ]
       [ SCANNERS ( <scanner name:literal> [, <scanner name:literal> ]* ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

..

   <source configuration property> ::= (see :ref:`Syntax of the
   CREATE WRAPPER ITP statement`)


The ``VERIFICATION`` clause allows enabling/disabling the ITPilot
automatic maintenance system for the wrapper. See the ITPilot
documentation for further details.

If ``DENODOBROWSEROPTIMIZATION`` is ``TRUE``, ITPilot will use the
optimization data of the Denodo Browser-based sequences.

If the ITPilot wrapper has two input parameters that were marked as
“Pass-through session credentials” and ``PASSTHROUGH`` is ``TRUE``,
these two parameters will not belong to the schema of the base views
created over this wrapper. At runtime, the Server will set the value of
these input parameters to the user name and password of the user that
queries the view.

If ``PASSTHROUGH`` is ``FALSE``, these fields will be part of the base
views.

Lastly, certain wrapper properties can be specified
(``SOURCECONFIGURATION``). These properties are used to determine the
operations that can be executed over the wrapper. The applicable
properties are indicated in the corresponding statement declaration
(:ref:`Syntax of the CREATE WRAPPER ITP statement`), and are explained
in the section :ref:`Wrapper Configuration Properties`.

ITPilot wrappers may contain Custom components. The syntax of the
command to create them is the following:



.. code-block:: bnf
   :caption: Syntax of the CREATE CUSTOMCOMPONENT statement
   :name: Syntax of the CREATE CUSTOMCOMPONENT statement

   CREATE [ OR REPLACE ] CUSTOMCOMPONENT <name:literal>
       [ JSCRIPT ] <javascript:literal>
       [ MODEL ] <xml:literal>

