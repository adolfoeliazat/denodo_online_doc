===================================
Configuring the Browser Pool Server
===================================

.. toctree::
   :hidden:
   
   global_configuration_parameters.rst
   msie_and_denodo_browser_configuration.rst

Wrappers being executed in a Wrapper Server need to have access to a
Browser Pool Server in order to run their NSEQL navigation sequences.



The Denodo Browser Pool Server supports two different browser types:

-  MSIE Browser: browsers based on Microsoft Internet Explorer.
-  Denodo Browser: a GUI-less, HTTP-based browser with JavaScript
   support.



The Denodo Browser embeds a JavaScript engine, which allows it to
perform complex navigations in several web sources. When deciding what
type of browsing tool to use (a MSIE browser or a Denodo browser) to
extract information from a web source, the following factors must be
taken into account:

#. Efficiency. The Denodo Browser is more efficient than the MSIE
   browsers, since it is lighter. This implies an increase in the
   response time when accessing sources, and a decrease in the CPU load
   of the machine which houses the Browser Pool; this feature is very
   important when several parallel executions are required.
#. The Denodo Browser cannot execute some of the NSEQL navigation
   commands.
#. The Denodo Browser does not interpret code written in VBScript.
#. In some pages, ITPilot’s JavaScript engine may process JavaScript
   code in a different way than Internet Explorer do. This is because
   these browsers’ interpreters might be laxer regarding the syntax used
   by the web pages. In these cases, the desired behavior will be that
   of Internet Explorer, since very probably, the target pages have been
   designed to work correctly with this browser.



The browser type to use for each browsing sequence is defined at wrapper
generation time: it can be set to a specific browser type or it can be
set to use whatever type of browser is configured as the default in the
Browser Pool Server.



The MSIE browsers in the Browser Pool Server will use the configuration
established for Microsoft Internet Explorer and in the system in which
the server is executed.



It is also necessary to consider the security options and cookies, as
the pool browsers will act according to said configuration.



In the administration tool, the configuration options for Browser Pool
Servers are split across three configuration sections. On each section,
configuration options are grouped in a set of forms. To change a
parameter on any of these groups, click the edit button (|image0|)
corresponding to that group to make the form enter in edition mode.



The “Global configuration parameters” section is for parameters like the
default browser type which will be used by the server, port assignments,
and HTML conversion options.



The two remaining sections are for configuring each supported browser
type and its associated pool. They have similar parameters, dealing with
pool sizes and browser reutilization policies, and also with timeouts,
download controls, and proxies. Denodo browsers lack some of the options
that can be configured for MSIE.

.. |image0| image:: DenodoITPilot.UserGuide-15.png

