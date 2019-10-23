====================================================
Best Practices When Using the Integration with a VCS
====================================================

This section lists some best practices regarding the usage of VCS that fall outside the scope of the Denodo Platform:

.. contents::
   :local:
   :depth: 1
   :backlinks: none

Tagging and Branching Releases
==============================

We recommend setting up a process to create tags and branches in the
VCS, in order to manage the versions that a project goes through during
its life.

Consider the following ideas:


-  Add a tag to the repository every time you deploy a database to testing
   and production. This ensures that a project can be brought to the exact
   state it was at the time of a release so developers can track issues.


-  Create a branch for each major version of a project if several versions
   of the same project must run in production at the same time. This may be
   necessary when a new version of a project introduces changes that break
   the backward compatibility and client applications cannot be updated in
   time.


-  If there is no companywide standard regarding the names of branches and
   tags, a good naming convention for tags and branches is adding the
   version number to the end of the project. For example, if the project is
   called project1, the branches would be named project1-X.Y.Z and the
   tags, project1-vX.Y.Z (the v, indicates that the element is a version),
   where:

   -  X is the major version. Major versions are defined as versions that
      break backwards compatibility or that bring substantial changes to
      the project.
   -  Y is the minor version. Minor versions are versions that do not
      include big new features and that do not break the compatibility with
      client applications.
   -  Z is the revision number. Versions that have small changes such as
      the ones introduced to fix small issues in the views created,
      performance improvements, etc.


To tag and branch the different versions, you can use a client tool for
the VCS you are using (e.g. for GIT, you can use `SourceTree <https://www.sourcetreeapp.com/>`_), check out
a copy of the repository where the database of the project is on your
local machine and add the tag/branch. 

Recommendations for the Testing Environment
===========================================

When possible, the testing environment should be similar to the
production environment. That is:

-  The configuration of the hardware/virtual machine where the Denodo
   servers run should be similar.
-  The data used should be representative of the data that will be found
   in production.
-  The performance of the data sources and network should be similar.

These guidelines ensure that the conclusions drawn from the tests
performed on the testing environment will apply to the production
environment.
