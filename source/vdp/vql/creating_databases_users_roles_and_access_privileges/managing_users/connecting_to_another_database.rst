==============================
Connecting to Another Database
==============================

When you are connected to a database, you can connect to other databases
or use different credentials to execute tasks that with your
current user, you are not allowed to. To do this, use the commands
``CONNECT`` and ``CLOSE``.


.. code-block:: bnf
   :caption: Syntax of the CONNECT statement
   :name: Syntax of the CONNECT statement

   CONNECT
   [
         { USER <name:identifier> PASSWORD <password:literal>
       | TOKEN <token:literal> }
   ]
   [ DATABASE <name:identifier> ]


The ``CONNECT`` command allows indicating either a user name (``USER``)
and password (``PASSWORD``) or a Kerberos token, to initiate a new
session in the server with a new user account. A session may also be
initiated with a new database (with the current user or another user).

If you indicate the credentials but not ``DATABASE``, this command opens a session to the database you are already connected to, with the credentials provided. 

The ``CLOSE`` command allows the previous session to be reestablished
after having established a new session with the ``CONNECT`` command.