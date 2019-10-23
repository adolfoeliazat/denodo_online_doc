============================================================
Resource Manager: Modify the Priority of a Query Dynamically
============================================================

You can jump to the next section if the Virtual DataPort server runs on Windows.

The Resource Manager of Virtual DataPort allows, among other things, to
modify the priority of the queries executed. See more about feature in
the section :doc:`Resource Manager <../../../../vdp/administration/resource_manager/resource_manager>` of the Administration Guide.

If you are planning to use modify the priority of the queries and the
Virtual DataPort server runs on Linux, you have to do the following:

#. Start the Virtual DataPort with the ``root`` user.
#. And add this parameter to the “JVM options” of the Virtual DataPort
   server:
   ``-XX:ThreadPriorityPolicy=1``.
   
   The section :ref:`Denodo Platform Configuration` explains 
   how to modify the JVM options of each module.

The reason for having to do this is that on Linux, only processes
launched by the ``root`` user can change the priority of its threads
dynamically. If these conditions are not met, the priorities of the
queries will not change even if a restriction plan indicates so.
