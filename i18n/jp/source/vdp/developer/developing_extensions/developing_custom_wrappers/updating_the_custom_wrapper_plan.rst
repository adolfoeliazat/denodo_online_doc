================================
Updating the Custom Wrapper Plan
================================

The method ``getCustomWrapperPlan()`` returns a ``CustomWrapperPlan``
object that represents the current wrapper execution plan. This object
allows adding information to the wrapper plan that will be displayed in
the execution trace. To add information to the wrapper plan, invoke the
method ``addPlanEntry(String title, String entry)``. For example, if the
custom wrapper queries a database, it could be useful to add to the
wrapper plan information about the query executed in the database.
