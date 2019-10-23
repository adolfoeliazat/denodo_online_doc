******************
License Management
******************

.. toctree::
   :hidden:

   install_license/install_license.rst
   assign_license_to_environment/assign_license_to_environment.rst
   assign_license_to_server/assign_license_to_server.rst
   how_licenses_work/how_licenses_work.rst
   license_usages_table/license_usages_table.rst

In order for a Denodo server to run, you have to make sure that it has access to
a Denodo license file. The usual way to configure a license in a Denodo platform
was to :ref:`specify it during the installation <configure-local-license-installation>`,
to :ref:`copy it manually on the machine <install-license>` or to
:ref:`install it through the Control Center <control-center-help>`. When your
organization has a large Denodo deployment, installing a new license may be a
cumbersome task, since it has to be performed on every machine of your Denodo
infrastructure. The Solution Manager provides an alternative way of managing all
your licenses with the introduction of the Denodo global license and the Denodo
License Manager.

A **Denodo global license** is a multi-license file that defines different
license scenarios. Each one of these scenarios enables distinct features and
restrictions in order to meet different requirements. For example, a scenario can
enable some products and another scenario can limit the maximum number of
concurrent requests. In the Solution Manager, each environment should have a
license scenario assigned. Common license scenarios are:

- *production*, for the final production system;

- *staging*, for the pre-production system;

- and *development*, for the developer machine.

The **Denodo License Manager** is a centralized place where Denodo servers can
obtain their license from and renew it periodically. The License Manager has a
global license installed and grants licenses to Denodo servers according to
which scenario is assigned on the environment they belong to.

.. note:: Only global administrators and Solution Manager
          administrators can configure licenses in the Solution Manager. More
          information is available in the :ref:`Authorization` section.

Along this chapter you will learn to use the Solution Manager Administration
Tool and the License Manager to work with licenses on large Denodo deployments.
More specifically, it describes how:

- :ref:`To install a global license <sm_install_license>`.

- :ref:`To assign licenses scenarios <sm_assign_license_to_envitonments>` to environments.

- :ref:`To assign custom licenses scenarios <sm_assign_license_to_servers>` to servers.

- :ref:`Licenses work <sm-licenses-work>`.

- To view the :ref:`license usages <sm_license_usages_table>` of the Denodo servers.