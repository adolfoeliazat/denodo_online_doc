====================
Scheduler Client API
====================

The Denodo Scheduler Client API allows jobs, filter sequences and data
sources to be programmatically configured and managed. It also allows to
see the list of jobs, start/stop jobs, manage permissions, etc.

 

To do this, the platform has a facade
``com.denodo.scheduler.client.SchedulerManager`` which allows the
following features:

-  Getting the job list with information on its state (``JobData``):
   ``getJobsInformation``.
-  See a job’s execution reports (``JobReport``): ``getJobsReport``.
-  Create, obtain, update, and delete the configuration of a job, a
   filter sequence, and a Data Source (``Configuration``):
   ``addElement``, ``getElement``, ``updateElement`` and
   ``removeElement``.
-  Enable or disable a job: ``resumeJob`` and ``pauseJob``.
-  Execute a job only once, without taking into account the start and
   end time for its configuration: ``startJob``.
-  Execute a job with its dependencies (see section :doc:`../../creating_and_scheduling_jobs/jobs/jobs`, **Start 
   With Dependencies**): ``startJobWithDependencies``.
-  Execute a job with state. If the execution of a job gives any errors,
   executing it again with state will only execute those queries that
   had errors (see section :doc:`../../creating_and_scheduling_jobs/jobs/jobs`, **Start with state**):
   ``retryOrContinueJob``.
-  Stop a job from running: ``stopJob``.
-  Obtain the types of jobs, filters, and data sources allowed by the
   system: ITP, VDP, etc.: ``getElementTypes`` and
   ``getElementSubTypes``.
-  Pause the job scheduler. When the scheduler is paused, the jobs will
   wait until it is resumed before being executed again. This order will
   only run successfully if no job is being executed:
   ``pauseScheduler``.
-  Resume executing the scheduler when it is paused:
   ``resumeScheduler``.

 

All configuration elements (jobs, data sources, and filter sequences)
are based on the object ``com.denodo.configuration.Configuration``,
which represents a collection of parameters (simple or compound) that
constitute the configuration of the element. The list of parameters and
their specification (data type, mandatoriness, etc.) are given by the
object ``com.denodo.configuration.metadata.MetaConfiguration``.

 

The filter sequences comprise an identifier of the sequence and a list
of filters that make it up, each one also being represented by an object
``Configuration``.

 

The jobs have an identifier, a description, a type (ITP, JDBC, VDP, 
VDPCache or VDPIndexer), some associated handlers, and a time-based scheduler.
VDP- and JDBC-type jobs have an extraction section which specifies where
the data is going to be extracted from (both differ as to the types of
data sources that can be used). ITP-type jobs also have an extraction
section, where the wrapper to be executed is indicated (with its
parameters, if applicable) for obtaining data. All these jobs (but VDPCache) have an export
section, where a list of exporters (Scheduler Index, JDBC, SQL, CSV) is defined.
The jobs also have a handlers section, where a list of them is specified 
(mail, custom handlers) that will be executed after the job export stage.
There is also a retry section that enable configuring when and why a job
should be reexecuted.

 

An example is shown below for creating a VDP-type job, assuming that the
data source has been previously created and has the identifier stored in
the ``vdpDatasourceIdentifier`` variable. This example shows how the
``Configuration`` object for a job is created, using
``ParameterStructure`` objects to specify the job parameters.


.. code-block:: java

      Configuration jobConfig = new Configuration();
      jobConfig.setDraft(false);
      jobConfig.setType("job");
      jobConfig.setSubType("vdp");
      
      ParameterStructure parameters = new ParameterStructure();
      parameters.put(Helper.buildSimpleMonoValuedParam("name", "jobName"));
      parameters.put(Helper.buildSimpleMonoValuedParam("description", "jobDescription"));
      parameters.put(Helper.buildSimpleMonoValuedParam("dataSource", vdpDatasourceIdentifier));
      parameters.put(Helper.buildSimpleMonoValuedParam("parameterizedQuery", "select * from emp"));
      Parameter trigger = new CompoundParameter();
      trigger.setName("trigger");
      Collection<ParameterStructure> params = new ArrayList<ParameterStructure>();
      ParameterStructure param = new ParameterStructure();
      param.put(Helper.buildSimpleMonoValuedParam("type", "cron"));
      param.put(Helper.buildSimpleMonoValuedParam("cronExpression", "0 10,44 14 ? 3 WED"));
      params.add(param);
      trigger.setValues(params);
      parameters.put(trigger);
      jobConfig.setParameters(parameters);
      schedulerManager.addElement(projectId, jobConfig);

 

The name of the parameters to use for configuring each type of element
can be obtained from the XML files in
``DENODO_HOME/metadata/scheduler/elements``. They are organized in
subfolders by element type (data source, job, exporter …).

 

For more information, refer to the :ref:`Scheduler API Javadoc`
and the examples in
:file:`{<DENODO_HOME>}/samples/scheduler/scheduler-api`.
