================
Tool Preferences
================

To change the settings of the Administration Tool, click **Tools** >
**Admin Tool** **Preferences**.

You can change settings related to:

-  Authentication: see section `Authentication`_.
-  Locale: see section `Locale`_.
-  Connection: see section `Connection Parameters`_.
-  Version Control System (VCS): see section `VCS Configuration`_.


Authentication
=================================================================================

In this wizard you configure if you want the Administration Tool to
login using the standard authentication or the Kerberos authentication.

The section :ref:`Configuring the Administration Tool to Use Kerberos
Authentication` explains how to configure the Kerberos authentication
in the Administration Tool.

Locale
=================================================================================

The Locale configuration of the Administration Tool controls the
appearance of data that depends on the user’s locale.

.. note:: This preference does not change the internationalization
   preferences of the Virtual DataPort server. See section :ref:`Configuring the
   Default Internationalization` for information about changing this
   preference of the server.

The Administration Tool is a client of the Virtual DataPort server and
as any other client, it has its own i18n settings. When you execute a
query from the “Execute” dialog of the view, the Tool automatically adds
the ``CONTEXT`` clause with the i18n of the Tool. I.e.:
``CONTEXT ("i18n" = 'us_pst')``

The queries executed from the VQL Shell Tool are sent to the Server
unmodified so they will be executed with the i18n of the database.

If you select the **Internationalize query results** check box, the
results of the queries executed from the administration tool will be internationalized. That is, in the values of type float, double and decimal, the decimal
separator used will be the one specified in the i18n of the query, database or server.

Unlike in previous versions of Denodo, datetime values are always displayed with the same format, regardless of the 
setting **Internationalize query results** or the language settings of the 
computer where the administration tool runs. If you want a query to display a datetime value with a different format, use the function :ref:`FORMATDATE`.

   
.. Commented by csantos@2019/03/21: I leave this for internal references but it provides a lot of detail that the user can see by herself.

   -  "yyyy-MM-dd" (<year>-<month>-<day>) for values with type ``localdate`` and values with type ``date (deprecated)`` with subtype ``DATE``. For example: "2018-12-31".
   -  "HH:mm:ss.S" (<hour>:<minute>:<second>.<millisecond>) for values with type ``time`` and values with type ``date (deprecated)`` with subtype ``TIME``. For example: "23:15:05.15673".
   -  "yyyy-MM-dd HH:mm:ss.S" (<date> <time>) for values with type ``timestamp``. For example, "2018-12-31 23:58:05.15673".
   -  "yyyy-MM-dd HH:mm:ss.S XXX" (<date> <time> <time zone>) for values with type ``timestamptz`` and values with type ``date (deprecated)`` with subtype ``TIMESTAMP``. For example: "2018-12-31 23:59:05.15673 -08:00". The time zone will be the specified in the query i18n.
         


Connection Parameters
=================================================================================

In this dialog, you can configure the following parameters of the
connection between the Administration Tool and the Virtual DataPort
server:

-  **Query timeout (milliseconds)**: Maximum time the Administration Tool
   will wait for a query to finish. If zero, it will wait indefinitely
   until the sentence ends.


-  **Chunk Timeout (milliseconds)**: It establishes the maximum time that
   the Server waits before returning a new block to the Tool. When this
   time is surpassed, the Server sends the current block even if it does
   not contain the number of results specified in the *Chunk Size*
   parameter.
   
   .. note:: If *Chunk size* and *Chunk timeout* are zero, the Server
      returns all the results in a single block. If both values are different
      from 0, the Server returns a chunk whenever one of these conditions
      happen:

      -  The chunk is filled (*Chunk size*)
      -  Or, after a certain time of not sending any chunk to the client
         (*Chunk timeout*).


-  **Chunk size (rows)**: The results obtained by executing a sentence can
   be divided into blocks (chunks), so the Virtual DataPort Server does not
   have to wait until a sentence ends, to send the processed tuples to the
   Tool.
   
   This parameter establishes the maximum number of results that a block
   can contain.
   
   When the Server obtains enough results to complete a block, it sends
   this block to the client and continues processing the next results.

-  **User agent**: Virtual DataPort publishes several JMX MBeans that
   provide information about the connection and queries executed by each
   client. Among others, these MBean publish the attribute “user agent”.
   When this administration tool connects to Virtual DataPort, the user
   agent reported by the MBeans will be the value entered in this box.

   See more about the JMX MBeans published by Virtual DataPort in the
   section :ref:`Monitoring with a Java Management Extensions (JMX) Agent`.


-  **Maximum number of active connections**: maximum number of active
   connections opened by the Tool.

-  **Maximum waiting time to obtain a connection**: maximum time in
   milliseconds the Tool will wait for a connection before showing an
   error. If ``-1``, the Tool will wait indefinitely.
   
-  **Use WAN optimized communications**: If selected, the communications
   with the server will be optimized for high latency networks. May 
   reduce performance if used in LAN environments.

-  **Use only SSL communications**: If selected, the communication with
   the server will be forced to be secured. If SSL is disabled on the Virtual DataPort server,
   the communication will fail.

VCS Configuration
=================================================================================

In this dialog, you can configure the VCS settings of the Administration
Tool. The section :ref:`VCS Settings of the Administration Tool` explains
the options of this dialog.
