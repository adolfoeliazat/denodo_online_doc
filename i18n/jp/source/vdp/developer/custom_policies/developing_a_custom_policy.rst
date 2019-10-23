==========================
Developing a Custom Policy
==========================

A custom policy is a Java class with some annotations that mark the
class as a custom policy and indicate which method the Server has to
execute to intercept the query before executing it. Every time a custom
policy is executed, the Server creates a new instance of the class.

We strongly recommend using the Denodo4E plugin for Eclipse, to develop
custom policies (see the file ``README`` in
:file:`{<DENODO_HOME>}/tools/denodo4e`).

To develop a custom policy, add the
:file:`{<DENODO_HOME>}/lib/contrib/denodo-commons-commons-custom.jar` file to
the Classpath of your environment.

The :ref:`Virtual DataPort API Javadoc` provides the documentation for the classes and annotations of this API.

There is a sample custom policy in the directory
:file:`{<DENODO_HOME>}/samples/vdp/customPolicies/`

This custom policy limits the number of concurrent queries that a
user/role can execute over the same view or stored procedure. This
custom policy has one input parameter called “Limit”, which sets the
maximum number of concurrent queries this user/role can execute.

The ``README`` file of this directory explains how to compile the
example and import it into Virtual DataPort.

To develop a custom policy, create a new Java class and annotate it with
the annotation ``com.denodo.common.custom.annotations.CustomElement``.
This annotation has the following parameters:

-  ``type``: it has to be
   ``com.denodo.common.custom.annotations.CustomElementType.VDPCUSTOMPOLICY``
-  ``name``: name of the custom policy. The Administration Tool displays
   this value in the list of custom policies.

To access to the context of the query, add an attribute of the class
``com.denodo.common.custom.policy.CustomRestrictionPolicyContext`` and
annotate it with ``com.denodo.common.custom.annotations.CustomContext``.

At runtime, this attribute will hold the context of the query. That is:

-  The query the user wants to execute: ``getQuery()``.
-  The fields involved in the query. That is, all the field in the
   ``SELECT``, ``WHERE``, ``GROUP BY`` and ``HAVING`` clauses:
   ``getFieldsInQuery()``.
-  The user who executes the query and her roles:
   ``getCurrentUserName()`` and ``getCurrentUserRoles()``.
-  The user agent associated to the connection of the user that executes
   the query: ``getCurrentUserAgent()``.
-  Database where the query is executed: ``getCurrentDatabaseName()``.
-  User / role to whom the custom policy was assigned:
   ``getPolicyCredentialsName()`` and ``getPolicyCredentialsType()``.
   The latter method returns if the policy is assigned to a user or a
   role.
-  View / stored procedure that the custom policy was assigned to:
   ``getElementType()`` and ``getElementName()``.
-  Properties of the query. Invoke ``getProperty(...)`` to obtain the
   value of the property and ``setProperty(...)`` to change it. The
   available properties are the constants defined in the
   ``CustomRestrictionPolicyContext`` class: ``I18N_PROPERTY``,
   ``SWAP_PROPERTY``, etc.
-  Provides a JMX connection to the Virtual DataPort server that the
   custom policy can use to retrieve any data via JMX:
   ``getJmxConnection()``.
-  Provides a method to log a message in the Server’s logging system:
   ``log(...)``.

When a custom policy is executed, the Server will execute the method
marked with the annotation
``com.denodo.common.custom.annotations.CustomExecutor``.

The Java class of the custom policy must have one and only one method
marked with the annotation ``CustomExecutor``. This method has to return
a ``com.denodo.common.custom.policy.CustomRestrictionPolicyValue``
object.

To add parameters to the custom policy, add a parameter to this method
and annotate it with
``com.denodo.common.custom.annotations.CustomParam``. This annotation
has two parameters:

-  ``name``: the Administration Tool uses this parameter to display
   information about the custom policy to the users.
-  ``mandatory``: boolean value that indicates if this parameter is
   optional.

The value of these parameters is set when assigning the custom policy to
a Virtual DataPort user or role.

The class ``CustomRestrictionPolicyValue`` (class of the objects
returned by the policy) has two constructors:

-  ``CustomRestrictionPolicyValue(CustomRestrictionPolicyType policyType)``
   Constructs a ``CustomRestrictionPolicyValue`` without imposing any
   restriction.
-  ``CustomRestrictionPolicyValue(     CustomRestrictionPolicyType policyType,      CustomRestrictionPolicyFilterType filterType,        String condition,         Set<String> sensitiveFields)``
   Constructs a ``CustomRestrictionPolicyValue``, which may impose a
   restriction.

``CustomRestrictionPolicyType`` is an ``enum`` with the following
fields:

-  ``REJECT``: it means that the policy rejects the query.
-  ``ACCEPT``: it means that the policy accepts the query and the Server
   will execute it.
-  ``ACCEPT_WITH_FILTER``: it means that the policy accepts the query
   but it sets some restrictions, which are determined by the fields of
   the second constructor: ``filterType``, ``condition`` and
   ``sensitiveFields``.

If you want the custom policy to accept (``ACCEPT``) or reject
(``REJECT``) the query, instance the object using the first constructor.

If you want the custom policy to accept the query with some restrictions
(``ACCEPT_WITH_FILTER``), use the second constructor and provide a
non-null value for ``filterType``, ``condition`` and
``sensitiveFields``. In this case, the custom policy works in the same
way as a restriction, so you have to define a condition, a set of
sensitive fields and a type of filter.

The object ``CustomRestrictionPolicyFilterType`` tells the Server what
to do when the query returns a row that does **not** verify the
``condition``. The filter can be:

-  ``REJECT_ROW``: the Server will only include in the result of the
   query, the rows that verify the condition set by the parameter
   ``condition``.
-  ``REJECT_ROW_IF_ANY_SENSITIVE_FIELDS_USED``: if the query uses at
   least one field of ``sensitiveFields``, the Server will only return
   the rows that verify ``condition``.
   If the query uses none of the ``sensitiveFields``, the Server will
   not filter any row.
-  ``REJECT_ROW_IF_ALL_SENSITIVE_FIELDS_USED``: if the query uses *all*
   the fields of ``sensitiveFields``, the Server will only return the
   rows that verify ``condition``.
   If the query does not use *all* the fields in ``sensitiveFields``,
   the Server will not filter any row.
-  ``MASK_SENSITIVE_FIELDS_IF_ANY_USED``: if the query uses at least one
   field of ``sensitiveFields``, the Server will set to ``NULL`` the
   fields in the ``Set sensitiveFields`` of the rows that do *not*
   verify ``condition``.
   If the query does not use any field in ``sensitiveFields``, the
   Server will not mask any field.
-  ``MASK_SENSITIVE_FIELDS_IF_ALL_USED``: if the query uses *all* the
   fields of ``sensitiveFields``, the Server will set to ``NULL`` the
   fields in the ``Set sensitiveFields`` of the rows that do *not*
   verify ``condition``.
   If the query does not use *all* the fields in ``sensitiveFields``,
   the Server will not mask any field.
   
If an input parameter of the custom policy is a datetime value, check the section :doc:`Dealing with Datetime and Interval Types <../developing_extensions/developing_custom_functions/dealing_with_datetime_and_interval_types>` for custom functions. The rules to manage input parameters in custom policies are the same as in custom functions.
