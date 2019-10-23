===========================================
Commands for Editing the Browser Properties
===========================================

List of commands for editing the browser properties:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

.. _nseql_guide_set_ambient_properties:


SETAMBIENTPROPERTIES
==========================================

Establishes the properties that control the elements that the active
browser downloads and executes.

**Parameters**

.. code-block:: bnf

   SetAmbientProperties(images, videos, bgSounds, noScripts, noJava,
   noActiveX, noVisualization, noCache, noProxyCache [, noRunActivex,
   noBehaviors ] )

All the parameters can take the value 0 (indicates a negative value) or
1 (indicates a positive value).

-  int images: download images.
-  int videos: download videos.
-  int bgSounds: download sounds.
-  int noScript: do not execute scripts.
-  int noJava: do not execute applets.
-  int noActiveX: do not download ActiveX controls.
-  int noVisualization: the page is downloaded but not viewed.
-  int noCache: the cache files are ignored and are requested from the
   server. If this indicates that the cached files are up to date, they
   are used.
-  int noProxyCache: the request is forced through the server, ignoring
   the cache of the proxy, even if it contains the data used.
-  int noRunActiveX: optional. ActiveX controls are not executed.
-  int noBehaviors: optional. Behaviors are not downloaded and are
   disabled in the document.

**Returns**

   Nothing.

.. note:: When using MSIE Browser this command will only have effect if
   it is executed before the browser performs any navigation. After that,
   the *setAmbientProperties* command will be ignored.

.. _nseql_guide_setproxyauthinfo:

SETPROXYAUTHINFO
==========================================

Allows the active browser to be configured with preferences to browse
through authenticated *proxies*. The system finds a *proxy*
authentication window and automatically completes its fields, where
necessary.

**Parameters**

-  String login: user name in the proxy. Some web proxies require the
   name of the domain to be specified together with the login using the
   format domain\\login or domain/login (in these cases the parameter
   domain must be left empty).
-  String password: password in the proxy. The value of this parameter
   can be encrypted, using the prefix ``encrypted:`` (for example
   ``encrypted:/QBQilPalsqjQpGOj4Ek8Q==``). To obtain the encrypted value,
   the Generation ToolBar can be used to record
   the command, or the encryption facilities of the Sequence Editor in
   the Wrapper Generator Tool can be used to
   encrypt a clear value.
-  String domain: domain in the proxy.

**Returns**

   (boolean): *true* if the command is successful, otherwise *false*.

.. important:: The symbols ``@`` and ``\\`` must
   be escaped with ``\\@`` and ``\\\\\\\\``.

.. note:: The proxy settings are different if you are using MSIE Browser
   or Denodo Browser, and if the proxy needs authentication or not:

-  If you are using MSIE:

   -  The proxy settings (host and port) must be configured in the settings
      dialog of the browser (not from the Denodo options). To access it,
      open the Connections tab of the Tools > Internet Options dialog of
      Microsoft Internet Explorer. You can configure the proxy using the
      LAN settings button.
      
      If the MSIE browser is not properly configured to browse through the proxy, ITPilot will ignore the setProxyAuthInfo command.

      Please notice that this will configure all the wrappers to use the proxy for the browsing, because it is configured in the browser. If you need to use it only for one wrapper, the use of Denodo Browser is recommended if possible (see below).

   -  If the proxy is authenticated, the login/password/domain settings can
      be filled either in the Options dialog of the Denodo toolbar or
      manually adding a setProxyAuthInfo command to the sequence.

-  If you are using Denodo Browser:

   -  Denodo Browser does not acknowledge the settings of MSIE Browser, but
      accepts a format in the ``setProxyAuthInfo`` command that includes
      the host and port. Use the following syntax

         ``SetProxyAuthInfo(user\\@host:port,passwd,_NULL_)`` (if the proxy is authenticated)

      or

         ``SetProxyAuthInfo(host:port,_NULL_,_NULL_)`` (if the proxy is not authenticated)

      For example, for an authenticated proxy: ``setProxyAuthInfo(demo-user\\@denodo-proxy:80, mypassword, mydomain)``


.. note:: There are different HTTP authentication methods. The command
   setProxyAuthInfo supports the following: BASIC, DIGEST and NTLM. In the
   basic form:

SetProxyAuthInfo (user,password,)

The command will try DIGEST first and BASIC if the first fails. In the
form:

SetProxyAuthInfo(user,password,WORKGROUP);

The command will use NTLM first and DIGEST in that order. Finally, the
method can be specified manually:

SetProxyAuthInfo(user,password,method:WORKGROUP);

Where method is one of BASIC, DIGEST or NTLM. For example

SetProxyAuthInfo(user,password,NTLM:WORKGROUP);


SETUSERAGENT
==========================================

Allows to set the value of the header User-Agent sent by the browser
during the current session.

**Parameters**

-  String useragent: the value to be sent in the User-Agent header. The
   value ``\_NULL\_`` can be used to restore the default user agent value
   of the browser.