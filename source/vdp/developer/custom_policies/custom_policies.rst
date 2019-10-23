==========================
Custom Policies
==========================


.. toctree::
   :hidden:

   developing_a_custom_policy


Custom policies are query interceptors, which are invoked before the
Virtual DataPort server executes a query over a view. They are similar
to row restrictions with the benefit that can be customized.

When a user queries a view with a custom policy assigned, the policy can
take one of the following actions:

-  Reject the query.
-  Accept the query without restrictions.
-  Or, accept the query but imposing restrictions such as limit the rows
   returned by the query, add a filter condition, etc.

To select one of these actions, custom policies have access to several
parameters of the queries’ context to decide how to proceed:

-  The query the user wants to execute
-  The user name and their privileges
-  A JMX connection to the Server that the policy can use to access any
   Virtual DataPort data via JMX
-  …

Custom policies are reusable, which means the following:

-  As they are similar to row restrictions, they are assigned in a
   similar manner. Therefore, you can assign the same custom policy over
   several views for a user or a role.
-  They can define configuration parameters. When a policy is assigned
   to a user or a role over a view, you can customize its behavior with
   these parameters. Thanks to this, the behavior of a policy can be
   customized depending on the user or role you assign it to.
   For example, if you develop a policy to limit the number of queries
   over a view, which the users of a role can execute at the same time,
   this number can be a parameter of the policy. That way, you can set a
   limit when assigning the policy to the role “developer” and another
   limit to the role “application”.

When a user queries a view and this user has custom policies assigned
over this view, the policies are evaluated in the following way:

-  Custom policies are not taken into account when the user executing
   the query is an administrator or an administrator of the database.
-  If the user does not have any role and she has custom policies
   assigned over the view, the Server evaluates the policies one by one.
   If one of the policies rejects the query, the query is rejected.
-  If the user has one or more roles assigned (these roles may have
   other roles assigned), the evaluation of custom policies is performed
   in groups. For each role, there is a group formed by the custom
   policies assigned directly to this role and another group with the
   custom policies directly assigned to the user.
   A group rejects a query when at least one of the policies of the
   group rejects the query.
   A group accepts a query when all the policies of the group accept the
   query.
   The query is accepted if at least one group accepts the query.

For example, let us say that there is a user with two roles: R1 and R2.
The user has two policies assigned over the view V: P1 and P2. The role
R1 of the user has another two policies assigned over the view V: P3 and
P4. The role R2 has another two policies assigned over the view V: P5
and P6.

When the user queries the view V, Virtual DataPort evaluates the policy
P1. If P1 accepts the query, it evaluates P2. If P2 also accepts the
query, the Server does not evaluate more policies and executes the
query.

If P1 rejects the query, the Server does not evaluate the other policies
of the user and begins evaluating the policies of the role R1: P3 and
P4. If P3 accepts the query, it evaluates P4. If P4 also accepts the
query, the Server does not evaluate more policies and executes the
query.

If P3 rejects the query, the Server does not evaluate the other policies
of the role R1 and begins evaluating the policies of the role R2: P5 and
P6. If P5 accepts the query, the Server evaluates P6. If P6 also accepts
the query, the Server executes the query.

If P5 rejects the query, the Server does not execute the query.
