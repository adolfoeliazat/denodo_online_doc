========
Handlers
========

Denodo Scheduler allows new custom handlers to be created.

 

To create a new *handler* the interface
``com.denodo.scheduler.api.handler.Handler`` needs to be implemented.
This interface has the following methods:

-  ``init``. Initializes the handler.
-  ``execute``. Method invoked by Scheduler once the extraction and
   exporting of all the tuples of a job have finished.

 

For more information, refer to the :ref:`Scheduler API Javadoc`
or the example for creating a handler in
:file:`{<DENODO_HOME>}/samples/scheduler/handler-api`.

 
