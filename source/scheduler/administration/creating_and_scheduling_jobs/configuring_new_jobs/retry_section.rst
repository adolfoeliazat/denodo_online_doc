=============
Retry Section
=============

A job execution can be repeated in any of the following exceptional
situations (retries are not enabled by default):


-  **Errors are returned**. In this case there are two options:

   -  **All queries if any error is returned**. The whole job is repeated
      when errors occurred during its execution. The job will only be retried 
      if some errors occurred in the extraction stage.
   -  **Only queries which return an error**. When there have been errors
      in the execution of the job (during the extraction stage), it only
      repeats those queries that have returned an error or have not started
      to be executed. This option has the same effects as executing the job
      from the “Scheduler” perspective using the “Start with state” option.


- **Exported documents are less than XX**. The whole job is repeated when
   the number of exported documents is less than the specified threshold
   (option **All queries if exported documents are less than: XX**).
 

You can configure for how long retries are performed, in the event that
after one or several retries the errors persist (you can also configure
the **Delay between executions**, set in milliseconds and 0 by default).
The options are as follows:

 

-  **Not limited. Retry while errors**. Retries will be performed until
   no errors are returned.
-  **Limited to: XX**. A maximum of XX retries will be performed even if
   errors persist.

 

.. note:: When using retries and an exporter to a file (CSV or SQL) is
   configured, all the job retries are exported to the same file.
