==============================================================
Best Practices: Deployment of Updates Across Your Organization
==============================================================

This section explains the best practices regarding how to deploy updates of the Solution Manager and the Denodo Platform installations, across your organization.

The updates of the Solution Manager and the Denodo Platform should be installed in this order:

1. The Solution Manager. Note that, because of the forward and backward compatibility between the Denodo Platform servers and the Solution Manager, you do not necessarily need to install the update. See more about this on :ref:`Solution Manager Compatibility with the Denodo Platform Servers`.
   
   Although it is not mandatory, it is always a good practice to have the Solution Manager up-to-date to benefit from the enhancements and bugfixes included in the update.
   
   If the Solution Manager is running on a high availability architecture, follow the steps of the section 
   below (:ref:`Updating Solution Manager with High Availability`).
   
   .. important:: Running the Solution Manager without any updates or running any Denodo Platform installation without any updates is an unsupported configuration.

#. Install the update of the Denodo Platform on the development environments.

#. Install the update of the Denodo Platform on the testing/staging environments.

#. Install the update of the Denodo Platform on the production environments.

After installing an update of the Denodo Platform on an environment, do the appropriate tests on that environment, before installing the update on the following environment.

The section :doc:`Installing Updates and Hotfixes </platform/installation/installing_updates_and_hotfixes/installing_updates_and_hotfixes>` of the Installation Guide explains how to install an update.

Updating Solution Manager with High Availability
==========================================================

The License Manager can be configured to run on an *active-passive* configuration for :ref:`high availability <High Availability>`.

This section explains how to install an update/hotfix for the Solution Manager and achieve zero downtime for the License Manager. This is only about the License Manager because the Solution Manager server and its administration tool cannot run on a cluster.

Follow these steps:

1. In the load balancer, disable the License Manager of the secondary node.

#. On the secondary node, stop the License Manager (this should be the only component of the Solution Manager running on the secondary node). At this point, the primary License Manager keeps processing all the license requests, as usual.

#. On the secondary node, install the update/hotfix and start the License Manager.

#. On a browser, open the URL \http://{host name of the secondary node}:10091/pingLicenseManager.

   If the page shows the message "OK", the License Manager has started successfully.

   If it does not show "Ok", do not continue. First, find out where the error is.

#. In the load balancer, enable the secondary node. At this point, the License Manager of both nodes is running.

#. In the load balancer, disable the License Manager of the primary node. At this point, the secondary License Manager will process all the license requests.

#. In the primary node, stop all the components of the Solution Manager.

#. On the primary node, install the update/hotfix and start *only* the License Manager. Once it is started, start the other components of the Solution Manager.

#. In the load balancer, enable the primary node.
