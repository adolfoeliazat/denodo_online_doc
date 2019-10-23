============
Threads Pool
============

The Virtual DataPort Execution Engine can execute queries concurrently
and for each query, each request to a data source is performed
concurrently as well.

To achieve this, Virtual DataPort uses a multithreading subsystem. In
the “Threads pool” dialog you can change the settings of this system.

.. note:: Only change these settings if you know what you are doing. The
   default values are correct for the majority of scenarios.

To change these settings, click **Server configuration** on the menu
**Administration** and then, click the tab **Threads pool**.

In this dialog, you can configure the following:

-  **Maximum number of threads**: Maximum number of threads the pool can
   support simultaneously (default value is 300).
-  **Expiration time (milliseconds)**: Maximum time an unused thread
   remains in the pool (default value is 600,000 milliseconds).
-  **Sleep time (milliseconds)**: Indicates how often the threads not
   used in the pool are checked to eliminate those that exceed the
   “Expiration time” (default value is 300,000 milliseconds).
-  **Timeout (milliseconds)**: Maximum time allowed by the pool for
   execution of one of the threads (default value is 43,200,000
   milliseconds; i.e. 12 hours).
-  **Checkout time (milliseconds)**: Maximum time the pool waits for a
   free thread. If this time is exceeded, the system displays an error
   message (30,000 milliseconds by default).

The changes to these properties will be effective immediately.
