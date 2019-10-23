==================================================
7.0 GA: New Features of the Embedded Web Container
==================================================

This section lists the new features of the web
container embedded with the Denodo Platform |version| GA. For the list of features included in the updates of Denodo |version|, check their RELEASE NOTES.

.. contents::
   :local:
   :backlinks: none

New Version of the Web Container: Apache Tomcat 8.5
======================================================================================

The embedded web container has been upgraded to Apache Tomcat 8.5. This version provides more stability, better performance and better
memory management than the version included in earlier versions of 
Denodo.

Future updates of Denodo may update the version of Tomcat to benefit from enhancements and security fixes. Find the exact version of Tomcat used in your installation in the file :file:`{<DENODO_HOME>}/apache-tomcat/RELEASE-NOTES`.

Allow URIs Slash and Backslash by Default
=========================================

.. #30227 Define the properties ALLOW_ENCODED_SLASH=true and ALLOW_BACKSLASH=true in the Denodo Tomcat to allow URIs with slash and backslash

The web container no longer returns an error when it receives a request to a URL that contains the characters %2F or %5C. They are the URL-encoded characters for "/" and "\\" respectively. To avoid this issue in previous versions, you have to add the system properties below to the configuration of the embedded web container:

::

   -Dorg.apache.tomcat.util.buf.UDecoder.ALLOW_ENCODED_SLASH=true 
   -Dorg.apache.catalina.connector.CoyoteAdapter.ALLOW_BACKSLASH=true

Others
======

.. #33737 CWE-757: Selection of Less-Secure Algorithm During Negotiation ('Algorithm Downgrade')

The web container is now configured to use TLS 1.2 for https connections. In addition, the container now ignores requests by clients to downgrade the encryption algorithm.

|

.. Web Container #31421 The Denodo tomcat logs this error to tomcat.log every time it starts: org.apache.catalina.core.StandardContext  - A context path must either be an empty string or start with a '/' and do not end with a '/'

The web container no longer logs this error when its starts:

.. code-block:: none

   org.apache.catalina.core.StandardContext  - A context path must either be an empty string or start with a '/' and do not end with a '/'

This is an error that can be ignored but by removing it, the logs are cleaner.

|

.. Web Container #32384 Each webapp should specify its own log4j.xml file from its deployed webapp, instead of the server's one
.. csantos@2018/03/09: QUESTION: ANY BENEFITS TO THIS?


.. Web Container #33004 Tomcat manager rises NPE in a misconfigured development environment
.. csantos@2018/03/09: Internal feature that only benefits developers of the Platform.


.. Web Container #35523 Change the 404 screen to adapt it to the new corporate image. 
.. csantos@2018/03/09: I do not think this is worth documenting


|

.. Web Container #31245 Increment the default size of the Tomcat's heap to 1g (currently is 512Mb) and the perm generation to 256 (currently 128)

The size of the memory heap assigned to the web container is now 1 gigabyte by default. In previous version is 512 megabytes by default.
