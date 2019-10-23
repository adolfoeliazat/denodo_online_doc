.. _vdp_admin_resource_manager:

================
Resource Manager
================

.. toctree::
   :hidden:
   
   defining_a_plan/defining_a_plan.rst
   defining_a_rule/defining_a_rule.rst

In an environment with multiple concurrent user sessions that run
queries, not all user sessions have the same importance. You may want to
give more priority to queries executed by transactional applications,
which need an immediate response, than to queries executed to generate a
daily report.

The Resource Manager allows you to classify sessions into groups based
on the attributes of the session, and to allocate resources to those
groups in a way that optimizes resources utilization for your
application environment.

The Resource Manager introduces two main concepts:

-  Plans
-  Rules

A “plan” is a set of restrictions assigned to a user session. For
example: limit the number of concurrent queries the same user can run,
limit the maximum number of rows of the queries’ results, etc.

A “rule” assigns a “plan” to the session of a user if a certain
condition is met. This condition can use any of the attributes of the
session. I.e. user name, access interface used by the application (JDBC,
ODBC, etc.), IP address, etc.

For every ``SELECT``, ``CALL`` or ``QUERY WRAPPER`` received, the Resource Manager
evaluates the condition of the first rule. If it is met, it assigns the
plan of the rule to this user session. If the condition is not met, it
evaluates the condition of the second rule. If the condition is not met
either, it evaluates the condition of the next rule and so on, until it
finds a rule that meets the condition or there are no more rules.

This process occurs before the execution engine executes the query.

For example, you can create a plan that limits the number of rows
returned by a query to 100,000; and create a rule with the condition
access interface = Web service. By doing this, you limit to a hundred
thousand the number of rows returned by any Web service.

|

The Resource Manager is enabled by default. To check that it is actually
enabled, click **Resource Manager**, on the **Administration** menu.
Then, in the **Configuration** tab, check that the selected option is
**On**.
