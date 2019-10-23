==========================================
Managing the Rules of the Resource Manager
==========================================

This section shows the syntax of the commands to create, modify and delete rules of the
resource manager.

.. code-block:: bnf
   :caption: Syntax of the CREATE RESOURCE_MANAGER RULE statement
   :name: Syntax of the CREATE RESOURCE_MANAGER RULE statement

   CREATE [ OR REPLACE ] RESOURCE_MANAGER RULE <name:identifier>
       [ DESCRIPTION = <description:literal> ]
       CONDITION <condition>
       RESOURCE_MANAGER PLAN <name:identifier>

..

   <condition> ::= (see :ref:`Rules for forming functions`)

   <identifier> ::= (see :ref:`Basic primitives for specifying VQL statements`)





.. code-block:: bnf
   :caption: Syntax of the ALTER RESOURCE_MANAGER RULE statement
   :name: Syntax of the ALTER RESOURCE_MANAGER RULE statement

   ALTER RESOURCE_MANAGER RULE <name:identifier>
       [ RENAME <new_name:identifier> ]
       [ DESCRIPTION = <description:literal> ]
       [ CONDITION <condition> ]
       [ RESOURCE_MANAGER PLAN <name:identifier> ]


To list the existing rules, run ``LIST RESOURCE_MANAGER RULES``.

To drop a plan, execute ``DROP RESOURCE_MANAGER RULE``:



.. code-block:: bnf
   :caption: Syntax of the DROP RESOURCE_MANAGER RULE statement
   :name: Syntax of the DROP RESOURCE_MANAGER RULE statement

   DROP RESOURCE_MANAGER RULE [ IF EXISTS ] <name:identifier>









