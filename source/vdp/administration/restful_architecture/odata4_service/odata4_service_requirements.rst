=======================================
Virtual DataPort Privilege Requirements
=======================================

The users of the OData 4 Service must have the following 
privileges assigned:

* At database level:

  * **Connect**

* At view level:

  * **Metadata**

  * **Execute**: if the user does not have **Execute** access to an entire database, 
    but she has it over some of its elements, the OData Service will only list 
    the elements over which she has **Execute** privilege.

The section :ref:`Administration of Databases, Users, Roles and Their Access Rights` explains how to grant privileges to users.
