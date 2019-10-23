===================
Swapping Parameters
===================

When a query uses non-streaming operations and in addition, it processes
a large number of rows, the amount of memory needed can increase
rapidly. To avoid excessive consumption, Denodo can swap part of the
data to disk. This behavior is controlled by the following parameters:

-  **Maximum size in memory of each node**. It limits the maximum amount
   in memory that Virtual DataPort allows a non-streaming operator to
   use. If Denodo estimates that this limit will be surpassed, Denodo
   will swap rows to disk to avoid it. This parameter also influences
   the memory usage at cache load processes (see section :ref:`Cache Load
   Processes` for details).
-  **Maximum size of the blocks stored in swap (KB)**. When Denodo swaps
   rows to disk, this is the maximum block size it will use for write
   operations.

Usually, you do not have to change the default values of these
parameters. Nevertheless, there are some scenarios where changing them
in a specific view brings some performance gains:


-  Increasing the swap size of a view can avoid swapping, which improves
   performance. For instance, if a certain join query / view is executed
   using the hash method, the performance of the execution can potentially
   be improved by increasing the “Maximum size of memory of each node”
   parameter of the join view, when the following conditions are met:

   a. There are rows of the view on the right side of the join being
      swapped to disk. To check this, open the execution trace of the query
      and check if the property “Swapping” is “yes”.
   b. There is spare space in the heap of the Java Virtual Machine (JVM).
   c. And the rows entering to the join operation from the view on the
      right side will not fill a substantial part of the free memory, so
      the Java Virtual Machine (JVM) will not be forced to do much
      additional garbage collection.

   To change the “Maximum size of memory of each node” property of a view,
   open the “Options” dialog of the view and click the “Memory” tab.


-  Increasing the “Maximum size of the blocks stored in swap (KB)”
   parameter can potentially improve the performance of large order by
   operations, where Denodo may need to write large amounts of data to disk
   (in other operations, the data to write is typically naturally segmented
   by some key, so this parameter will have less significant effect).
   However, increasing this parameter too much can increase memory usage
   and affect performance negatively.


