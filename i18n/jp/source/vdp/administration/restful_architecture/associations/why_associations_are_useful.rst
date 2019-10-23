============================
Why Associations Are Useful?
============================

There are several reasons why you should define associations between
views when appropriate:

#. From a performance perspective, some queries are executed much faster; there are some optimizations that the query optimizer can only apply if the appropriate associations are defined.
#. Virtual DataPort publishes information about the associations marked as referential constraints. Some business intelligence tools take into account this information to execute more efficient queries. 
#. From a usability perspective, using the HTML representation of the
   RESTful Web service, you can browse across views that have an
   association between them.
#. From a metadata perspective, they help define a better semantic model
   because you can define referential constraints (primary key-foreign
   key relation) between views.
   
   At runtime, third-party modeling tools that connect to Virtual DataPort can extract information about the foreign key relations established between views.
#. From the perspective of the *Data Catalog*, the associations allow business users to discover views that are related. For example:

   -  When querying a view, you can traverse its associations.
   -  The associations of the views are displayed in the :ref:`Relationships <Relationships>` dialog.


