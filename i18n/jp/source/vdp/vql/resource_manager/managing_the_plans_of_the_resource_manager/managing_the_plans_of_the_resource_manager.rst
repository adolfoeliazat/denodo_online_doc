==========================================
Managing the Plans of the Resource Manager
==========================================

This section shows the syntax of the commands to create, modify and delete plans of the
resource manager.



.. code-block:: bnf
   :caption: Syntax of the CREATE RESOURCE_MANAGER PLAN statement
   :name: Syntax of the CREATE RESOURCE_MANAGER PLAN statement

   CREATE [ OR REPLACE ] RESOURCE_MANAGER PLAN <name:identifier>
       [ DESCRIPTION = <description:literal> ]
       CONDITION <condition>
       ACTION <literal> [ PARAMETERS ( <parameters> ) ]*
       [ ACTION <literal> [ PARAMETERS ( <parameters> ) ]* ]*
       [ CONDITION <condition>
         ACTION <literal> [ PARAMETERS ( <parameters> ) ]*
         [ ACTION <literal> [ PARAMETERS ( <parameters> ) ]* ]*
       ]*

   <parameters> ::= <param name:literal> = <value> [, <param name:literal>
   = <value> ]

..

   <condition> ::= (see :ref:`Rules for forming functions`)

   <identifier> ::= (see :ref:`Basic primitives for specifying VQL statements`)

   <literal> ::= (see :ref:`Basic primitives for specifying VQL statements`)

   <value> ::= (see :ref:`Rules for forming functions`)

.. code-block:: bnf
   :caption: Syntax of the ALTER RESOURCE_MANAGER PLAN statement
   :name: Syntax of the ALTER RESOURCE_MANAGER PLAN statement

   ALTER RESOURCE_MANAGER PLAN <name:identifier>
       [ RENAME <name:identifier> ]
       [ DESCRIPTION = <description:literal> ]
       [ CONDITION <condition>
         { ACTION <literal> [ PARAMETERS ( <parameters> ) ]* }+
       ]*


To list the existing plans, run ``LIST RESOURCE_MANAGER PLANS``.

To drop a plan, execute ``DROP RESOURCE_MANAGER PLAN``:



.. code-block:: bnf
   :caption: Syntax of the DROP RESOURCE_MANAGER PLAN statement
   :name: Syntax of the DROP RESOURCE_MANAGER PLAN statement

   DROP RESOURCE_MANAGER PLAN [ IF EXISTS ] <name:identifier> [ CASCADE ]


You cannot delete a plan if it has rules associated to it, unless you
add the ``CASCADE`` clause. With the ``CASCADE`` clause, you will delete
the plan and the rules associated to it.

