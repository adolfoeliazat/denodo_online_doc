==================================
Hardware Requirements
==================================

The Denodo Platform has the following hardware requirements:

.. table:: Hardware requirements of the Denodo Platform
   :name: Hardware requirements of the Denodo Platform

   +----------------------------+------------------------------------------------+
   | **Processor**              | For simple scenarios: Intel Xeon quad-core     |
   |                            | or similar.                                    |
   |                            |                                                |
   |                            | For high-load scenarios or cases with complex  |
   |                            | calculations: 8 cores or more.                 |
   +----------------------------+------------------------------------------------+
   | **Physical memory (RAM)**  | 16 gigabytes of memory so the Virtual DataPort |
   |                            | can allocate a runtime heap space of up to     |
   |                            | 8 gigabytes. The rest is needed for the other  |
   |                            | components of the Denodo Platform and the      |
   |                            | operating system.                              |
   +----------------------------+------------------------------------------------+
   | **Disk space**             | Minimum: 5 gigabytes free                      |
   |                            |                                                |
   |                            | Recommended: 10 gigabytes free                 |
   |                            |                                                |
   |                            | Recommended for very complex                   |
   |                            | scenarios involving billions of                |
   |                            | rows: 100 gigabytes free                       |
   |                            |                                                |
   |                            | See the explanation below.                     |
   +----------------------------+------------------------------------------------+
   | For a High-Availability configuration, an external load-balancer.           |
   +-----------------------------------------------------------------------------+
   
The Denodo Platform can be installed on a virtual machine with similar 
characteristics, provided that equivalent resources are permanently 
allocated.

It is very important to avoid *memory overcommitment*. That is, the amount of memory
assigned to the virtual machine has to be backed by physical memory. 
Otherwise, the host operating system will have to swap to disk parts of the virtual         |
machine. This will lead to a severe decrease of the performance of the Denodo Platform.  

|

For more advice regarding sizing, contact the `Denodo Support Team <https://support.denodo.com>`_.

Recommendations when Running Denodo on Amazon AWS
=================================================

In the following table, find the recommended instance types of Amazon AWS to run the Denodo Platform:

.. table:: Recommended instance types

   +----------------------------------------+------------------------------------------------+
   | **Recommended instance type**          | *General Purpose* instance of type *M*         |
   +----------------------------------------+------------------------------------------------+
   | **Recommended minimum for production   | *m4.xlarge* or *m5.xlarge*                     |
   | servers**                              |                                                |
   +----------------------------------------+------------------------------------------------+
   | **Recommended for production servers   | *m4.4xlarge* or *m5.4xlarge*                   |
   | in very complex scenarios**            |                                                |
   |                                        | On scenarios where Denodo executes very        |
   |                                        | complex queries, these instances provide       |
   |                                        | significant performance improvements over less |
   |                                        | powerful instances. More powerful instances    |
   |                                        | (e.g. *m4.10xlarge*, *m5.10xlarge*) may be     |
   |                                        | beneficial with some workloads but the         |
   |                                        | improvement is usually smaller.                |
   +----------------------------------------+------------------------------------------------+

Recommendations when Running Denodo on Microsoft Azure
======================================================

In the `Denodo page <https://azuremarketplace.microsoft.com/en-us/marketplace/apps/denodo.denodo-platform-7_0?tab=PlansAndPrice>`_ of the Microsoft Azure Marketplace, the tab *Plans + Pricing* lists the instance types we recommend to run the Denodo Platform.

Running the Denodo Platform on Docker
=====================================

The Denodo Platform can run on Docker.

Denodo distributes a Docker image you can download from the `Denodo Support Portal <https://support.denodo.com/resources/installer/list/Denodo%207.0>`_. The `Denodo Platform Container Quick Start Guide <https://community.denodo.com/docs/html/document/7.0/DenodoPlatformContainerQuickStartGuide>`_ explains how to launch and use this image.

Disk Space Requirements of the Denodo Platform
==============================================

The amount of free disk space required depends on the use of the Denodo
Platform:

-  A full installation of the Denodo Platform (i.e. installation of all
   the modules: ITPilot, Scheduler and Virtual DataPort) uses
   1.5 gigabytes.
-  The metadata of each module of the Denodo Platform is stored locally
   (e.g. information about the data sources, views, web services,
   Scheduler jobs, etc.) and usually does not go over a few hundred
   megabytes.
-  Space used by updates: Denodo keeps a backup copy of the libraries
   that are being updated. Each update takes up to 300 megabytes.
   Considering the number of updates released per each major version,
   the backup copies end up using around 5 gigabytes.
-  Space used for swapping data: to avoid memory overflows, Virtual
   DataPort swaps to disk the intermediate results of queries when they
   do not fit in memory.
   
   Usually, most of the processing is pushed down to the source so the 
   number of rows processed by Virtual DataPort is low and swapping is 
   not required. However, with very complex queries where Virtual 
   DataPort processes billions of rows, some data is swapped to disk 
   to avoid a memory overflow. This is why, for complex scenarios, we recommend having 100 
   gigabytes of free disk space.
   
   The section :doc:`Memory Management <../../../../vdp/administration/memory_management/memory_management>` of
   the Virtual DataPort Administration Guide explains when Virtual DataPort 
   swaps intermediate results to disk.

Requirements for the Virtual DataPort Administration Tool
==================================================================

Any modern computer running Windows or Linux can run the Virtual DataPort Administration Tool.

.. table:: Recommended requirements for the Virtual DataPort Administration Tool

   +------------------------------------------------------------------------+------------------------------------------------------------------------+
   | **Processor**                                                          | Quad Core processor                                                    |
   +------------------------------------------------------------------------+------------------------------------------------------------------------+
   | **Physical memory (RAM)**                                              | 8 gigabytes                                                            |
   +------------------------------------------------------------------------+------------------------------------------------------------------------+
   | **Disk space**                                                         | 1.5 gigabytes free                                                     |
   +------------------------------------------------------------------------+------------------------------------------------------------------------+
   | **Operating System**                                                   | Any of the ones listed in the section                                  |
   |                                                                        | :ref:`Supported Platforms <platform_installation_supported_platforms>` |
   +------------------------------------------------------------------------+------------------------------------------------------------------------+
