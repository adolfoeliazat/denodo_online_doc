===================
Defining Base Views
===================

Each source in the system is modeled as a series of base views. Each
base view has attributes like a table in a relational database.

Each attribute of a view has a data type. The type of a specific
attribute delimits which query operators can be applied to it and
certain constraints that the elements of this type should comply with.
The data types supported by Virtual DataPort are organized in two
categories:

#. *Normal*: character strings, integers, floats, dates, etc.
#. *Complex*: *array*-type data (to represent multi-valued data) and
   *register* (to represent register-type data).
   By combining these two data types, Virtual DataPort represents
   hierarchical data structures like the ones obtained from an XML file.

In addition, each base view describes its query capabilities through its
*search methods*. They define which queries can be executed over a base
view. This is necessary because some data sources (e.g. Web sources or
Web services) do not allow any type of query to be made on its data.
Instead, they present limited interfaces (e.g. operations of a Web
Service). If a base view does not have any search method, it cannot be
queried.

Each search method is composed of a series of 5-uples. Each 5-uple
represents a constraint that a specific query should comply with so that
it can be executed on the source using this search method.

The format of a 5-uple is: attribute, operators, obligatoriness,
multiplicity, possible values.

The section :ref:`administration_guide_search_methods` of the Administration Guide explains in
detail what search methods are.