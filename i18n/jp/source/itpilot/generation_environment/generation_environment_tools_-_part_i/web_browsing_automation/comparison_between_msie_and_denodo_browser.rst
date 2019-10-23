==========================================
Comparison Between MSIE and Denodo Browser
==========================================

Denodo Browser is a GUI-less browser that embeds an HTTP client and a
JavaScript engine that allows it to perform complex web navigations.
When deciding what type of browser to use (Microsoft Internet Explorer
or Denodo Browser), the following factors must be taken into account:

#. Efficiency. Denodo Browser is more efficient than the MSIE browsers,
   since it is much lighter and allows greater customization options.
   This implies an improvement in the response time when accessing
   sources, and a decrease in the CPU load of the machine which houses
   the navigation system; this feature is important when several
   parallel executions are required.
#. Denodo Browser cannot execute some of the NSEQL navigation commands.
#. Denodo Browser does not interpret code written in VBScript.
#. In some pages, Denodo Browser’s JavaScript engine may process
   JavaScript code in a different way than MSIE do. In these cases, the
   desired behavior will typically be that of the MSIE, since very
   probably, the target pages have been designed to work correctly with
   this browser.

