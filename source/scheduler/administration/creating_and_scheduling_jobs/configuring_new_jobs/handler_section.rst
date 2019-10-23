===============
Handler Section
===============

The last step before completing the execution of a job is to execute the
handlers that have been configured.


Denodo Scheduler allows one or several of the following types of
handlers to be added for a job:

-  **Mail**. Allows the results report on the job execution to be sent by
   e-mail (see section :doc:`../jobs/jobs`, **Reports** action on a job). Requires a
   list of destination e-mail addresses to be specified (**Mail
   addresses**) and allows the following sending conditions to be set:


   -  **Always**. An e-mail is sent to the recipients each time a job
      execution is completed.

   -  **When any of the following conditions happen**:

      -  **Errors**. An e-mail will be sent when there have been errors in the
         execution of the job (any errors that have occurred before the
         handlers’ stage, i.e. extraction/filtering/export errors).
      -  **The number of exported documents is less than: XX**. An e-mail will
         be sent when the number of exported tuples is below the configured
         threshold.
      -  **The number of exported documents is more (or equals) than: XX**. An
         e-mail will be sent when the number of exported tuples is greater or
         equals than the configured threshold.
         
         
-  **Custom**. Users can also create their own custom handlers. See section :ref:`Handlers`.

.. important:: "*Move-File-Repository*" handler is deprecated and will be removed in a later version of the Denodo Platform.
  No new "*Move-File-Repository*" handlers can be created in version 7.0, but you could edit and use the existing ones. 
  You can see the documentation from `previous version <https://community.denodo.com/docs/html/browse/6.0/scheduler/administration/creating_and_scheduling_jobs/configuring_new_jobs/handler_section>`_.
  

Handlers can be configured to be executed only on the last retry of the
job (see section :ref:`Retry Section`) by checking the option **Execute
handler only on last retry**. Otherwise, they will be executed on all
the retries of the job. In the latter case, they can check, during it
execution, if the job is going to be retried (using the ``JobReport``
object received by the method ``execute``; see section :ref:`Handlers` for
more information).
