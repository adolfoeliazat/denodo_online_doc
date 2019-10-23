======================
Use of the Ping Script
======================

The script ``ping`` checks that a Scheduler server is “alive”. It is
available in the ``tools/scheduler`` directory of the platform. It is
provided in two versions: ``ping.sh`` (for Linux systems) and
``ping.bat`` (for Windows systems).

 

Its syntax is the following:

.. code-block:: bnf
   :name: Syntax of the ping script
   :caption: Syntax of the ping script

   ping [-t timeout] [-h host] [-p port] [-l login] [-P password] [-a authType] [-v]


-  ``-t``. Optional. Time to wait for a response, in milliseconds. If
   after this period the script does not receive a response, it returns
   an error.
-  ``-h``. Optional. Host where the Server is running. If not present,
   the script sends the request to the default Scheduler host:
   ``localhost``.
-  ``-p``. Optional. Port where the Server is running. If not present,
   the script sends the request to the default Scheduler port: ``8000``.
-  ``-l``. Optional. User login for the Server. If not present, the
   script uses the default Scheduler local admin user: ``admin.``
-  ``-P``. Optional. User password for the Server. If not present, the
   script uses the default Scheduler local admin password: ``admin``.
   You can provide the password encrypted by prefixing it with
   ``encrypted:``. (Use the ``encrypt_password`` script to encrypt the
   password).
-  ``-a``. Optional. Authentication type: ``local`` or ``vdp``. If not
   present, the script uses ``local`` authentication.
-  ``-v``. Optional. If present, the script displays the status and the
   time taken to receive a response from the Server.

 

The ping script returns ``0`` if the status check is successful.
Otherwise, it returns ``1``.

An example of running the ping command is shown below:

 

**Example 1**:

.. code-block:: bash

   ping -t 1000 -v

Sends a ping request to ``localhost`` and port ``8000`` in verbose mode
with a timeout of 1 second

 

**Example 2**:

.. code-block:: bash

   ping myMachine

Sends a ping request to ``myMachine``. As the port is not set, the
script sends the request to the default port: ``8000``.

 

**Example 3**:

.. code-block:: bash
   
   ping -t 9000 -l admin -P encrypted:UjOsIu8972jviqGpcLP3Mg== -h localhost -p 5999

Sends a ping request to ``localhost`` and port ``5999`` with a timeout
of 9 seconds. The password was encrypted by executing this command:
``encrypt_password.bat mypassword``
