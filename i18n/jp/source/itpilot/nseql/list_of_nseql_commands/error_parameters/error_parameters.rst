================
Error Parameters
================

When a NSEQL command has not been able to execute adequately, it will
return an error code regardless of the type of command it is.
Specifically, the possible values are the following:

#. ``WRAPPER_ERROR`` (-1): error in the sequence definition (e.g. an
   href HTML tag is not found in the execution of a ClickOnAnchorByHref
   command). Whenever the error is not related to a network or server
   problem, ITPilot will return a ``WRAPPER_ERROR``.
#. ``DOWNLOAD_TIMEOUT`` (-2): when accessing a web resource, the maximum
   waiting time has expired, as it had been configured in the browser
   pool.
#. ``DOWNLOAD_ERROR`` (-3): there has been some type of error when
   downloading information from the web.
#. ``CONNECTION_ERROR`` (-4): the connection with the web resource has
   not been possible.
#. ``HTTP_ERROR`` (-5): the web server has returned an HTTP error code.
