============
UNION Clause
============

The operator ``UNION`` combines the result of two queries into a single result set that contains all the rows of both queries. 

The rules of SQL standard define that:

-  The number and the order of the columns of the queries involved in the UNION has to be the same.
-  The data types of each field have to be compatible.
-  The UNION does not return duplicated rows. If there are duplicated rows, the UNION removes them. If you want to keep the duplicates, you have to use ``UNION ALL``.

In Denodo, the behavior of the UNION operator is different from the SQL standard:

-  The association of fields of the queries is done by name, not by position.
-  If a field is present in one query but not the other, the UNION is executed anyway. The field is added to the result set as well. In the rows of the query that does not return this field, the value of this field is ``null``.
-  The UNION operator of Denodo does not remove duplicates (it behaves like ``UNION ALL``). If you want to remove duplicates, add the clause ``DISTINCT``.

Enabling the Standard SQL Union
=======================================

You can configure the UNION operator of Denodo to behave as it is defined in the SQL standard. 

Consider the performance implications of enabling this feature: on any database and in Denodo, the process of removing the duplicate rows of a UNION is costly because the execution engine has to evaluate all the rows of the result of the union looking for duplicates, instead of returning them immediately.

If you know that all the rows returned by each branch of the UNION are unique, execute UNION ALL instead because the query will be run faster.

To configure the UNION to behave as defined in the SQL standard, follow these steps:

1. Log in to the administration tool with an administration user.
#. Open the VQL Shell and execute this command:
  
   .. code-block:: vql
        
      SET 'com.denodo.vdb.union.enableStandardSQLUnion' = 'true';
    
   You do not need to restart the Virtual DataPort server to apply this change; it is applied instantly.
   
   You do need to restart the administration tool so the wizard to :ref:`create standard union views<Creating Standard Union Views>`
   is available.
  
Once enabled, the operations ``UNION [ DISTINCT ]`` and ``UNION ALL`` will behave
as defined by the SQL standard. It will be possible to continue using the 
extended union operation using the operator ``EXTENDED UNION ALL``.

To maintain backward compatibility, the views that were created before changing this option and use the UNION operator will keep behaving in the same way.

To revert to the previous behavior, execute this command:

.. code-block:: vql
  
   SET 'com.denodo.vdb.union.enableStandardSQLUnion' = 'false';



