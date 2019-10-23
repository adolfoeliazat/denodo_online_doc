=============
Logical Layer
=============

.. toctree::
   :hidden:
   
   data_module_cache.rst  

The logical layer integrates and combines the relations exported by the
different wrappers (called base relations or base views) to create the
views that will comprise the system global schema.

Once the *base relations* representing the system sources have been
created, the administrator can create views that combine them as
required, thus creating the *global schema views* (or derived views). It
is important to point out that this process can be carried out in a
recursive manner in several steps: a *derived view* can be used as a
base to create new *views*, thus allowing combinations of arbitrary
complexity. Views are defined using the Denodo VQL language - described in the :doc:`/vdp/vql/index` - 
although, as explained in the next sections, the administration tool
allows graphically creating them, so the VQL statements do not have to
be manually written.

Once the *views of the* *global schema* have been created by combining
source data, the logical layer is capable of responding to queries
expressed in VQL both on *derived* *views* and on *base relations*.

The VQL query language is SQL-based, but it incorporates different
extensions to handle heterogeneous and distributed data. For example,
VQL includes certain commands to allow querying non-structured data and
combining it with structured data. It also supports compound types such
as arrays and registers.

When the system receives a query, it checks that it can be resolved
depending on the query capabilities supported by the data sources, it
then draws up the possible execution plans, selects the most suitable
one and executes the query returning the results obtained to the higher
layer.

The logical layer of Virtual DataPort also allows writing to data
sources using INSERT/UPDATE/DELETE operations, provided that these are
capable of supporting transactions.

The following modules can be differentiated in the logical layer:

-  **Query Plan Generator**: Firstly, the plan generator decides if the
   query received can or cannot be answered in accordance with the query
   capabilities supported by the data sources. Where it is possible, it
   generates the possible execution plans for the query.
-  **Optimizer**: Aims to select the optimum execution plan from all the
   options (obtained by the Query Plan Generator). The query
   capabilities of the sources are also considered so, when possible,
   the processing of some operations is delegated to the data sources,
   thus achieving a more efficient execution and less data exchange
   through the network. Other aspects taken into account are the most
   optimal execution strategies for join operations.
-  **Query Execution Engine**:
   Once the optimum plan has been selected, the execution engine is
   responsible for putting it into practice, executing the necessary
   subqueries on the sources and integrating the results obtained to
   generate the global response. In turn, the execution engine takes
   into consideration that information from the sources which is already
   preloaded in the cache module, whereby unnecessary access to data
   sources is avoided, thus achieving greater efficiency.
