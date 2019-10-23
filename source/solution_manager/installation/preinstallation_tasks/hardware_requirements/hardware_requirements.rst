==================================
Hardware Requirements
==================================

The Solution Manager has the following hardware requirements:

.. table:: Hardware requirements of the Solution Manager
   :name: Hardware requirements of the Solution Manager

   +----------------------------+------------------------------------------------+
   | **Processor**              | Intel Xeon quad-core or similar.               |
   +----------------------------+------------------------------------------------+
   | **Physical memory (RAM)**  | 16 gigabytes of memory so the Solution         |
   |                            | Manager components can allocate a runtime      |
   |                            | heap space up to 8 gigabytes.                  |
   +----------------------------+------------------------------------------------+
   | **Disk space**             | Minimum: 5 gigabytes                           |
   +----------------------------+------------------------------------------------+
   
The Solution Manager can be installed on a virtual machine with similar 
characteristics, provided that equivalent resources are permanently allocated.

It is very important to avoid *memory overcommitment*. That is, the amount of memory
assigned to the virtual machine has to be backed by physical memory. 
Otherwise, the host operating system will have to swap to disk parts of the virtual
machine. This will lead to a severe decrease of the performance of the Denodo 
Solution Manager.  

|

The amount of free disk space required depends on several factors:

-  A full installation of the Solution Manager uses 1 GB initially.
-  The metadata of each module of the Solution Manager is stored locally.
-  Space used by the revisions created by the administrators.
-  Space used by updates: Denodo keeps a backup copy of the libraries
   that are being updated.
