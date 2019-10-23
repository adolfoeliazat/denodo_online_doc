============
Introduction
============

The Denodo Technologies product suite provides advanced functionalities
for the time-based scheduling of jobs integrating data from disperse and
heterogeneous sources that may be poorly structured.

 

Denodo Scheduler allows the scheduling and execution of data extraction
and integration jobs, as defined on the different modules of the Denodo
Platform. In combination with Denodo Scheduler, the modules of the
Denodo Platform provide features such as the following:

  -  Denodo Virtual DataPort. Scheduling any job that involves the collection
     of data from various dispersed and heterogeneous sources, combining such
     information and exporting it to different types of repositories. It can
     also be used to preload data regularly in the Virtual DataPort cache.
     See the :doc:`/vdp/administration/index` for further information.
     
  -  Denodo ITPilot. Periodic automation of Web data extraction and storage
     or time-based scheduling of Web automation jobs. See the 
     :doc:`/itpilot/user_guide/index`
     for further information.
	 

The main features of Denodo Scheduler include:

  -  Scheduling flexible batch jobs on the different components of the
     Denodo Platform: Virtual DataPort and/or ITPilot.
  -  Generation of detailed reports on the outcome of job execution,
     including detailed information on errors. The reports can be sent by
     e-mail to the addresses that have been configured.
  -  The results obtained by a job can be exported to a CSV file, a SQL
     file, a database, or to an index. It also allows for the inclusion of
     new exporters developed for specific purposes.
  -  Support for data extraction of sources with limited query
     capabilities. For example, consider a Web service or a Web site that
     allows information to be obtained on a particular company based on
     their Tax ID. It is possible to define a job that obtains the
     different Tax IDs of a database server or CSV file and queries the
     Web service or the Web site using each of them.
  -  Persistent jobs. If you restart the system while a job is being
     executed, the job can continue its execution from the last query that
     was successfully executed. In the previous example, if the complete
     job obtains information from 1000 companies and the system restarts
     after the first 200 queries, after the start of the system you can
     launch the execution of the job in the state in which it ended,
     continuing from query 201 instead of starting again from the first
     one.
  -  Transparent retries in the event of failure.
  -  Possibility of configuring a parallel execution of the different
     queries involved in the same job.

