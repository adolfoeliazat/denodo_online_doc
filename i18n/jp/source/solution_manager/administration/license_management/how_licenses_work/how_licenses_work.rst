
.. _sm-licenses-work:

*****************
How Licenses Work
*****************

There are four kind of servers in the Denodo Platform that require a license to
work: Virtual DataPort, Scheduler, ITPilot Browser Pool and ITPilot Verification
servers. To begin working, they request a license at boot time. Moreover, they
have to periodically send a license renewal request in order to check that their
license is still valid.

This section will explain in detail how a server requests its license and how it
behaves according to whether there is a valid license or not.

How a Server Resolves its License
=================================

First, every Denodo server looks for a local file with its license. For previous
versions of the Denodo Platform, this was the only way to retrieve the license.
To configure a local license, you have to:

* specify it :ref:`during the installation <configure-local-license-installation>`,

* :ref:`copy it manually <install-license>` on the machine, or

* install it :ref:`through the Control Center <control-center-help>`.

If there is no local license, the Denodo server will request its license to the
Denodo License Manager. In order for the License Manager to answer properly the
license requests, your configuration should meet the following prerequisites:

#. In the Denodo Platform of the server that needs a license, configure properly
   how to access the License Manager. You can do this
   :ref:`during the installation <configure-license-manager-installation>` or
   :ref:`in the Control Center <control-center-configuration>`.

#. Make sure the :ref:`License Manager Server is running <Launching the License Manager Server>`.

#. :ref:`Install a global license <sm_install_license>` in the License Manager.
   
#. In the Solution Manager Administration Tool, define your
   :ref:`environments <sm-creating-environments>`,
   :ref:`clusters <sm_creating_clusters>` and
   :ref:`servers <sm_creating_servers>` infrastructure. Pay special attention to
   the **Host** and **Port** fields on the server definition, since they will be
   used in the server resolution process.

#. :ref:`Assign a license scenario <sm_assign_license_to_envitonments>` to
   those environments with servers that need a license through the License
   Manager.

When a Denodo server needs to check its license information, it sends a license
request to the License Manager that is configured in its Denodo Platform. Among
other things, the request includes:

* the **IP addresses and hostnames** of the machine where the server resides,
  and

* the **port** where it listens for incoming requests.

The License Manager looks in its catalog for a server whose **Host** and
**Port** values match with those in the request. On a match, it retrieves the
environment that contains the server and resolves its license according to the
license scenario it has configured. The License Manager now validates if all the
license requirements are met. If everything is fine, it responds with the server
license. Otherwise, it responds with an error.

.. note:: The section :ref:`License Usages Table` explains how to monitor
          license requests and usages.

How Denodo Servers Behave on License Responses
==============================================

When a Denodo server component starts, it
requests its license as explained before and revalidates it. If the validation
is successful, the server runs normally. If there is any problem during this
process, the server will shut down.

Besides the initial license request, the Denodo servers periodically
send license requests to check that the license is still valid or to update its
features or requirements, in case they have changed in the meantime. Again, if
the license requirements are still met the server will work as usual. Otherwise,
the server will stop listening for new incoming requests, but will continue to
request periodically its license, waiting for the moment when the license
becomes valid again.

There is a special case to consider here. If the server is running and tries to
retrieve the license again, but there is no response because of a network
problem, then it will enter the grace period. The **grace period** is a period
of time in which the server can still work normally, despite not having a valid
license.

The grace period lasts for five days. When the grace period expires, if the server was not able to get a
valid license, it will stop listening for new incoming requests. At all times,
the server keeps requesting its license periodically.