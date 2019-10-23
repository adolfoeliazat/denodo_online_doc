===================================
Configuring the Verification Server
===================================

.. toctree::
   :hidden:
   
   server_configuration_verification_server.rst
   database_configuration.rst
   mail_configuration.rst
   rules_configuration.rst

Denodo ITPilot offers a component for automatic verification of wrappers
that detects changes in the web sources and sends notifications by email
when the wrappers stop working properly. The main concept on which this
component is based is the storage of the results of successful queries
of the wrappers. These stored queries are used by a set of configurable
rules to detect invalid results in wrappers that are currently running,
indicating a possible change in the web source.



This component is made available through the Verification Server, whose
configuration process through the Web administration tool is detailed in
this section.

.. important:: The ITPilot Verification Server has been deprecated and may be removed in the next version of the Denodo Platform.

In the web administration tool, the configuration options for Wrapper
Servers are split across four sections: “Server Configuration”,
“Database Configuration”, “Mail Configuration” and “Rules
Configuration”.


