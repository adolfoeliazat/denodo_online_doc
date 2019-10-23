=======================
Dependencies Among Jobs
=======================

When editing a job, it is possible to specify the job(s) this one
depends on. The cron time trigger has a section for specifying this
purpose. Each trigger may have its own dependencies. By default, a
trigger will not execute the job unless the jobs it depends on have
successfully finished their execution (including all their retries, if
configured). This behavior may be altered by checking the *Execute
this job even though any of its dependencies finish with error*
check box, in order to let the job be executed having its dependencies
finished successfully or not. Optionally, the maximum time (in
milliseconds) for waiting for the dependencies to be completed can be
specified.

 

It is important to note that jobs cannot depend on jobs that are on
draft state. Nevertheless, draft jobs may have dependencies (and they
will be kept if the project is exported).

 

If a job is waiting for a trigger’s dependencies to be satisfied and
then the jobs is executed as consequence of other trigger, then the wait
for the first trigger dependencies is reset.

 

Example: Suppose a job “A”, having a trigger that depends on a job “B”,
starts its execution. Then, “A” will be in WAITING state until job “B”
completes its execution (note that if “B” is already being executed when
“A” starts, then “A” will be in WAITING state until “B” starts a new
execution and finishes it). If job “B” has retries (because of the Retry
Handler configuration), “A” will keep waiting until all the retries of
“B” are executed. When “B” finishes,


a. If it finishes successfully, then job “A” will be executed.
#. If it finishes with error, then job “A” will stop waiting and will
   not be executed (nor the jobs that follows “A” in the chain of
   dependencies). A message informing about this circumstance will be
   added to the report.
#. If it finishes with error and the job “A” was configured to be
   executed even though any of its dependencies finish with error, then
   job “A” will be executed.
 

.. note:: If “A” and “B” are configured to be executed at the same time,
   sometimes “B” could start before “A”, and other times “A” could start
   before “B”:
   
   
   -  If “A” starts before “B”, then “A” is executed when “B” completes its
      execution
   -  If “B” starts before “A”, then “A” is executed when “B” completes its
      next execution (not the current one). This means that “A” will be in
      WAITING state until the next time job “B” is scheduled to run.
   
   This way, if “A” depends on “B” and “B” depends on “C”, then “A” should
   be scheduled to be the first job to be executed. The following, would be
   an example of scheduling:
   
   -  “A” starts at time X.
   -  “B” starts at time X + 1 second.
   -  “C” starts at time X + 2 seconds.
   
    
   
   If the timeout of A is reached before B finishes, the behavior will be
   like in b), with the message “Timeout exceeded for this job, while
   waiting for dependencies to be satisfied.” added to the report.
   
    
   
   If while B is being executed, another trigger causes the execution of A,
   then the first trigger waiting is reset. Therefore, when B finishes, A
   will not be executed. Another execution of B starting and ending while A
   is waiting would be required for the dependencies of the first trigger
   to be satisfied.
   
    
   
   If “B” were deleted, the configuration of job “A” would be updated
   (removing this dependency from the trigger/s that have it) and would
   change to the DISABLED state (although if “A” were in the RUNNING state,
   it would continue execution until finishing and, then, would change to
   DISABLED state). The user must then enable or edit the job to
   re-schedule it.
