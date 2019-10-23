===============================================
Error Processing on the Web Browsing Automation
===============================================

To complete the configuration of this component, we can access to the
error configuration dialog using the option “Open error configuration”
from the contextual menu.



This action opens a wizard divided in two parts. At the top of the
window we can create *action* records that can be used by some of the
error actions that can be configured to be executed in the bottom part
of the wizard (see below). To add a record, click on the |image0| button
and specify a name. Then click on the |image1| button to open the record
editor (similar to the one used by the Record Constructor component
explained in section :ref:`Processing the individual records: use of Record
Constructor`). There are three optional predefined fields that we can
be added to the action record:

-  ERROR\_TYPE: contains the error type.
-  ERROR\_COMPONENT: the name of the component that fails.
-  ERROR\_MESSAGE: the description of the error.



The bottom side of the wizard allows specifying error handlers and
actions to be executed for each kind of error that can be produced in
the component. The types of errors depend on the type of the component.
In the case of Sequence components the following errors can occur:

-  *Runtime error*: this error occurs when there is some internal error
   in the component execution.
-  *Connection error*: this error occurs when there is some kind of
   connection problem with the source.
-  *Sequence error*: error produced when there is some problem with the
   sequence (the sequence is not correctly written, some command could
   not be run, etc.).
-  *HTTP error*: produced by an http error (e.g. an http 404 error
   because of a page not found).
-  *Timeout error*: this error is produced if the Web source takes a
   long time to respond. The waiting time is configurable. If the
   wrapper is used in the run environment, this parameter is configured
   in the browser pool used. In the generation environment, this
   value is configured in the dialog accessible from the
   View->Preferences option in the main menu.



For each of these kinds of errors, the user can choose to handle them by
raising the error and stopping the wrapper execution, or ignoring the
error and continuing with the wrapper execution. Besides, one or more of
the following actions can be configured to be executed before raising or
ignoring the error:

-  *Trace record*: when the error occurs, the specified action record
   will be traced in the wrapper execution plan (these errors are
   accessible via the ITPilot API, see :doc:`/itpilot/developer/index`).
-  *Output record*: when the error occurs, the specified action record
   will be added to the wrapper output.
-  *Retry*: when the error occurs, the system tries to execute the
   component again. The maximum number of retries is configured in the
   component wizard. If the error persists it will be raised or ignored
   according to the error configuration.

For this example, all of the values will be kept as ‘raise error’,
indicating that any error is found, it will be reported and the
execution stopped.

.. figure:: DenodoITPilot.GenerationEnvironment-33.png
   :align: center
   :alt: Error handling dialog
   :name: Error handling dialog

   Error handling dialog





.. |image0| image:: DenodoITPilot.GenerationEnvironment-31.png
.. |image1| image:: DenodoITPilot.GenerationEnvironment-32.png

