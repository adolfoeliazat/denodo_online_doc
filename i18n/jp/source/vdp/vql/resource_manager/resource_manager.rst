================
Resource Manager
================

In an environment with multiple concurrent user sessions that run
queries, not all user sessions have the same importance. You may want to
give more priority to queries executed by transactional applications,
which need an immediate response, than to queries executed to generate a
daily report.

The Resource Manager allows you to classify sessions into groups based
on the attributes of the session, and to allocate resources to those
groups in a way that optimizes resources utilization for your
application environment.

We recommend managing the Resource Manager from the administration tool.

The section :doc:`/vdp/administration/resource_manager/resource_manager` of the Administration Guide provides
explains how the resource manager works and how to use it from the
administration tool. This section only lists the VQL commands related to
the management of the plans and rules of the resource manager.

.. toctree::

   managing_the_plans_of_the_resource_manager/managing_the_plans_of_the_resource_manager.rst
   managing_the_rules_of_the_resource_manager/managing_the_rules_of_the_resource_manager.rst
