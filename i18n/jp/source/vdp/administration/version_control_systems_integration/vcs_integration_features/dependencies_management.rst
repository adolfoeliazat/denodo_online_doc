=======================
Dependencies Management
=======================

The dependencies of elements under version control are automatically
managed by Virtual DataPort when performing check in or check out
operations (i.e. when checking in or out an element, all of its
dependencies are also checked in or out if they need to). For example, let us say you have a view "customer" and a selection view over it called "important_customer". If you modify both and then, you select to "important_customer" to commit, "customer" will be automatically added to the commit as well.

Global Elements Management
=================================================================================

Elements from a database can depend on global elements (i.e. elements
that do not belong to a single database). The global elements that can be versioned are i18n maps and jar extensions.

These elements are versioned separately from the elements that belong to a database and you have to use a different wizard, the *Global Elements* wizard. To open it, right-click the database > **VCS** > **Global elements**. This wizard is only available to administrators.

From this dialog you can check in and check out global elements. In addition, if at the moment the database is synchronized (i.e. there are not modified elements in it), the dialog will show the button **Revert** that allows you to revert to a previous version of these elements.

When developers use the dialog to check in/out elements of a database (data sources, views, etc.), the check in/out will never include modifications to global elements, even if they have been modified. 
  
  .. figure:: DenodoVirtualDataPort.AdministrationGuide-VCS-GlobalElements.png
     :align: center
     :alt: Global Elements management wizard
