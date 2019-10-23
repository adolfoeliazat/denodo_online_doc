============================
Verification Environment
============================

The Verification Environment adds functionality on top of the Execution
Environment, and complements it by verifying the deployed wrappers
operation, and reducing the manual effort. As Web sources are autonomous
and independent of the wrappers, they can change over time, and these
changes can invalidate the current access mode, whereby the wrappers no
longer extract the data properly. Denodo ITPilot offers an automated
verification tool that detects the changes referred to above. Although
this will be dealt with in more depth in section :ref:`Configuring the
Verification server`, its basic functioning is as follows:

-  The Wrapper Server stores all the wrappers in each of the Web
   sources.
-  The system periodically checks each wrapper for changes, monitoring
   the results of the queries executed against them.
-  When a change is detected in a source, the system sends a
   notification of the change.



The components of this environment, apart from those already mentioned
in the execution environment, are as follows:

-  Verification Server: component responsible for detecting
   automatically any change happened in the sources and for notifying
   those changes. It communicates with the Wrapper Server to request all
   the wrappers to verify, and to obtain the query execution results
   over them (which will be used to check possible changes).



As mentioned earlier, a detailed explanation of this environment is
provided in section :ref:`Distribution of the Verification Environment` of
this same manual.

.. important:: The ITPilot Verification Server has been deprecated and may be removed in the next version of the Denodo Platform.

The next section recommends different distribution architectures for
these components. Chapter :ref:`Installation and Initial Configuration`
gives details of the installation and configuration processes for each
of the ITPilot environments.









