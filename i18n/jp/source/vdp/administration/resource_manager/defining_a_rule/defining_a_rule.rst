===============
Defining a Rule
===============

After creating one or more plans, you have to create rules. Each rule
will have a plan assigned that will be assigned to the user session when
the condition of the rule is met.

To define a rule, follow these steps:

#. Click the **Rules** tab
#. Click **New**.

   .. figure:: DenodoVirtualDataPort.AdministrationGuide-250.png
      :align: center
      :alt: Resource Manager: creating a new rule
      :name: Resource Manager: creating a new rule

      Resource Manager: creating a new rule

3. Enter a name and optionally, a description for the new rule.
#. If you want this rule to be applied to all the user sessions, select
   **Always**. However, you usually want to add a condition to decide if
   you want to apply the plan to the queries of a session. To do this,
   select **Specify condition**. In this case, enter a condition based
   on the attributes of the user’s session.
   
   For example, if the condition is ``user_agent='reports application'``, 
   the Resource Manager will apply the plan to the sessions opened by clients 
   whose property user agent is “reports application”. The section 
   :ref:`Setting the User Agent of an Application` explains how clients 
   can set the property “user agent” when opening a connection.

   To forbid queries from running at certain times of the day, you can use a condition like this:
   
   .. code-block:: sql
   
      user_agent='reports application' AND gethour(now()) >= 8 AND gethour(now()) < 23
   
   A rule with this condition is useful to make sure that
   CPU-intense queries, typical in reporting scenarios, only run at night
   and do not interfere with the daily operational queries.

   To create a condition based on the roles granted to the user that opens the connection to Virtual DataPort, use the attribute “(roles).value”. A condition “(roles).value = <role name>” is true if at least one of the roles assigned to the user is <role name>.

   The appendix :ref:`Resource Manager: Available Fields to Evaluate a Rule` lists all the fields you can use in these conditions.
      
   
#. In the list **Plan name**, select the plan that will be applied to
   the user session when the condition above is met.
#. Click **Ok** to create the rule.

The conditions of the plans are evaluated in the order they are listed
in the “Rules” dialog, from top to bottom. When the condition of a rule
is met, the Resource Manager assigns the plan of the rule to the user
session and stops evaluating more rules.

To change the order in which the Resource Manager evaluates the rules’
conditions, select a rule and click the buttons |image1| or |image2| to
move the rule up or down.

.. rubric:: Example 1

Let us say that you define two rules:

-  Rule A with the condition “user\_name = acme”
-  Rule B with the condition “access\_interface = JDBC”

If a JDBC application connects to Virtual DataPort with the user name
“acme”, the Resource Manager will assign the plan of the rule A. That is
because rule A is the first one found in the list of rules that meets
the condition, even if the user session meets the condition of the rule
B.

If a session does not meet the condition of any rule, the Resource
Manager will not assign any plan to the user session.

.. rubric:: Example 2

To create a rule that only applies to the users of the Data Catalog, create a rule with the condition 
``access_interface in ('Data-Catalog', 'JDBC')``. You need to specify "JDBC" as well because the Data Catalog executes some requests using its own access interface and others, like a regular JDBC application.

This also applies to the Scheduler, the Diagnostic Monitoring Tool and the Solution Manager.

.. |image1| image:: ../../common_images/icon-black-arrow-down.gif
.. |image2| image:: ../../common_images/icon-black-arrow-up.gif

